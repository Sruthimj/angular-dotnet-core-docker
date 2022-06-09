#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["Api/WebApi/WebApi.csproj", "WebApi/"]
COPY ["Api/Application/Application.csproj", "Application/"]
COPY ["Api/Domain/Domain.csproj", "Domain/"]
COPY ["Api/Infrastructure.Shared/Infrastructure.Shared.csproj", "Infrastructure.Shared/"]
COPY ["Api/Infrastructure.Identity/Infrastructure.Identity.csproj", "Infrastructure.Identity/"]
COPY ["Api/Infrastructure.Persistence/Infrastructure.Persistence.csproj", "Infrastructure.Persistence/"]
RUN dotnet restore "Api/WebApi/WebApi.csproj"
COPY . .
WORKDIR "/src/WebApi"

RUN dotnet build "WebApi.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "WebApi.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WebApi.dll"]
