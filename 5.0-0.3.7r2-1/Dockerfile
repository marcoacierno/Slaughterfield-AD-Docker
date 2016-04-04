FROM debian:jessie

MAINTAINER Marco Acierno <marcoaciernoemail@gmail.com>

# SA-MP server executable is a x86 application only
RUN dpkg --add-architecture i386

# Install packages
RUN apt-get update && apt-get install -y \
 lib32stdc++6 \
 wget \
 psmisc

# Download and extract server files
# Choose a more general directory name which does not contain any version
RUN wget http://files.sa-mp.com/samp037svr_R2-1.tar.gz \
 && tar xzf samp037svr_R2-1.tar.gz \
 && rm -f samp037svr_R2-1.tar.gz \
 && mv /samp03 /samp-svr \
 && cd samp-svr \
 && rm -rf include npcmodes/*.pwn filterscripts/*.pwn gamemodes/*.pwn \
 && mv samp03svr samp-svr \
 && chmod 700 * \
 && mkdir plugins

COPY samp.sh /usr/local/bin/samp
COPY docker-entrypoint.sh /entrypoint.sh
COPY server.cfg /samp-svr/server.cfg
COPY geoip_city.db /samp-svr/scriptfiles/geoip_city.db
COPY geoip.db /samp-svr/scriptfiles/geoip.db

RUN chmod +x /usr/local/bin/samp \
 && chmod +x /entrypoint.sh

# Copy gamemode

RUN wget https://github.com/marcoacierno/Slaughterfield-AD/raw/develop/gamemodes/s7ad2.amx \
 && mv s7ad2.amx /samp-svr/gamemodes

RUN wget http://revonline.comuf.com/slaughterfield-ad/5.0-0.3.7r2-1.tar.gz \
 && mkdir deps \
 && tar xvf 5.0-0.3.7r2-1.tar.gz \
 && rm -f 5.0-0.3.7r2-1.zip \
 && cd 5.0-0.3.7r2-1/linux-plugins/ \
 && mv * /samp-svr/plugins/

ENTRYPOINT ["/entrypoint.sh"]

CMD ["samp", "start"]

EXPOSE 7777/udp
