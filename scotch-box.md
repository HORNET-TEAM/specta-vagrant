# Get Started

Getting started with Scotch Box is a breeze. We’ll cover everything, but chances are you’ve probably already done some of these things.

DOWNLOAD AND INSTALL VAGRANT



Go to https://www.vagrantup.com/downloads.html

DOWNLOAD AND INSTALL VIRTUALBOX


Go to https://www.virtualbox.org/wiki/Downloads

CLONE THE SCOTCH BOX GITHUB REPOSITORY



From the command line, you’ll want to do this inside the folder where you want your project to be on your computer.
```
git clone https://github.com/scotch-io/scotch-box.git my-project
 
```
RUN VAGRANT UP



Start the box! If this is your first time, the box will need to download. After that, everything should be ultra-fast to start.

```
vagrant up

```

ACCESS YOUR PROJECT



Success! You can now access your project by visiting http://192.168.33.10/. There’s a nice little landing page to let you know it’s working too.

DETAILS FOR SPECTASONIC

#### Config Apache2

* Virtual Host


```
cd /etc/apache/sites-available
sudo nano spectasonic.local.conf

<VirtualHost *:80>
        ServerAdmin webmaster@localhost

        ServerName spectasonic.local
        ServerAlias www.spectasonic.local

        DocumentRoot /var/www/spectasonic/web
        <Directory /var/www/spectasonic/web>
            AllowOverride All
            Order Allow,Deny
            Allow from All
        </Directory>

    # uncomment the following lines if you install assets as symlinks
    # or run into problems when compiling LESS/Sass/CoffeScript assets
        <Directory /var/www/project>
           Options FollowSymlinks
        </Directory>

        ErrorLog /var/log/apache2/specta_error.log
        CustomLog /var/log/apache2/specta_access.log combined
</VirtualHost>

```

* Database

Change Root Password

```
mysqladmin -u root password root
```

Create the database with

```
php bin/console doctrine:database:create

```

Nb: Database name is on `app/config/parameters.yml`.

Other solution if base not created

```
mysql -u root -proot
CREATE DATABASE database_name;
```
* Restart apache

```
sudo a2ensite spectasonic.local.conf
sudo service apache2 reload

```

* Config on **host**

Get out from vagrant

```
sudo nano /etc/hosts

192.168.33.10 www.spectasonic.local
```

* Ip on app_dev.php

If you have permissions problems go on your app_dev.php file and add your vagrant ip

```
if (isset($_SERVER['HTTP_CLIENT_IP'])
    || (!(in_array(@$_SERVER['REMOTE_ADDR'], array('127.0.0.1','<My_IP>', 'fe80::1', '::1'))) AND !(($minIp <= $ip2longCli) && ($ip2longCli <= $maxIp)) )
        || php_sapi_name() === 'cli-server'
) {
    header('HTTP/1.0 403 Forbidden');
    echo @$_SERVER['REMOTE_ADDR'],
    exit('You are not allowed to access this file. Check '.basename(__FILE__).' for more information.');
}
```