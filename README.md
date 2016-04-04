Slaughterfield AD Docker container
======================

### Run container

Latest SA-MP version: 
```
docker run -d -p 7777:7777/udp -e SAMP_RCON_PASSWORD=secret --name server marcoacierno/slaughterfield-ad
```

Run MySQL

```
docker run --name db -v /path/to/mysql/dump/mysqldb:/docker-entrypoint-initdb.d -e MYSQL_ROOT_PASSWORD=<your_root_password> -d mysql:5.7
```
