# conversao-peso
## Executar o docker build na raiz do projeto

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base </br>
WORKDIR /app </br>
EXPOSE 80 </br>
EXPOSE 443 </br>
</br>
FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build </br>
</br>
COPY ["ConversaoPeso.Web/ConversaoPeso.Web.csproj", "ConversaoPeso.Web/"] </br>
RUN dotnet restore "ConversaoPeso.Web/ConversaoPeso.Web.csproj"</br>
WORKDIR /src</br>
COPY . .</br>
WORKDIR "/src/ConversaoPeso.Web"</br>
RUN dotnet build "ConversaoPeso.Web.csproj" -c Release -o /app/build</br>
</br>
FROM build AS publish</br>
RUN dotnet publish "ConversaoPeso.Web.csproj" -c Release -o /app/publish</br>
</br>
FROM base AS final</br>
WORKDIR /app</br>
COPY --from=publish /app/publish .</br>
ENTRYPOINT ["dotnet", "ConversaoPeso.Web.dll"]</br>
