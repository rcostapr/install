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
- copy c:\php\php.ini-development c:\php\php.ini
- edit php.ini
- uncomment Dynamic Extensions

### Install Composer
https://getcomposer.org/download/
cd c:\php
```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === '906a84df04cea2aa72f40b5f787e49f22d4c2f19492ac310e8cba5b96ac8b64115ac402c8cd292b8a03482574915d1a8') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
echo @php "%~dp0composer.phar" %*>composer.bat
php -r "unlink('composer-setup.php');"
```
composer global require squizlabs/php_codesniffer
 
C:/Users/adm_r.costa/AppData/Roaming/Composer

C:\Users\adm_r.costa\AppData\Roaming\Composer\vendor\squizlabs\php_codesniffer\bin\phpcs

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
$ sudo apt install php8.1-{common,mysql,sqlite3,xml,xmlrpc,curl,gd,imagick,cli,dev,imap,mbstring,opcache,soap,zip,intl,bcmath,opcache,xdebug,apcu} -y
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

## CENTOS

-------
### CENTOS 8
```
$ sudo dnf install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm

$ sudo dnf module list php

$ dnf module reset php
$ dnf module install php:remi-7.4
$ dnf update
```
### CENTOS 7
#### Installing PHP version 7.3
1. PHP 7.3 is available for CentOS 7 and Fedora distributions from the Remi repository. Add it to your system by running
```
$ sudo yum -y install http://rpms.remirepo.net/enterprise/remi-release-7.rpm 
$ sudo yum -y install epel-release yum-utils
```
2. Turn on Remi repo i.e.remi-php73:
```
$ sudo yum-config-manager --enable remi-php73
```
 - To disable previous repo:
    ```
    $ sudo yum-config-manager --disable remi-php54
    ```
__Refresh repository:__
```
$ sudo yum update
```
3. Install PHP 7.3 on CentOS 7 / Fedora
```
$ sudo yum -y install php php-cli php-fpm php-mysqlnd php-zip php-devel php-gd php-mcrypt php-mbstring php-curl php-xml php-pear php-bcmath php-json
```
4. Check Version
```
$ php -v
```
#### Installing other PHP 7.3 Extensions
Install syntax:
```
$ sudo yum install php-<entension-name>
```
As an example, to install a mysql module for PHP applications that use MySQL databases, you’ll run:
```
sudo yum install php-mysqlnd
```
__Confirm:__
```
$ rpm -qi php-mysqlnd
```
