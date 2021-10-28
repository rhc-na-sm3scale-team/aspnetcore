#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

# Container we use for final publish
FROM mcr.microsoft.com/dotnet/core/aspnet:5.0-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

# Build container
FROM mcr.microsoft.com/dotnet/core/sdk:5.0-buster AS build

# Copy the code into the container
WORKDIR /src
COPY ["src/Com.RedHat/Com.RedHat.csproj", "Com.RedHat/"]

# NuGet restore
RUN dotnet restore "Com.RedHat/Com.RedHat.csproj"
COPY ["src/Com.RedHat/", "Com.RedHat/"]

# Build the API
WORKDIR "Com.RedHat"
RUN dotnet build "Com.RedHat.csproj" -c Release -o /app/build

# Publish it
FROM build AS publish
RUN dotnet publish "Com.RedHat.csproj" -c Release -o /app/publish

# Make the final image for publishing
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Com.RedHat.dll"]
