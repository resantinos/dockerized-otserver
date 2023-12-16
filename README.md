# Dockerized Tibia OTserver

## What's in this repository?
In this repository you will find shell scripts, SQL, yaml and PHP files to start a docker environment and run an OTserver(_open tibia server_).

Four containers are used:
- OTserver(_open tibia server_)
- Database(_mysql_)
- Database manager(_phpmyadmin_)
- Web server (_php+apache_)

## Requirements
-docker
- docker compose
- unzip
- wget
- notepad++
- client tibia 12x([Canary - Version 2.0.0](https://github.com/opentibiabr/canary/releases/tag/v2.0.0))
- dependencies seen in [Compiling on Ubuntu 22.04](https://github.com/opentibiabr/canary/wiki/Compiling-on-Ubuntu-22.04)

## Quick start
- To start the containers (otserver, mysql, phpmyadmin, php+apache) and download files from the server, run the `start.sh` script providing the `-d` or `--download` parameter. If you have already downloaded the files, just run `start.sh` without any parameters.
- The database can be managed through `phpMyAdmin` exposed at http://localhost:9090, the credentials to access it are: `root`/`noob` or `otserv`/`noob`.
- The authentication endpoint is exposed by the `php+apache` container at http://localhost:8080/login.php.
- Use a Tibia Client 12x to access the server. The download can be done through [tag 2.0.0](https://github.com/opentibiabr/canary/releases/tag/v2.0.0) from the repository [opentibiabr/canary](https://github.com/ opentibiabr/canary). In Tibia Client 12x itself, it will be necessary to change the values of the `loginWebService` and `clientWebService`([tutorial](https://github.com/RafaelClaumann/dockerized-otserver/blob/main/README.md#alterando-url- de-autentica%C3%A7%C3%A3o-no-tibia-client)).
- To log in to Tibia Client 12x use the following credentials: `@god`/`god` or `@a`/`1`.
- To close the containers (otserver, mysql, phpmyadmin, php+apache) run the `docker-compose down` command.

## Repository files
In the `start.sh` script, the database credentials and Docker network settings are defined; in few cases, it will be necessary to change the credentials or network settings. The script is also responsible for starting containers (otserver, mysql, phpmyadmin, php+apache) with the `docker-compose up -d` command.

Available parameters to start the `start.sh` script:
| parameter | description |
|-------------------|----------------------------- -------------------------------------------------- ------------------------|
| -d or --download | Download and extract the server [canary](https://github.com/opentibiabr/canary) in the `server/` folder. If the server files are not found in the `server/` folder and you do not provide the -d or --download parameter the script will not work. |

The `login.php` file is a simplification of the login.php found in [MyAAC](https://github.com/otsoft/myaac/blob/master/login.php).
This simplification facilitates authentication on the server/database and avoids the installation and configuration of an AAC (_Gesior2012 or MyAAC_).

During login, Tibia Client 12x makes requests to the URLs `loginWebService` and `clientWebService` which are configured in the Tibia Client itself([tutorial](https://github.com/RafaelClaumann/dockerized-otserver/blob/main/README .md#alterando-url-de-autentica%C3%A7%C3%A3o-no-tibia-client)).

The URLs configured in Tibia Client 12x lead to the `login.php` file on the web server (php+apache), which in turn will communicate with the database (MySQL) to authenticate the client. The web server does not have a graphical interface, it is only possible to create accounts and characters in the database using SQL commands.

The database schema and some accounts are created automatically upon initialization of the `MySQL` container, see the files [00_schema.sql](https://github.com/RafaelClaumann/dockerized-otserver/blob/main/sql /00_schema.sql) and [01_data.sql](https://github.com/RafaelClaumann/dockerized-otserver/blob/main/sql/01_data.sql).

The accounts listed below are created at database initialization (MySQL).
| email | password | characters |
|------- |---------- |---------------------- ------------------------------ |
| @god | god | GOD, paladin/sorcerer/druid/knight sample |
| @a | 1 | Paladin(800) Sorcerer(800) Druid(800) Knight(800) |
| @b | 1 | ADM1 |
| @c | 1 | ADM2 |

`docker-compose.yaml` contains the declaration of containers (otserver, mysql, phpmyadmin, php+apache) that are started when the `start.sh` script is executed. The fields in the format `${xxxx}` in `docker-compose.yaml` receive the values of the variables exported in the `start.sh` script.

## Changing authentication URL in Tibia Client
Assuming that [download](https://github.com/opentibiabr/canary/releases/tag/v2.0.0) of T
