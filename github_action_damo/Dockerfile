# ==================== Build Stage ====================
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Copy project files
COPY ["github_action_damo/github_action_damo.csproj", "github_action_damo/"]
RUN dotnet restore "github_action_damo/github_action_damo.csproj"

# Copy source code
COPY . .

# Build and Publish
WORKDIR "/src/github_action_damo"
RUN dotnet publish "github_action_damo.csproj" -c Release -o /app/publish --no-restore

# ==================== Final Runtime Stage ====================
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS final

WORKDIR /app

# Copy published files from build stage
COPY --from=build /app/publish .

# Expose port
EXPOSE 8080

# Environment settings
ENV ASPNETCORE_URLS=http://+:8080
ENV ASPNETCORE_ENVIRONMENT=Production

# Run the application
ENTRYPOINT ["dotnet", "github_action_damo.dll"]