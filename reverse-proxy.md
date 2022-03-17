# Configuring Nginx as a reverse proxy for Web application.


## Install nginx 
>## sudo apt-get install nginx

## Once you gave nginx installed you would have a directory structure like this under 
>## cd /etc/nginx/

## Here, conf.d is the directory where the configuration files are strored.
## The main configuration file is nginx.conf file and it is recommended to take a backup for it.

## The 2 important directories are "sites-available" and "sites-enable"
## An important thing to know about "sites-enable" is whatever configuration you will put into "sites-enabled", this configuration will apply when you do nginx start or nginx restart. The same will not happpen in the case of "sites-available"
---------------------------------------------
## we will be creating a file in sites-available directoy and then create a symbolic link for that in sites enabled directory.

>## cd /etc/nginx/sites-available/
## NOW WE WRITE OUR CONFIGURATION FILE.
>## sudo vi example-app



