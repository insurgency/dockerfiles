FROM docker.pkg.github.com/insurgency/dockerfiles/steamcmd

ARG GAME_SLUG
ARG GAME_APPID

USER root
# srcds_run: "sh: 1: ifconfig: not found"
RUN apt-get install -yq net-tools
RUN mkdir /${GAME_SLUG}/ && chown -R steam:steam /${GAME_SLUG}/
USER steam
RUN steamcmd \
		+login anonymous \
		+force_install_dir /${GAME_SLUG}/ \
		+app_update ${GAME_APPID} validate \
		+quit
# Remove stupid joystick configs that are useless on a headless server and spit out console errors on startup
RUN bash -c "rm -f /$GAME_NAME/$GAME_NAME/cfg/{joy_configuration_sc,joystick,controller,joy_configuration_sc_analog,joy_configuration_sc_absolute}.cfg"
# Remove usless .exe and .dll files (won't be needing those on a Linux server!)
RUN find /${GAME_SLUG}/ -type f \( -iname \*.exe -o -iname \*.dll -iname \*.dylib \) -delete
# Remove all map files to reduce size by quite a bit (they can be loaded from a volume instead)
RUN find /${GAME_SLUG}/ -type f \( -iname \*.nav -o -iname \*.bsp \) -delete
# Remove any of these macOS binaries
RUN find /${GAME_SLUG}/ -iname 'srcds_osx*'
RUN find /${GAME_SLUG}/ -iname 'osx64*' -type d

# https://discordapp.com/channels/335290997317697536/335290997317697536/577952978368069653
USER root
# Remove SteamCMD download cache crap
RUN bash -c "rm -rf /home/*/.steam/ /root/.steam/"
USER steam

# SRCDS
EXPOSE 27015/udp
# SourceTV Web
EXPOSE 27016/tcp
# SourceTV
EXPOSE 27020/udp

WORKDIR /${GAME_SLUG}/

ARG GAME_APPID
ENV GAME_APPID=${GAME_APPID}

ENTRYPOINT ["/${GAME_APPID}/srcds_run"]
