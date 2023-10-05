Reference: https://learn.microsoft.com/en-us/dotnet/core/docker/build-container?tabs=windows&pivots=dotnet-7-0

```
cd k8s-App-EKS
ls
cd k8s-App
ls
dotnet publish -c Release
```
```
k8s-App -> D:\GitHub\k8s-App-EKS\k8s-App\k8s-App\bin\Release\net6.0\k8s-App.dll
k8s-App -> D:\GitHub\k8s-App-EKS\k8s-App\k8s-App\bin\Release\net6.0\publish\
```
```
cd k8s-App
```
```
New-Item Dockerfile
```
```
code Dockerfile
```
Paste following Code to Dockerfile

```
FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build-env
WORKDIR /App

# Copy everything
COPY . ./
# Restore as distinct layers
RUN dotnet restore
# Build and publish a release
RUN dotnet publish -c Release -o out

# Build runtime image
FROM mcr.microsoft.com/dotnet/aspnet:7.0
WORKDIR /App
COPY --from=build-env /App/out .
ENTRYPOINT ["dotnet", "DotNet.Docker.dll"]
```

docker build -t counter-image -f Dockerfile .

