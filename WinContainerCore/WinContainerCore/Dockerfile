#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-nanoserver-1809 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-nanoserver-1809 AS build
WORKDIR /src
#COPY ["WinContainerCore/WinContainerCore.csproj", "WinContainerCore/"]
#RUN dotnet restore "WinContainerCore/WinContainerCore.csproj"
COPY *.csproj ./
RUN dotnet restore "WinContainerCore.csproj"

COPY . .
#WORKDIR "/src/WinContainerCore"
WORKDIR "/src/"

RUN dotnet build "WinContainerCore.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "WinContainerCore.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WinContainerCore.dll"]

# Steps to change in the dockerfile
# 1. If deploying to Azure, change nanoserver ltsc to 1809 - (1903 is not supported)
# 2. Copy needs to change. . .
# 3. WORKDIR needs to change to sRC
# 4. 