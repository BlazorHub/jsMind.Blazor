#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["examples/jsMind.Server.App/jsMind.Server.App.csproj", "examples/jsMind.Server.App/"]
COPY ["src/jsMind.Blazor/jsMind.Blazor.csproj", "src/jsMind.Blazor/"]
RUN dotnet restore "examples/jsMind.Server.App/jsMind.Server.App.csproj"
COPY . .
WORKDIR "/src/examples/jsMind.Server.App"
RUN dotnet build "jsMind.Server.App.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "jsMind.Server.App.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "jsMind.Server.App.dll"]