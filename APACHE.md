## Apache
=========
$ sudo apt install apache2
$ sudo ufw app list
$ sudo ufw allow 'Apache'
$ sudo ufw enable
$ sudo ufw default deny

And I then do:

$ sudo iptables -L

$ sudo systemctl status apache2

$ sudo nano /etc/apache2/apache2.conf
ServerName srvph.simplesenergia.es

Then save and test the configuration with the following command:

$ apachectl configtest

You should get:

    Syntax OK

Then you can restart the server and check you don't get the error message:

$ sudo service apache2 restart