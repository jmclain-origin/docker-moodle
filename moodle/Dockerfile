FROM moodlehq/moodle-php-apache:8.0

# Set www-data (Apache user and group) to the desired user and group IDs (33 in this case)
RUN usermod -u 33 www-data \
    && groupmod -g 33 www-data

# Configure Apache DocumentRoot to point to the Moodle directory
RUN sed -i 's/DocumentRoot \/var\/www\/html/DocumentRoot \/var\/www\/html\/moodle/g' /etc/apache2/sites-available/000-default.conf

# Copy the virtual host configuration file
COPY learn.joshmclain.com.conf /etc/apache2/sites-available/learn.joshmclain.com.conf

# Enable the virtual host
RUN ln -s /etc/apache2/sites-available/learn.joshmclain.com.conf /etc/apache2/sites-enabled/learn.joshmclain.com.conf

# Install Certbot
RUN apt-get update && apt-get install -y certbot python3-certbot-apache

# Expose ports 80 and 443
EXPOSE 80
EXPOSE 443

# Start Apache
CMD ["apache2-foreground"]
