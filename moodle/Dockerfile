FROM moodlehq/moodle-php-apache:8.0

# Set www-data (Apache user and group) to the desired user and group IDs (33 in this case)
RUN usermod -u 33 www-data \
    && groupmod -g 33 www-data

# Configure Apache DocumentRoot to point to the Moodle directory
RUN sed -i 's/DocumentRoot \/var\/www\/html/DocumentRoot \/var\/www\/html\/moodle/g' /etc/apache2/sites-available/000-default.conf
