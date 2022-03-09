#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["Source/Presentation/OwenCloudInteraction.UI.BlazorWASM/Server/OwenCloudInteraction.UI.BlazorWASM.Server.csproj", "Source/Presentation/OwenCloudInteraction.UI.BlazorWASM/Server/"]
COPY ["Source/Presentation/OwenCloudInteraction.UI.BlazorWASM/Client/OwenCloudInteraction.UI.BlazorWASM.Client.csproj", "Source/Presentation/OwenCloudInteraction.UI.BlazorWASM/Client/"]
COPY ["Source/Presentation/OwenCloudInteraction.UI.BlazorWASM/Shared/OwenCloudInteraction.UI.BlazorWASM.Shared.csproj", "Source/Presentation/OwenCloudInteraction.UI.BlazorWASM/Shared/"]
RUN dotnet restore "Source/Presentation/OwenCloudInteraction.UI.BlazorWASM/Server/OwenCloudInteraction.UI.BlazorWASM.Server.csproj"
COPY . .
WORKDIR "/src/Source/Presentation/OwenCloudInteraction.UI.BlazorWASM/Server"
RUN dotnet build "OwenCloudInteraction.UI.BlazorWASM.Server.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "OwenCloudInteraction.UI.BlazorWASM.Server.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "OwenCloudInteraction.UI.BlazorWASM.Server.dll"]
