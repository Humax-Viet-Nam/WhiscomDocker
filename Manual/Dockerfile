#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["Whiscom.API/Whiscom.API.csproj", "Whiscom.API/"]
COPY ["Whiscom.Application/Whiscom.Application.csproj", "Whiscom.Application/"]
COPY ["Whiscom.Infrastructure/Whiscom.Infrastructure.csproj", "Whiscom.Infrastructure/"]
COPY ["Whiscom.Domain/Whiscom.Domain.csproj", "Whiscom.Domain/"]
RUN dotnet restore "Whiscom.API/Whiscom.API.csproj"
COPY . .
WORKDIR "/src/Whiscom.API"
RUN dotnet build "Whiscom.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Whiscom.API.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Whiscom.API.dll"]