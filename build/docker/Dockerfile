# -----------------------------
# Build stage
# -----------------------------
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Copy solution and restore
COPY sla-engine-backend.sln .
COPY src/Sla-engine-backend/*.csproj src/Sla-engine-backend/
RUN dotnet restore

# Copy remaining source
COPY src/ src/
WORKDIR /src/src/Sla-engine-backend

RUN dotnet publish -c Release -o /app/publish

# -----------------------------
# Runtime stage
# -----------------------------
FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app

COPY --from=build /app/publish .

EXPOSE 8080
ENTRYPOINT ["dotnet", "Sla-engine-backend.dll"]
