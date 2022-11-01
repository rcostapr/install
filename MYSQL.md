# Mysql
-------------------------
## Install the MySQL server by using the Ubuntu operating system package manager:
```bash
$ sudo apt-get update
$ sudo apt-get install mysql-server
```
## The installer installs MySQL and all dependencies.

If the secure installation utility does not launch automatically after the installation completes, enter the following command:
```
$ sudo mysql_secure_installation utility
```
This utility prompts you to define the mysql root password and other security-related options, including removing remote access to the root user and setting the root password.
Allow remote access

If you have iptables enabled and want to connect to the MySQL database from another machine, you must open a port in your server’s firewall (the default port is 3306). You don’t need to do this if the application that uses MySQL is running on the same server.

##  Run the following command to allow remote access to the mysql server:

```
$ sudo ufw enable
$ sudo ufw allow mysql
```

## Definir o root para autenticar-se com o mysql_native_password:
```
$ sudo mysql
mysql > ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
mysql > FLUSH PRIVILEGES;
mysql > exit;
```

## Start the MySQL service

After the installation is complete, you can start the database service by running the following command. If the service is already started, a message informs you that the service is already running:
```
$ sudo systemctl start mysql
```
## Launch at reboot

To ensure that the database server launches after a reboot, run the following command:
```
$ sudo systemctl enable mysql
```

## Workbench
### Snap Install
```
$ snap install mysql-workbench-community
```
#### To store passwords
```
$ snap connect mysql-workbench-community:password-manager-service
$ snap connect mysql-workbench-community:ssh-keys
```
