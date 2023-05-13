In your Docker Compose file, you have the Moodle data directory mapped to a volume on the host:

yaml
Copy code
    volumes:
      - ./moodledata_data:/var/www/html/moodledata
Here are some things you can check:

Ensure that the ./moodledata_data directory exists on your Docker host and has the correct permissions. You can adjust the permissions using the chmod command, for example:

bash
Copy code
sudo chmod -R 777 ./moodledata_data
This command gives all users read, write, and execute permissions for the directory. In a production environment, you should restrict these permissions as much as possible for security reasons.

The user running the web server inside the Docker container (usually www-data for Apache) needs to have write permissions for the /var/www/html/moodledata directory.

You can change the owner of the directory to www-data using the chown command:

bash
Copy code
sudo chown -R www-data:www-data ./moodledata_data
However, this command needs to be run inside the Docker container, not on the host. You can do this by opening a shell in the Docker container using the docker exec command:

bash
Copy code
docker exec -it <container-id> /bin/bash
chown -R www-data:www-data /var/www/html/moodledata
Replace <container-id> with the ID of your Moodle container.

If you are still having problems, it might be worth checking that the Moodle configuration file is correctly set up to use this data directory.

Please remember that these suggestions are meant for debugging and may not be suitable for a production environment, especially the suggestion to use chmod 777. It's important to properly secure your Moodle data directory in a production environment.


-------------------------------------------------------------------------------------------------------------------------------
    Name                   Command               State                                   Ports                                 
-------------------------------------------------------------------------------------------------------------------------------
lms_certbot_1   /bin/sh -c trap exit TERM; ...   Up      443/tcp, 80/tcp                                                       
lms_db_1        docker-entrypoint.sh mariadbd    Up      3306/tcp                                                              
lms_moodle_1    moodle-docker-php-entrypoi ...   Up      0.0.0.0:443->443/tcp,:::443->443/tcp, 0.0.0.0:80->80/tcp,:::80->80/tcp