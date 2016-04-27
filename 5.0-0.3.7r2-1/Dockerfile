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
 && mkdir plugins \
 && wget https://github.com/jwilder/dockerize/releases/download/v0.2.0/dockerize-linux-amd64-v0.2.0.tar.gz \
 && tar -C /usr/local/bin -xzvf dockerize-linux-amd64-v0.2.0.tar.gz

COPY samp.sh /usr/local/bin/samp
COPY docker-entrypoint.sh /entrypoint.sh
COPY etc/geoip_city.db /samp-svr/scriptfiles/geoip_city.db
COPY etc/geoip.db /samp-svr/scriptfiles/geoip.db

RUN chmod +x /usr/local/bin/samp \
 && chmod +x /entrypoint.sh

# Copy gamemode

RUN wget https://github.com/marcoacierno/Slaughterfield-AD/raw/develop/gamemodes/s7ad2.amx \
 && mv s7ad2.amx /samp-svr/gamemodes \
 && wget https://github.com/pBlueG/SA-MP-MySQL/releases/download/R39-3/mysql-r39-3-debian7.tar.gz \
 && mkdir mysql \
 && tar xzf mysql-r39-3-debian7.tar.gz -C mysql \
 && rm -f mysql-r39-3-debian7.tar.gz \
 && mv mysql/plugins/mysql_static.so /samp-svr/plugins/mysql_static.so \
 && rm -r mysql \
 && wget https://github.com/samp-incognito/samp-streamer-plugin/releases/download/v2.8/streamer-plugin-2.8.zip \
 && unzip streamer-plugin-2.8.zip -d streamer-plugin-2.8 \
 && mv streamer-plugin-2.8/plugins/streamer.so /samp-svr/plugins/streamer.so \
 && rm -rf streamer-plugin-2.8 \
 && rm -f streamer-plugin-2.8.zip \
 && wget http://revonline.comuf.com/Whirlpool.so \
 && mv Whirlpool.so /samp-svr/plugins/Whirlpool.so \
 && wget http://revonline.comuf.com/sscanf.so \
 && mv sscanf.so /samp-svr/plugins/sscanf.so

COPY etc/slfd_mysql.tmpl /slfd_mysql.tmpl
COPY etc/server.cfg.tmpl /server.cfg.tmpl

ENTRYPOINT ["/entrypoint.sh"]

CMD ["dockerize", "-template", "server.cfg.tmpl:samp-svr/server.cfg", "-template", "slfd_mysql.tmpl:samp-svr/scriptfiles/slfd_mysql.ini", "-stdout", "/samp-svr/server_log.txt", "-wait", "tcp://db:3306", "-timeout", "120s", "samp", "start"]

EXPOSE 7777/udp
