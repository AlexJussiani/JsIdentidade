#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["./Services/JsIdentidade/JS.Identidade.API/JS.Identidade.API.csproj", "Services/JsIdentidade/JS.Identidade.API/"]
COPY ["./Services/JsIdentidade/building blocks/JS.WebAPI.Core/JS.WebAPI.Core.csproj", "Services/JsIdentidade/building blocks/JS.WebAPI.Core/"]
COPY ["./Services/JsIdentidade/building blocks/JS.Core/JS.Core.csproj", "Services/JsIdentidade/building blocks/JS.Core/"]
COPY ["./Services/JsIdentidade/JS.Identidade.Domain/JS.Identidade.Domain.csproj", "Services/JsIdentidade/JS.Identidade.Domain/"]
COPY ["./Services/JsIdentidade/JS.Identidade.Infra/JS.Identidade.Infra.csproj", "Services/JsIdentidade/JS.Identidade.Infra/"]

RUN dotnet restore "Services/JsIdentidade/JS.Identidade.API/JS.Identidade.API.csproj"
COPY . .
WORKDIR "./Services/JsIdentidade/JS.Identidade.API"
RUN dotnet build "JS.Identidade.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "JS.Identidade.API.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "JS.Identidade.API.dll"]