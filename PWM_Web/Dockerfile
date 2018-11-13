#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat

FROM microsoft/dotnet:2.1-aspnetcore-runtime-nanoserver-1803 AS base
WORKDIR /app
EXPOSE 53818
EXPOSE 44374

FROM microsoft/dotnet:2.1-sdk-nanoserver-1803 AS build
WORKDIR /src
COPY ["PWM_Web/PWM_Web.csproj", "PWM_Web/"]
RUN dotnet restore "PWM_Web/PWM_Web.csproj"
COPY . .
WORKDIR "/src/PWM_Web"
RUN dotnet build "PWM_Web.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "PWM_Web.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "PWM_Web.dll"]