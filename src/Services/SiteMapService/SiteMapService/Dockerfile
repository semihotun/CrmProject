#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["src/Services/SiteMapService/SiteMapService/SiteMapService.csproj", "src/Services/SiteMapService/SiteMapService/"]
COPY ["src/BuildingBlocks/EventBus/EventBus.Factory/EventBus.Factory.csproj", "src/BuildingBlocks/EventBus/EventBus.Factory/"]
COPY ["src/BuildingBlocks/EventBus/EventBus.Base/EventBus.Base.csproj", "src/BuildingBlocks/EventBus/EventBus.Base/"]
COPY ["src/BuildingBlocks/EventBus/EventBus.RabbitMQ/EventBus.RabbitMQ.csproj", "src/BuildingBlocks/EventBus/EventBus.RabbitMQ/"]
COPY ["src/BuildingBlocks/Consul/ConsulConfig/ConsulConfig.csproj", "src/BuildingBlocks/Consul/ConsulConfig/"]
RUN dotnet restore "src/Services/SiteMapService/SiteMapService/SiteMapService.csproj"
COPY . .
WORKDIR "/src/src/Services/SiteMapService/SiteMapService"
RUN dotnet build "SiteMapService.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "SiteMapService.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "SiteMapService.dll"]
