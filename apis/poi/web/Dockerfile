FROM microsoft/dotnet:2.1-sdk AS build-env
WORKDIR /app

# copy csproj and restore as distinct layers
COPY apis/poi/web/poi.csproj ./
RUN dotnet restore

# copy everything else and build
COPY apis/poi/ /app/
RUN dotnet publish -c Release -o out
RUN ls -la /app/*
# COPY app/web/appsettings.*.json /app/out/
# COPY app/web/appsettings.json /app/out/
COPY apis/poi/web/ /app/out/

# build runtime image
FROM microsoft/dotnet:2.1-aspnetcore-runtime
WORKDIR apis/poi/web/poi.csproj

ENV SQL_USER="YourUserName" \
SQL_PASSWORD="changeme" \
SQL_SERVER="changeme.database.windows.net" \
SQL_DBNAME="mydrivingDB" \
WEB_PORT="8080" \
WEB_SERVER_BASE_URI="http://0.0.0.0" \
ASPNETCORE_ENVIRONMENT="Production"

COPY --from=build-env /app/out .

ENTRYPOINT ["dotnet", "poi.dll"]

