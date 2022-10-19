FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["src/Oauth2Sample.STS.Identity/Oauth2Sample.STS.Identity.csproj", "src/Oauth2Sample.STS.Identity/"]
COPY ["src/Oauth2Sample.Admin.EntityFramework.Shared/Oauth2Sample.Admin.EntityFramework.Shared.csproj", "src/Oauth2Sample.Admin.EntityFramework.Shared/"]
COPY ["src/Oauth2Sample.Shared/Oauth2Sample.Shared.csproj", "src/Oauth2Sample.Shared/"]
RUN dotnet restore "src/Oauth2Sample.STS.Identity/Oauth2Sample.STS.Identity.csproj"
COPY . .
WORKDIR "/src/src/Oauth2Sample.STS.Identity"
RUN dotnet build "Oauth2Sample.STS.Identity.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Oauth2Sample.STS.Identity.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENV ASPNETCORE_FORWARDEDHEADERS_ENABLED=true
ENTRYPOINT ["dotnet", "Oauth2Sample.STS.Identity.dll"]