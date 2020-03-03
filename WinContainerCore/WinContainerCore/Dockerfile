#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-nanoserver-1809 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-nanoserver-1809 AS build
WORKDIR /src
#COPY ["dotnetcorewc/dotnetcorewc.csproj", "dotnetcorewc/"]
#RUN dotnet restore "dotnetcorewc/dotnetcorewc.csproj"
COPY *.csproj ./
RUN dotnet restore "dotnetcorewc.csproj"

COPY . .
#WORKDIR "/src/dotnetcorewc"
WORKDIR "/src/"

RUN dotnet build "dotnetcorewc.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "dotnetcorewc.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "dotnetcorewc.dll"]

# Steps to change in the dockerfile
# 1. If deploying to Azure, change nanoserver ltsc to 1809 - (1903 is not supported)
# 2. COPY = *.csproj ./
# 3. RUN = dotnet restore "windowscontainercore.csproj"
# 4. WORKDIR = "/src"
#
