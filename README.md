# Docker Compose Piwigo & MariaDB
This is build documentation for Piwigo and MariaDB using docker compose.

The following images will be used:
 * [Piwigo](https://docs.linuxserver.io/images/docker-piwigo/)
 * [MariaDB](https://docs.linuxserver.io/images/docker-mariadb/)

## Getting started
Before starting this project we need to decide where data is stored. For the sake of testing, make a directory in the home directory. Take note of the file path, it will be used in future configuration.
```
mkdir /home/<user>/piwigo
```

Make a `.yml` file. This will be used as the docker compose config. 
```
touch docker-compose.yml
```

Use wget to get the base config from this repo or add the following to your config:

```
services:
  mariadb:
    image: lscr.io/linuxserver/mariadb:latest
    container_name: mariadb
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
      - MYSQL_ROOT_PASSWORD=ROOT_ACCESS_PASSWORD
      - MYSQL_DATABASE=USER_DB_NAME #optional
      - MYSQL_USER=MYSQL_USER #optional
      - MYSQL_PASSWORD=DATABASE_PASSWORD #optional
      - REMOTE_SQL=http://URL1/your.sql,https://URL2/your.sql #optional
    volumes:
      - /path/to/mariadb/config:/config
    ports:
      - 3306:3306
    restart: unless-stopped
services:
  piwigo:
    image: lscr.io/linuxserver/piwigo:latest
    container_name: piwigo
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
    volumes:
      - /path/to/piwigo/config:/config
      - /path/to/appdata/gallery:/gallery
    ports:
      - 80:80
    restart: unless-stopped
```
