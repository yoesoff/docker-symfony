docker-symfony
==============


This is a complete stack for running Symfony 4 (latest version: Flex) into Docker containers using docker-compose tool.

# Installation

First, clone this repository:

```bash
$ git clone https://github.com/yoesoff/docker-symfony.git
```

Next, put your Symfony application into `symfony` folder and do not forget to add `symfony.localhost` in your `/etc/hosts` file.

Make sure you adjust `database_host` in `parameters.yml` to the database container alias "db"

Then, run:

```bash
$ docker-compose up
```

You are done, you can visit your Symfony application on the following URL: `http://symfony.localhost` (and access Kibana on `http://symfony.localhost:81`)

_Note :_ you can rebuild all Docker images by running:

```bash
$ docker-compose build
```

# How it works?

Here are the `docker-compose` built images:

* `db`: This is the MySQL database container (can be changed to postgresql or whatever in `docker-compose.yml` file),
* `php`: This is the PHP-FPM container including the application volume mounted on,
* `nginx`: This is the Nginx webserver container in which php volumes are mounted too,
* `elk`: This is a ELK stack container which uses Logstash to collect logs, send them into Elasticsearch and visualize them with Kibana.

This results in the following running containers:

```bash
> $ docker-compose ps
        Name                       Command               State              Ports
--------------------------------------------------------------------------------------------
dockersymfony_db_1      docker-entrypoint.sh mysqld      Up      0.0.0.0:3306->3306/tcp
dockersymfony_elk_1     /usr/bin/supervisord -n -c ...   Up      0.0.0.0:81->80/tcp
dockersymfony_nginx_1   nginx                            Up      443/tcp, 0.0.0.0:80->80/tcp
dockersymfony_php_1     php-fpm7 -F                      Up      0.0.0.0:9000->9000/tcp
```

# Read logs

You can access Nginx and Symfony application logs in the following directories on your host machine:

* `logs/nginx`
* `logs/symfony`

# Use Kibana!

You can also use Kibana to visualize Nginx & Symfony logs by visiting `http://symfony.localhost:81`.

# Use xdebug!

To use xdebug change the line `"docker-host.localhost:127.0.0.1"` in docker-compose.yml and replace 127.0.0.1 with your machine ip addres.
If your IDE default port is not set to 5902 you should do that, too.

# Code license

You are free to use the code in this repository under the terms of the 0-clause BSD license. LICENSE contains a copy of this license.
