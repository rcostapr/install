# Install PHP

--------

## WINDOWS
- Download Zip File from https://windows.php.net/download#php-8.1
- Extract to c:\php
- Add c:\php to PATH
- Test with
    -   php --version
    ```
   PHP 8.1.2 (cli) (built: Jan 19 2022 10:18:23) (ZTS Visual C++ 2019 x64)                                                 
   Copyright (c) The PHP Group                                                                                             
   Zend Engine v4.1.2, Copyright (c) Zend Technologies   
    ```
### Install Extensions
copy c:\php\php.ini-development c:\php\php.ini
### Install Composer
https://getcomposer.org/download/
```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === '906a84df04cea2aa72f40b5f787e49f22d4c2f19492ac310e8cba5b96ac8b64115ac402c8cd292b8a03482574915d1a8') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
```
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
