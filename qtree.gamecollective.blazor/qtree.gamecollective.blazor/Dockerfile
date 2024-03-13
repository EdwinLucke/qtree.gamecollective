#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["qtree.gamecollective.blazor/qtree.gamecollective.blazor/qtree.gamecollective.blazor.csproj", "qtree.gamecollective.blazor/qtree.gamecollective.blazor/"]
COPY ["qtree.gamecollective.blazor/qtree.gamecollective.blazor.Client/qtree.gamecollective.blazor.Client.csproj", "qtree.gamecollective.blazor/qtree.gamecollective.blazor.Client/"]
RUN dotnet restore "./qtree.gamecollective.blazor/qtree.gamecollective.blazor/qtree.gamecollective.blazor.csproj"
COPY . .
WORKDIR "/src/qtree.gamecollective.blazor/qtree.gamecollective.blazor"
RUN dotnet build "./qtree.gamecollective.blazor.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./qtree.gamecollective.blazor.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "qtree.gamecollective.blazor.dll"]