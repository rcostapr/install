# Install PHP

--------

## DEBIAN

-------

## Install Openssl
```
$ sudo apt install openssl
```
--------

## Repo

```bash
$ sudo add-apt-repository ppa:ondrej/php
$ sudo apt-get update -y
```

## Install PHP base

```bash
$ sudo apt install php8.1 php8.1-fpm -y
```

## Install Extensions
```
$ sudo apt install php8.1-{common,mysql,xml,xmlrpc,curl,gd,imagick,cli,dev,imap,mbstring,opcache,soap,zip,intl,bcmath} -y
```

## Switching PHP Version

- Interactive switching mode
```bash
$ sudo update-alternatives --config php
$ sudo update-alternatives --config phar
$ sudo update-alternatives --config phar.phar
```

- Manual Mode
    - From PHP 7.4 => PHP 8.1
```bash
# Apache
$ sudo a2dismod php7.4
$ sudo a2enmod php8.1
$ systemctl restart apache2

# PHP cli
$ sudo update-alternatives --set php /usr/bin/php8.1

# To enable PHP 8.1 FPM in Apache2 do:
$ a2enmod proxy_fcgi setenvif
$ a2enconf php8.1-fpm
```

## Install Composer
--------------------
```bash
$ wget -O composer-setup.php https://getcomposer.org/installer
```
### Install Globally
```bash
$ sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer
```

## Install Phar
--------------------
```bash
$ wget http://pear.php.net/go-pear.phar
$ php go-pear.phar
```

## PHP Extra Extensions
------------------------
-   sudo apt-get install php-intl
-   sudo apt-get install graphviz
-   sudo apt-get install libssh2-1-dev
-   sudo apt-get install php-ssh2
-   sudo pecl install ssh2-1.2
-   sudo phpenmod xsl
-   sudo phpenmod dom
-   sudo phpenmod ssh2

--------

## CENTOS 8

-------

```
$ sudo dnf install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm

$ sudo dnf module list php

$ dnf module reset php
$ dnf module install php:remi-7.4
$ dnf update
```