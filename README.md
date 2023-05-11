# Docker Moodle LMS

Goal: to create an instance that will be used to develop, test and deploy to a cloud platform provider

(ARM64 and AMD64 architectures supported)

## Requirements 

- Moodle 4.2
- docker-compose
    - Apache 8.0
    - Maria 10.6 

### First time setup

After cloning this this repository, run commands from project root

Build the Dockerfile
```bash
$ docker-compose build
``` 

Add Moodle source code to repository
```bash
$ git clone -b MOODLE_402_STABLE https://github.com/moodle/moodle.git ./moodle_data
```

Start docker containers
```bash
$ docker-compose up -d
```

Go to [localhost:8080](http://localhost:8080) and follow installation instructions

if error occurs when loading web page check logs for troubleshooting
```bash
$ docker-compose logs moodle
```

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
