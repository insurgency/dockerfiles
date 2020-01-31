FROM mcr.microsoft.com/dotnet/core/sdk:latest

RUN apt-get update
RUN apt-get install -y git
RUN git clone --depth 1 https://github.com/SteamRE/DepotDownloader.git /depot-downloader/
WORKDIR /depot-downloader/
RUN dotnet build DepotDownloader.sln
WORKDIR /depot-downloader/DepotDownloader/bin/Debug/netcoreapp2.0/

VOLUME [ "/downloads/" ]

ENTRYPOINT [ "dotnet", "./DepotDownloader.dll", "-validate", "-dir", "/downloads/", "-filelist", "/file_list.txt" ]
