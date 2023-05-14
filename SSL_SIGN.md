# Certbot SSL

Certbot has a container that is independent of the moodle installation. The cron job is scheduled to check every 12hours if a renew is necessary. The is also a certbot instance installed with container running moodle. That is where our volume data is stored and will be making final edits.

### `docker ps` gets outbelow.
|CONTAINER ID|IMAGE|COMMAND|CREATED|STATUS|PORTS|NAMES|
|    ---    | --- | --- |     --- |   --- | --- | --- |
|6c357accd9ed|   docker-moodle_moodle|   "moodle-docker-php-e…"|   32 seconds ago|   Up 25 seconds|   0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp|   docker-moodle_moodle_1|
|a4ce64095c72|   mariadb:10.6|           "docker-entrypoint.s…"|   34 seconds ago|   Up 27 seconds|   3306/tcp|                                   docker-moodle_db_1|
|21ba81958c5e|   certbot/certbot|        "/bin/sh -c 'trap ex…"|   34 seconds ago|   Up 27 seconds|   80/tcp, 443/tcp|                            docker-moodle_certbot_1|

The names will vary depending on parent folder. We are going to need the named container running moodle. based on this example.

```bash
# brings us in root user inside the container
docker exec -it docker-moodle_moodle_1 /bin/bash

# need temporary editor
apt install vim

```
