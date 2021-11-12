# AWS Stack Installation

## AWS Instance Configuration

Build the ec2 instances and install LAMP stack as per Amazon instructions. Use the LAMP Stack template if available.

* Create new EC2 Amazon AMI Linux 2 instance 
* Apply secrurity rules for SSH, HTTP,and HTTPS inbound
* Use existing BabyLab Key (or create new key for SSH access)

## Connect to AWS Instance 

`ssh -i /Users/paulcoogan/Documents/Clients/SDSU/WordRel/Keys/BabyLab.pem ec2-user@ec2-34-221-53-76.us-west-2.compute.amazonaws.com`

## LAMP Build

Refer to https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-lamp-amazon-linux-2.html for build instructions.

Make sure PHP is version 7+ and has the right modules.

```bash
sudo yum install php72-mbstring
sudo yum install php72-mysqli
```

Some other useful commands.

```bash
sudo yum update -y
sudo amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2
cat /etc/system-release
sudo yum install -y httpd mariadb-server
yum info package_name
sudo systemctl start httpd
```

Start at boot:

```bash
sudo systemctl enable httpd
```

Check if auto start is set up:

```bash
sudo systemctl is-enabled httpd
= enabled
```

Add GD Library

```bash
sudo yum update -y
sudo yum install -y php-gd
sudo reboot
```

## Modify Apache Conf

Set the doc root to the public folder and update to allow reading the .htaccess file for redirection.

```bash
Change Allow Override to All 

    AllowOverride All
```

While we are here, add aliases to a new conf file (/etc/httpd/conf.d/aliases.conf) for other web sites on the same URL.

```bash
    Alias "/mapper" "/var/www/html/mapper"
    Alias "/db" "/var/www/html/phpmyadmin"
```

## Set Permissions

Add your user (in this case, ec2-user) to the apache group.
sudo usermod -a -G apache ec2-user

Log out and then log back in again to pick up the new group

sudo chown -R ec2-user:apache /var/www

To add group write permissions and to set the group ID on future subdirectories, change the directory permissions of /var/www and its subdirectories.
sudo chmod 2775 /var/www && find /var/www -type d -exec sudo chmod 2775 {} \;

To add group write permissions, recursively change the file permissions of /var/www and its subdirectories:

Create a PHP file in the Apache document root.
echo "<?php phpinfo(); ?>" > /var/www/html/phpinfo.php

http://my.public.dns.amazonaws.com/phpinfo.php

http://34.221.53.76/phpinfo.php

## Install and Secure MySQL Database Server

```bash
sudo mysql_secure_installation
```

Yes to all and set root password: ************

Start at boot

```bash
sudo systemctl enable mariadb
```

Add MySQL to the start up

```bash
`sudo chkconfig mysqld on
```

Check what services are set for each run state

```bash
sudo chkconfig --list
```

Manual start up

```bash
sudo service mysqld start
```

## Install phpMyAdmin

```bash
sudo yum install php-mbstring php-xml -y
sudo systemctl restart httpd
sudo systemctl restart php-fpm
cd /var/www/html
wget https://www.phpmyadmin.net/downloads/phpMyAdmin-latest-all-languages.tar.gz
mkdir phpMyAdmin && tar -xvzf phpMyAdmin-latest-all-languages.tar.gz -C phpMyAdmin --strip-components 1
rm phpMyAdmin-latest-all-languages.tar.gz
```

http://IPADDRESS/phpMyAdmin

## Update DNS

Essential for installing the SSL certificate.

Name registration and DNS are at domain.com
Login cctapp.org
Password: ************
A Name * = IPADDRESS

## Add SSL

Refer to https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/SSL-on-amazon-linux-2.html

### Enable TLS on the server

Confirm that Apache is running.

```bash
sudo systemctl is-enabled httpd

sudo yum install -y mod_ssl

Your instance now has the following files that you use to configure your secure server and create a certificate for testing:
/etc/httpd/conf.d/ssl.conf

Your instance now has the following files that you use to configure your secure server and create a certificate for testing:
/etc/httpd/conf.d/ssl.conf
The configuration file for mod_ssl. It contains directives telling Apache where to find encryption keys and certificates, the TLS protocol versions to allow, and the encryption ciphers to accept.
/etc/pki/tls/certs/make-dummy-cert
A script to generate a self-signed X.509 certificate and private key for your server host. This certificate is useful for testing that Apache is properly set up to use TLS. Because it offers no proof of identity, it should not be used in production. If used in production, it triggers warnings in Web browsers.
```

### Install a Self-signed Cert (optional)

A self-signed cert can be used for running https but there will still be warnings in the browser. This is only useful if DNS is not pointing to the installation site yet or when setting up a development environment.

```bash
cd /etc/pki/tls/certs
sudo ./make-dummy-cert localhost.crt
```

This generates a new file localhost.crt in the /etc/pki/tls/certs/ directory. The specified file name matches the default that is assigned in the SSLCertificateFile directive in /etc/httpd/conf.d/ssl.conf.

```bash
sudo vi /etc/httpd/conf.d/ssl.conf
```

Comment out SSLCertificateKeyFile /etc/pki/tls/private/localhost.key/

```bash
sudo systemctl restart httpd
```

https://cctapp.org/phpinfo.php - works but gives a lot of warnings!

## Install Real SSL Certificate 

Refer to https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/SSL-on-amazon-linux-2.html#letsencrypt

```bash
sudo wget -r --no-parent -A 'epel-release-*.rpm' https://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/e/
sudo rpm -Uvh dl.fedoraproject.org/pub/epel/7/x86_64/Packages/e/epel-release-*.rpm
sudo yum-config-manager --enable epel*
sudo yum repolist all
```

Edit the main Apache configuration file, /etc/httpd/conf/httpd.conf

```bash
sudo vi /etc/httpd/conf/httpd.conf
```
Add

```conf
<VirtualHost *:80>
    DocumentRoot "/var/www/html"
    ServerName "cctapp.org"
    ServerAlias "www.cctapp.org"
</VirtualHost>
```

Restart Apache

```bash
sudo systemctl restart httpd
```

## Install and Run Certbot

Certbot will install the SSL certificate into Apache and take care of renewals

```bash
sudo yum install -y certbot python2-certbot-apache
sudo certbot
```

Register paul@inkbear.com
Select both 1,2

Response:

```bash
IMPORTANT NOTES:

 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/cctapp.org/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/cctapp.org/privkey.pem
   Your certificate will expire on 2021-08-02. To obtain a new or
   tweaked version of this certificate in the future, simply run
   certbot again with the "certonly" option. To non-interactively
   renew *all* of your certificates, run "certbot renew"

### Automate Certbot

Add Certbot to the cron file

```bash
sudo crontab -e
42 2,14 * * * root certbot renew --no-self-upgrade
:wq
```

Check the cert in the browser

## Make an Image in AWS

This is a build check point.

# Install Laravel Framework

## Install Composer

Install composer as per Amazon instructions.

    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
    php -r "if (hash_file('sha384', 'composer-setup.php') === '572cb359b56ad9ae52f9c23d29d4b19a040af10d6635642e646a7caa7b96de717ce683bd797a92ce99e5929cc51e7d5f') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
    php composer-setup.php
    php -r "unlink('composer-setup.php');"

If less than 2GB memory are ont he AWS instance configure a swap file for 2GB.

    sudo /bin/dd if=/dev/zero of=/var/swap.1 bs=1M count=2048
    sudo /sbin/mkswap /var/swap.1
    sudo chmod 600 /var/swap.1
    sudo /sbin/swapon /var/swap.1

## Install Laravel

Use Composer to intall Laravel framework. You will also need to generate a key for sessions which writes to the .env file and modify ownership so Apache can write files.

    composer create-project laravel/laravel cctapp "7.*" --prefer-dist
    php artisan key:generate
    sudo chown -R apache storage
    sudo chown -R apache hooks

Load the root URL and see if Laravel is displayed.

## Add Voyager Admin and Front End

Use composer to install and configure the modules.

    composer require tcg/voyager
    composer require pvtl/voyager-frontend
    composer install 

## Create and Load the Database

Create database cctapp as UTF8 MB4 ci and import the data. Using PHPMyAdmin is recommended.

## Configure the Environment File

Update .env for appname and db credentials. The site should start forming but may be wonky until the files are uploaded.

## Upload Files

Upload the app files and anything modified. This will use Git Hub later.

Test out the site.
