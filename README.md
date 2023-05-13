# Docker Moodle LMS

Goal: to create an instance that will be used to develop, test and deploy to a cloud platform provider

(ARM64 and AMD64 architectures supported)

## Requirements 

1. docker-compose 
    - Apache 8.0
    - Maria 10.6 
2. Moodle 4.2

### First time setup

make sure everything is update

```bash
sudo apt update && sudo sudo apt upgrade -y
```

install docker & docker-compose

```bash
sudo apt install docker docker-compose
```

create a group for docker users

```bash
sudo usermod -aG docker $USER
```

---
```bash
# current session for changes to take effect
exit 

# reconnect and users groups
ubuntu@ip-172-31-2-124:~/lms__dirname$ groups
>ubuntu adm dialout cdrom floppy sudo audio dip video plugdev netdev lxd docker

# if docker isnt there try rebooting
sudo reboot
```


After cloning this repository, run commands

Build the Dockerfile
```bash
docker-compose build
``` 

Add Moodle source code to repository
```bash
git clone -b MOODLE_402_STABLE https://github.com/moodle/moodle.git ./moodle_data
```

Start docker containers
```bash
docker-compose up -d
```

<!-- Go to [localhost:8080](http://localhost:8080) and follow installation instructions -->

if error occurs when loading web page check logs for troubleshooting
```bash
docker-compose logs moodle
```

## SSL signing

its a little tricky here but it does work, and runs cron job twice a to checck with certbot

## Install Process

### Confirm paths

- Paths are auto populated and were mapped during docker build
    - `/var/www/html/moodle` is mapped to `~/moodle_data`
    - `/var/www/html/moodledata` is mapped to `~/moodledata_data`

### Database driver 

- Select `MariaDB` from dropdown menu

### Database settings

|key|value|notes|
|---|---|---|
|database host| `db`|
|database name|`moodle`|
|database user|`moodle`|
|database password|`dbPassword123`| *set by `MYSQL_PASSWORD` in compose file*|
|table prefix|`mdl_`|
|database port|`3306`|
|unix socket||*leave this field blank*|

- Confirm database settings and click "next" button

### Setup and installation

- Moodle will set up tables, click "continue" when completed
- Agree to lincense and click "continue"
- wait for installation to complete and click "continue" when done

> Next step takes a few minutes, if you are connected via ssh you'll be disconnected, this is normal behavior. wait a few minutes process should finish about 10-15 minutes

### Create admin account

- Fill out the admin account form, including username, password, email, first name, and last name. Make sure to remember your credentials, as you'll need them to log in and manage your Moodle site
- Click "Update profile" to create the admin account

### Set up front page settings

- Fill out the information for your site, such as the full site name, short name, and description. You can customize other settings, like the front page format and default language
- Click "Save changes" to proceed

---

Moodle setup is now complete. You will be redirected to the front page of your Moodle site.

Visit the [Moodle Docs](https://docs.moodle.org/) and the [Moodle community forums](https://moodle.org/course/) for additional resources and support.

---

## troubleshooting

clear all data from docker _this will erase all volume data too_
```bash
# check all container aare stopped
docker-compose ps

# if they're any running
docker-compose down

# remove images 
docker image prune

# remove volume
docker volume prune

# clean system
docker system prune

```

installation setup error: file permission

```bash

# change owner
sudo chown ubuntu:ubuntu moodledata_data

# set read write execute 
sudo chmod 777 moodledata_data

# enter the container
docker exec -ts lms_moodle_1 bash

# ensure the owner `www-data` has right me

ls -la /var/www/html

# if not as below
    # drwxrwxrwt  1 www-data www-data 4096 May 13 12:16 .
    # drwxr-xr-x  1 root     root     4096 Apr 22 18:33 ..
    # drwxrwxr-x 60 www-data www-data 4096 May 13 11:11 moodle
    # drwxrwxrwx  5 www-data www-data 4096 May 13 12:48 moodledata
# then next 2 cmds

RUN chown -R www-data:www-data /var/www/html/moodle

RUN chown -R www-data:www-data /var/www/html/moodledata

```


-------------------------


`sudo docker exec -it lms_moodle_1 /bin/bash`

`vim /etc/apache2/sites-available/learn.joshmclain.com.conf`
