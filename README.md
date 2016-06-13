Installing LEMP
===============

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

https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-on-ubuntu-12-04


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
