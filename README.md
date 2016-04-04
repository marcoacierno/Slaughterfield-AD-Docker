Slaughterfield AD Docker container
======================

### Run container

Latest SA-MP and Gamemode version: 

```
docker run -d -p 7777:7777/udp -e SAMP_RCON_PASSWORD=secret --name server marcoacierno/slaughterfield-ad
```

To link with MySQL DB 

```
docker run -d -p 7777:7777/udp -e SAMP_RCON_PASSWORD=secret --name server --link db:db marcoacierno/slaughterfield-ad
```

Run MySQL

```
docker run --name db -v /path/to/mysql/dump/mysqldb:/docker-entrypoint-initdb.d -e MYSQL_DATABASE=s7ad -e MYSQL_USER=s7ad -e MYSQL_PASSWORD=<password> -e MYSQL_ROOT_PASSWORD=<your_root_password> -d mysql:5.7
```
