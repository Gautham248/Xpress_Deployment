# Base image for final container
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 80

# Build the app
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# Copy and restore
COPY ["Xpress_backend_V2/Xpress_backend_V2.csproj", "Xpress_backend_V2/"]
RUN dotnet restore "Xpress_backend_V2/Xpress_backend_V2.csproj"

# Copy everything else and build
COPY . .
WORKDIR "/src/Xpress_backend_V2"
RUN dotnet build "Xpress_backend_V2.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Publish the app
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "Xpress_backend_V2.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Final runtime image
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .

# ENTRYPOINT to run your app
ENTRYPOINT ["dotnet", "Xpress_backend_V2.dll"]
