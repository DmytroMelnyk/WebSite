ARG CS_PROJ="WebSite"
FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS prerestore
COPY . .
RUN mkdir ./proj && for f in $(find . -name '*.csproj'); do \
  cp --parents $f ./proj/; done

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS publish
ARG CS_PROJ
WORKDIR /src
COPY --from=prerestore ./proj .
RUN dotnet restore "$CS_PROJ/$CS_PROJ.csproj"
COPY . .
WORKDIR "/src/$CS_PROJ"
RUN dotnet publish "$CS_PROJ.csproj" -c Release -o /app/publish --no-restore

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim
WORKDIR /app
COPY --from=publish /app/publish .
#EXPOSE 80
#ENTRYPOINT ["dotnet", "TelegramBot.dll"]
# heroku uses the following
CMD ASPNETCORE_URLS=http://*:$PORT dotnet TelegramBot.dll
