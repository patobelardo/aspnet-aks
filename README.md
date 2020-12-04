# aspnet-aks
Quick demo about how to containerize an ASP.NET app (Framework) and deploy to aks

# First Step - Manual approach
## Build image

This is one example of a multi-stage Dockerfile

````Dockerfile
FROM mcr.microsoft.com/dotnet/framework/sdk:4.8 as build
WORKDIR /src
COPY . .
#RUN dir  #for debugging
RUN nuget restore SampleApp.sln
RUN msbuild SampleApp.sln /p:OutputPath=c:\published /p:PublishProfile=FolderProfile


FROM mcr.microsoft.com/dotnet/framework/aspnet:4.8-windowsservercore-ltsc2019
WORKDIR /inetpub/wwwroot
COPY --from=build /published/_PublishedWebsites/SampleApp .
#RUN dir   #for debugging
````

To build the image, we should be located at /src folder and run:

````bash
docker build -t localsampleapp .
````
>Once we need to push it to a registry, the tag should have the form of the container registry/repository, like **myregistry.azurecr.io/sampleapp:version1**

Once we built the image, we should see a localsampleapp image in our local image repository:

````bash
docker images
````
![](img/imageoutput.png)

## Test container

To test locally, we should run

````bash
docker run -p 88:80 localsampleapp:latest
````

After a moment, we should be able to access to localhost:port like in the following picture:
![](img/WebOnDocker.png)
