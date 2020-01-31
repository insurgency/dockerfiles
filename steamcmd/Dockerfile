FROM ubuntu

# Allow 32-bit software packages in order to install "steamcmd"
RUN dpkg --add-architecture i386 && apt-get update

# Accept Steam's installation terms of agreement
RUN echo "steam steam/question select I AGREE" | debconf-set-selections \
		&& echo "steam steam/license note" | debconf-set-selections

# Install steamcmd from APT
# See: https://developer.valvesoftware.com/wiki/SteamCMD#Package_from_repositories
RUN DEBIAN_FRONTEND=noninteractive apt-get -yq install steamcmd \
		# Needed to allow SteamCMD to make HTTPS connections
		ca-certificates \
		# ia32-libs are not required to install SteamCMD; lib32gcc1 is enough.
		lib32gcc1

# Symlink the steamcmd executable to a place that's in $PATH
RUN ln -sf /usr/games/steamcmd /usr/bin/steamcmd

# Create a new user that has a home directory in which ~/.steam/* files may be placed.
RUN useradd --create-home steam
# Switch to that previously created user
USER steam
# Automatically install any updates to steamcmd and quit
RUN steamcmd +quit

ENTRYPOINT ["steamcmd"]
