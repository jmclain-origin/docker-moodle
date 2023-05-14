# Moodle LMS Docker setup

(ARM64 and AMD64 architectures supported)

## Getting started

Before starting you should have

- Host machine and youre able to secure shell (`ssh`) into it
- Domain name that you have access to DNS (can use host public IP, can't change later)
- set strong password for `MYSQL_PASSWORD` in compose file

## a) Install deps on host

```bash
# 1) Update host system

sudo apt update && sudo sudo apt upgrade -y

# 2) Install docker & docker-compose

sudo apt install docker docker-compose

# 3) add current user to docker group

sudo usermod -aG docker $USER
```

In order for changes to take effect user must logout anf log back in.

```bash
# 4) exit to reset
exit 

# 5) checking after exit still gone
ubuntu@ip-0-0-0-0:~/lms__dirname$ groups
>ubuntu adm dialout cdrom floppy sudo audio dip video plugdev netdev lxd docker

# 6) if docker isnt there try rebooting
sudo reboot
```

Should be able to proceed if reboot was done, else Id wait for full resolution

# b) preping docker files

Before continuing if you plan on having HTTPS traffic you need to add/edit files in `./moodle/`

1) We will need to change the virtirual host config file to match the name of your site.
   If your domain name is **awesome_site.com** then the file name should be `awesome_site.com.conf`
2) Next edit the content in `.conf` to mactch your specific requirements
3) edit `./moddle/Dockerfile` to match your site. I'll continue useing `awesome_site.com`

```apache
# file: ./moodle/awesome_site.com.conf

<VirtualHost *:80>
    ServerName awesome_site.com
    DocumentRoot /var/www/html/moodle

    # Certbot challenge proxy
    <Location /.well-known/acme-challenge>
        Require all granted
    </Location>

    # Additional Moodle configuration...
</VirtualHost>
```

 *make sure to change to awesome_stie.com to you*

```bash
# file: ./moodle/Dockerfile

# Copy the virtual host configuration file
COPY nice.example.com.conf /etc/apache2/sites-available/nice.example.com.conf

# Enable the virtual host
RUN ln -s /etc/apache2/sites-available/nice.example.com.conf /etc/apache2/sites-enabled/nice.example.com.conf

# Install Certbot
RUN apt-get update && apt-get install -y certbot python3-certbot-apache```
) creating docker containers

```

*we will be coming back to these for later edits.*

# c) Build Docker image

Before starting the build we need to get the Moodle source code and add it to our `./moodle_data` folder once finish we can start building

1) Add Moodle source code to our file tree
2) Build docker images
3) Use docker-compose to start up containers running our Moodle instace

```bash
# clone Moodle v4.2 to folder `moodle_data` in root dir !important do not change folder name
git clone -b MOODLE_402_STABLE https://github.com/moodle/moodle.git ./moodle_data

# build dockerfile
docker-compose build

# start containers
docker-compose up -d
```

*Moodle should be accessable now on host IP on HTTP traffic I you had a domain routed to that IP check status.*

---

#### Stoping docker

If this is the first time shutting down and you hadn't done clean-up yet, you'll need to before starting docer again.

```bash
# stoping docker process
docker-compose down
```

#### Logging

if error occurs when loading web page check logs for troubleshooting

```bash
docker-compose logs moodle
```

# d) Post Build Cleanup

Before moving forward there are a couple things from the build that we will want to remove. The volume running Moodle is presistant and after adding SSL we don't want to overwrite. 

Delete these 3 line `./moodle/Dockerfile` and we will delete the `*.conf` file if you made one earlier. If not the proceed to Moodle installation

```bash
# Copy the virtual host configuration file
COPY nice.example.com.conf /etc/apache2/sites-available/nice.example.com.conf
# Enable the virtual host
RUN ln -s /etc/apache2/sites-available/nice.example.com.conf /etc/apache2/sites-enabled/nice.example.com.conf
# Install Certbot
RUN apt-get update && apt-get install -y certbot python3-certbot-apache
```

# SSL certbot signing

[SSL_SIGN](./SSL_SIGN.md)

# Install Process

[MOODLE_INSTALL](./MOODLE_INSTALL.md)


## troubleshooting

[TROUBLESHOOTING](./TROUBLESHOOTING.md)

---

`sudo docker exec -it lms_moodle_1 /bin/bash`

`vim /etc/apache2/sites-available/learn.joshmclain.com.conf`
