#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

# estagio 1
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80

ENV ASPNETCORE_URLS=http://+:8000;http://+:80;
ENV ASPNETCORE_ENVIRONMENT=Development

# estagio 2
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["ApiDocker/ApiDocker.csproj", "ApiDocker/"]
RUN dotnet restore "ApiDocker/ApiDocker.csproj"
COPY . .
WORKDIR "/src/ApiDocker"
RUN dotnet build "ApiDocker.csproj" -c Release -o /app/build

# estagio 3
FROM build AS publish
RUN dotnet publish "ApiDocker.csproj" -c Release -o /app/publish

# estagio 4
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ApiDocker.dll"]