# Mapbender installation 

### Mapbender3 Open Source Project

The manditory system requirements to work on the mapbender3 open source project is: 
  - cURL
  - pear
  - Phing
  - php5-dev

So to start the installation process you have to first fork the mapbender github directory at https://github.com/mapbender/mapbender-starter. Then clone the repository into you own local repo by running:
git clone https://github.com/mapbender/mapbender-starter.git mapbender3
 - cd mapbender3

The you will need ot change the tag of the project to the latest version, this is done by running: 
 - git tag -l 
 - git checkout v3.0.5.3

To get the submodules
 - git submodule update --init --recursive

To install curl run
 - sudo apt-get install curl

Install Phing to build 
 - sudo apt-get install php-pear

Then update pear by running
 - sudo config-set auto_discover 1
 - sudo pear upgrade-all 
But for me it cause an error warning that pear change it's upgrade command so I had to upgrade each
individual package one at a time by running:
 - sudo pear list-upgrades
 - sudo pear upgrades [package name]

To get the Phing management system you would have to run:
 - sudo pear channel-discover pear.phing.info
 - sudo pear install phing/phing

Then to get all the dependencies for this project you would have to run composer in the application directory so:
 - cd application 
 - curl -sS https://getcomposer.org/installer | php

Create a configuration file called parameters.yml and copy the parameters.yml.dist:
 - cp app/config/parameters.yml.dist app/config/parameters.yml

To get the dependencies for the mapbender project you would have to run composer:
 - ./composer.phar update

### Mapbender Installation 
To get the mapbender application you would have to download the mapbender zip and extract it to the /var/www/ directory in order to run the applicatiom. The system requirements and component dependencies require to run the actually application itself can be obtain by running:
 - sudo apt-get install php5 php5-pgsql php5-gd php5-curl php5-cli php5-sqlite sqlite php-apc php5-intl curl openssl

Then reload apache to ensure that the compontest that you have just downloaded are up and running.
 - sudo a2enmod rewrite

Create the file /etc/apache2/sites-available/mapbender3.conf with the content below:
Alias /mapbender3 /var/www/mapbender3/web/
<Directory /var/www/mapbender3/web/>
 Options MultiViews FollowSymLinks
 DirectoryIndex app.php
 Require all granted

 RewriteEngine On
 RewriteBase /mapbender3/
 RewriteCond %{ENV:REDIRECT_STATUS} ^$
 RewriteCond %{REQUEST_FILENAME} !-f
 RewriteCond %{REQUEST_FILENAME} !-d
 RewriteRule ^(.*)$ app.php/$1 [PT,L,QSA]
</Directory>

Then you would have to reload the apache serve in order for you the file you have just create to run.  
 - service apache2 reload

To check that the alias is working got to http://localhost/mapbender3/

References
http://doc.mapbender3.org/en/book/installation/installation_git.html
http://doc.mapbender3.org/en/book/installation/installation_ubuntu.html
