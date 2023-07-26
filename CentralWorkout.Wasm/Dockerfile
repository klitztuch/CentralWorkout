FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["CentralWorkout.Wasm/CentralWorkout.Wasm.csproj", "CentralWorkout.Wasm/"]
RUN dotnet restore "CentralWorkout.Wasm/CentralWorkout.Wasm.csproj"
COPY . .
WORKDIR "/src/CentralWorkout.Wasm"
RUN dotnet build "CentralWorkout.Wasm.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "CentralWorkout.Wasm.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "CentralWorkout.Wasm.dll"]
