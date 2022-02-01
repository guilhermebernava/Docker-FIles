# https://hub.docker.com/_/microsoft-dotnet
FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /source

# copy csproj and restore as distinct layers
COPY *.sln .
COPY Qodeless.WebApi/*.csproj ./Qodeless.WebApi/
COPY Qodeless.Services/*.csproj ./Qodeless.Services/
COPY Qodeless.PrimePix.Domain/*.csproj ./Qodeless.PrimePix.Domain/
COPY Qodeless.Infra.CrossCutting.IoC/*.csproj ./Qodeless.Infra.CrossCutting.IoC/
COPY Qodeless.Infra.CrossCutting.Identity/*.csproj ./Qodeless.Infra.CrossCutting.Identity/
COPY Qodeless.DataManager/*.csproj ./Qodeless.DataManager/
RUN dotnet restore

# copy everything else and build app
COPY Qodeless.WebApi/. ./Qodeless.WebApi/
COPY Qodeless.Services/. ./Qodeless.Services/
COPY Qodeless.PrimePix.Domain/. ./Qodeless.PrimePix.Domain/
COPY Qodeless.Infra.CrossCutting.IoC/. ./Qodeless.Infra.CrossCutting.IoC/
COPY Qodeless.Infra.CrossCutting.Identity/. ./Qodeless.Infra.CrossCutting.Identity/
COPY Qodeless.DataManager/. ./Qodeless.DataManager/
WORKDIR /source/Qodeless.WebApi
RUN dotnet publish -c release -o /app --no-restore

# final stage/image
FROM mcr.microsoft.com/dotnet/aspnet:5.0
WORKDIR /app
COPY --from=build /app ./
ENTRYPOINT ["dotnet", "Qodeless.WebApi.dll"]