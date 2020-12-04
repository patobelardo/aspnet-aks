#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat 
FROM mcr.microsoft.com/dotnet/framework/sdk:4.8 as build
WORKDIR /src
COPY . .
#RUN dir
RUN nuget restore SampleApp.sln
RUN msbuild SampleApp.sln /p:OutputPath=c:\published /p:PublishProfile=FolderProfile


FROM mcr.microsoft.com/dotnet/framework/aspnet:4.8-windowsservercore-ltsc2019
WORKDIR /inetpub/wwwroot
COPY --from=build /published/_PublishedWebsites/SampleApp .
RUN dir

