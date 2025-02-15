FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["src/API/ECommerce.API/ECommerce.API.csproj", "src/API/ECommerce.API/"]
COPY ["src/Core/ECommerce.Core/ECommerce.Core.csproj", "src/Core/ECommerce.Core/"]
COPY ["src/Adapters/ECommerce.Adapters/ECommerce.Adapters.csproj", "src/Adapters/ECommerce.Adapters/"]
RUN dotnet restore "src/API/ECommerce.API/ECommerce.API.csproj"
COPY . .
WORKDIR "/src/src/API/ECommerce.API"
RUN dotnet build "ECommerce.API.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "ECommerce.API.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ECommerce.API.dll"]
