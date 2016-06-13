Installing LEMP on Ubuntu 14.04 LTS
===================================

Assuming you have a clean install of Ubuntu 14.04 LTS

Install main packages
---------------------

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install php5 php5-fpm php5-mysql php5-cli nginx mysql-server git
```

*Optional PHP librairies*

```
sudo apt-get install php5-apcu php5-imagick php5-intl
```

Node
----

```
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
```

Composer
--------

```
curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer
```

Install Gulp globally
---------------------

```
sudo npm install -g gulp
```

Setup NGINX
-----------

Steps based on this [post](https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-on-ubuntu-12-04)

We need to make one small change in the php configuration.

**Open up php.ini:**

```
 sudo nano /etc/php5/fpm/php.ini
```

Find line, cgi.fix_pathinfo=1, and change to 0.

```
cgi.fix_pathinfo=0
```

Save and exit.

**update php5-fpm** (if necessary)

```
sudo nano /etc/php5/fpm/pool.d/www.conf
```

Find line, listen = 127.0.0.1:9000, and change to /var/run/php5-fpm.sock.

```
listen = /var/run/php5-fpm.sock
```

Save and exit.

Restart php-fpm:

```
sudo service php5-fpm restart
```

**Setup nginx**

Open up the default virtual host file.

```
sudo nano /etc/nginx/sites-available/default
```

Update Server information

```
 [...]
server {
        listen   80;
     
        root /usr/share/nginx/html;
        index index.php index.html index.htm;

        server_name example.com;

        location / {
                try_files $uri $uri/ /index.html;
        }

        error_page 404 /404.html;

        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
              root /usr/share/nginx/html;
        }

        # pass the PHP scripts to FastCGI server listening on the php-fpm socket
        location ~ \.php$ {
                try_files $uri =404;
                fastcgi_pass unix:/var/run/php5-fpm.sock;
                fastcgi_index index.php;
                fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
                include fastcgi_params;
                
        }

}
[...]
```

**Optionnally create a phpinfo file**

To set this up, first create a new file:

```
sudo nano /usr/share/nginx/html/info.php
```

Add in the following lines:

```
<?php
phpinfo();
```

Save and exit.

Restart nginx

```
sudo service nginx restart
```


Setup Deployment
----------------

**Create user deploy**

```
sudo useradd -d /home/deploy -m deploy
sudo passwd deploy
cd /var
mkdir www
chown -R deploy:root www
```

**Change to user Deploy**

```
su deploy
```

**Generate RSA KEY**

```
cd ~
ssh-keygen -t rsa
more .ssh/id_rsa.pub
```

Install Laravel Project
-----------------------

Clone repo using SSH

```
composer install
npm install in resource/assets
gulp deploy
```

Make sur storage is writable

```
chmod 777 sur storage
```

Run artisan migrate

```
php artisan migrate
```
