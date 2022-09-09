# Configuring Nginx as a reverse proxy for Web application.


## Install nginx 
>## sudo apt-get install nginx

## Once you have nginx installed it will creates nginx folder in /etc

>## cd /etc/nginx/

## There is a main configuration file that is nginx.conf file and it is recommended to take a backup for it.

---------------------------------------------
## we will be creating a file in sites-available directoy and then create a symbolic link for that in sites enabled directory.

>## cd /etc/nginx/sites-available/
## NOW WE WRITE OUR CONFIGURATION FILE.
>## sudo vi example-app

```
server {
    listen 80;
    location / {
        proxy_pass http://localhost:3000;
    }
}  
```
## Now we a create symbolic link, so that when we reload the nginx , this file will be taken into effect.

>## sudo ln -s /etc/nginx/sites-avaialable/example-app /etc/nginx/sites-enabled/example-app
## *This will create the symbolic link.

## Now lets start Nginx

> ## sudo /etc/init.d/nginx start
> ## sudo systemctl start nginx



