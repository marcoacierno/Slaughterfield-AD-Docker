FROM debian:jessie

MAINTAINER Marco Acierno <marcoaciernoemail@gmail.com>

# SA-MP server executable is a x86 application only
RUN dpkg --add-architecture i386

# Install packages
RUN apt-get update && apt-get install -y \
 lib32stdc++6 \
 wget \
 psmisc \
 unzip

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

RUN wget https://github.com/marcoacierno/Slaughterfield-AD/raw/v5.0/gamemodes/s7ad2.amx \
 && mv s7ad2.amx /samp-svr/gamemodes

# Copy plugins: mysql, sscanf, streamer, Whirlpool

# Plugin: mysql

RUN wget https://github.com/pBlueG/SA-MP-MySQL/releases/download/R39-3/mysql-r39-3-debian7.tar.gz \
 && mkdir mysql \
 && tar xzf mysql-r39-3-debian7.tar.gz -C mysql \
 && rm -f mysql-r39-3-debian7.tar.gz \
 && mv mysql/plugins/mysql_static.so /samp-svr/plugins/mysql_static.so

# Plugin: streamer

RUN wget https://github.com/samp-incognito/samp-streamer-plugin/releases/download/v2.8/streamer-plugin-2.8.zip \
 && unzip streamer-plugin-2.8.zip -d streamer-plugin-2.8 \
 && mv streamer-plugin-2.8/plugins/streamer.so /samp-svr/plugins/streamer.so \
 && rm -rf streamer-plugin-2.8

# Plugin: Whirlpool

RUN wget http://revonline.comuf.com/Whirlpool.so \
 && mv Whirlpool.so /samp-svr/plugins/Whirlpool.so

# Plugin: Whirlpool

RUN wget http://revonline.comuf.com/sscanf.so \
 && mv sscanf.so /samp-svr/plugins/sscanf.so


ENTRYPOINT ["/entrypoint.sh"]

CMD ["samp", "start"]

EXPOSE 7777/udp
