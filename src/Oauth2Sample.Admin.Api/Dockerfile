FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["src/Oauth2Sample.Admin.Api/Oauth2Sample.Admin.Api.csproj", "src/Oauth2Sample.Admin.Api/"]
COPY ["src/Oauth2Sample.Admin.EntityFramework.Shared/Oauth2Sample.Admin.EntityFramework.Shared.csproj", "src/Oauth2Sample.Admin.EntityFramework.Shared/"]
COPY ["src/Oauth2Sample.Admin.EntityFramework.SqlServer/Oauth2Sample.Admin.EntityFramework.SqlServer.csproj", "src/Oauth2Sample.Admin.EntityFramework.SqlServer/"]
COPY ["src/Oauth2Sample.Shared/Oauth2Sample.Shared.csproj", "src/Oauth2Sample.Shared/"]
COPY ["src/Oauth2Sample.Admin.EntityFramework.PostgreSQL/Oauth2Sample.Admin.EntityFramework.PostgreSQL.csproj", "src/Oauth2Sample.Admin.EntityFramework.PostgreSQL/"]
COPY ["src/Oauth2Sample.Admin.EntityFramework.MySql/Oauth2Sample.Admin.EntityFramework.MySql.csproj", "src/Oauth2Sample.Admin.EntityFramework.MySql/"]
RUN dotnet restore "src/Oauth2Sample.Admin.Api/Oauth2Sample.Admin.Api.csproj"
COPY . .
WORKDIR "/src/src/Oauth2Sample.Admin.Api"
RUN dotnet build "Oauth2Sample.Admin.Api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Oauth2Sample.Admin.Api.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENV ASPNETCORE_FORWARDEDHEADERS_ENABLED=true
ENTRYPOINT ["dotnet", "Oauth2Sample.Admin.Api.dll"]