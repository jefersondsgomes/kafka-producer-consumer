#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/runtime:6.0 AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["src/ConsumerWorker/ConsumerWorker.csproj", "src/ConsumerWorker/"]
RUN dotnet restore "src/ConsumerWorker/ConsumerWorker.csproj"
COPY . .
WORKDIR "/src/src/ConsumerWorker"
RUN dotnet build "ConsumerWorker.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "ConsumerWorker.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ConsumerWorker.dll"]