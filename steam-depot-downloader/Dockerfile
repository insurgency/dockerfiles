FROM mcr.microsoft.com/dotnet/core/sdk:2.2

RUN DEBIAN_FRONTEND=noninteractive apt-get update \
		&& apt-get install -y git
RUN git clone --depth 1 https://github.com/SteamRE/DepotDownloader.git /depot-downloader/
WORKDIR /depot-downloader/
RUN dotnet build DepotDownloader.sln
WORKDIR /depot-downloader/DepotDownloader/bin/Debug/netcoreapp2.0/

VOLUME [ "/downloads/" ]

ENTRYPOINT [ "dotnet", "./DepotDownloader.dll", "-validate", "-dir", "/downloads/", "-filelist", "/file_list.txt" ]
