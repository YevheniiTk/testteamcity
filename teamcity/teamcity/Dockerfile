#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see http://aka.ms/containercompat 

FROM microsoft/dotnet:2.2-aspnetcore-runtime-nanoserver-1803 AS base
WORKDIR /app
ENV ASPNRTCORE_URLS THHP://+:83
EXPOSE 83
EXPOSE 443

FROM microsoft/dotnet:2.2-sdk-nanoserver-1803 AS build
WORKDIR /src
COPY teamcity/teamcity.csproj teamcity/
RUN dotnet restore teamcity/teamcity.csproj
COPY . .
WORKDIR /src/teamcity
RUN dotnet build teamcity.csproj -c Release -o /app

FROM build AS publish
RUN dotnet publish teamcity.csproj -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "teamcity.dll"]
