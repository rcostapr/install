# Apache
-----------
## DEBIAN
----------

```
$ sudo apt install apache2
$ sudo ufw app list
$ sudo ufw allow 'Apache'
$ sudo ufw enable
$ sudo ufw default deny
```
Do:
```
$ sudo iptables -L
```
```
$ sudo systemctl status apache2
```
```
$ sudo nano /etc/apache2/apache2.conf
 ...
 ServerName srvph.simplesenergia.es
 ...
```


Then save and test the configuration with the following command:
```
$ apachectl configtest

    Syntax OK
```
Then you can restart the server and check you don't get the error message:
```
$ sudo service apache2 restart
```
---------
## CENTOS
---------
### Install Apache
```
$ sudo yum update
$ sudo yum -y install httpd
```
### Start and Manage Apache Web Server
```
$ sudo systemctl start httpd
```

### To configure Apache to run on startup:
```
$ sudo systemctl enable httpd
 Created symlink /etc/systemd/system/multi-user.target.wants/httpd.service → /usr/lib/systemd/system/httpd.service.

```

### To disable Apache at system startup:
```
$ sudo systemctl disable httpd
```

### Status of the Apache service:status of the Apache service:

```
$ sudo systemctl status httpd

    httpd.service - The Apache HTTP Server
    Loaded: loaded (/usr/lib/systemd/system/httpd.service; disabled; vendor preset: disabled)
    Drop-In: /usr/lib/systemd/system/httpd.service.d
            └─php-fpm.conf
    Active: active (running) since Sun 2022-01-30 13:17:30 EST; 7min ago
        Docs: man:httpd.service(8)
    Main PID: 3032 (httpd)
    Status: "Running, listening on: port 443, port 80"
        Tasks: 214 (limit: 49478)
    Memory: 32.8M
    CGroup: /system.slice/httpd.service
            ├─3032 /usr/sbin/httpd -DFOREGROUND
            ├─3059 /usr/sbin/httpd -DFOREGROUND
            ├─3060 /usr/sbin/httpd -DFOREGROUND
            ├─3061 /usr/sbin/httpd -DFOREGROUND
            ├─3062 /usr/sbin/httpd -DFOREGROUND
            └─3063 /usr/sbin/httpd -DFOREGROUND

    jan 30 13:17:00 srvcentos.neohome systemd[1]: Starting The Apache HTTP Server...
    jan 30 13:17:30 srvcentos.neohome systemd[1]: Started The Apache HTTP Server.
    jan 30 13:18:05 srvcentos.neohome httpd[3032]: Server configured, listening on: port 443, port 80

```

### Test Apache Web Server
```
$ hostname -I | awk '{print $1}'
```

### Adjust Firewall for Apache

```
$ sudo firewall-cmd --permanent --zone=public --add-service=http
$ sudo firewall-cmd --permanent --zone=public --add-service=https
$ sudo firewall-cmd --reload
$ sudo firewall-cmd --list-all | grep services
```


### Apache Files and Directories
Apache is controlled by applying directives in configuration files:

    /etc/httpd/conf/httpd.conf – Main Apache config file
    /etc/httpd/ – Location for all config files
    /etc/httpd/conf.d/ – All config files in this directory are included in the main confog file
    /etc/httpd/conf.modules.d/ – Location for Apache module config files

```
$ nano /etc/httpd/conf/httpd.conf
    ...
    DocumentRoot "/var/www/html"
    ...
```

## Create New Virtual Host Files

```
$ sudo mkdir /etc/httpd/sites-available
$ sudo mkdir /etc/httpd/sites-enabled
$ sudo nano /etc/httpd/conf/httpd.conf
```
```bash
# Add this line to the end of the file:
IncludeOptional sites-enabled/*.conf
```

## Create the First Virtual Host File
Note: Due to the configurations that we have outlined, all virtual host files must end in .conf.

Start by opening the new file in your editor with root privileges:
```
$ sudo nano /etc/httpd/sites-available/example.com.conf
```

```
<VirtualHost *:80>
    ServerAdmin admin@localhost
    DocumentRoot /var/www/example.com/public_html
    ServerName www.example.com
    ServerAlias example.com
    ErrorLog /var/www/example.com/error.log
    CustomLog  /var/www/example.com/requests.log combined|common
    <Directory /var/www/example.com/public_html>
        Options +ExecCGI -Indexes
        AddHandler cgi-script .cgi	.py
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```

```
$ sudo nano /etc/httpd/sites-available/example.com-ssl.conf
```
```
<VirtualHost *:443>
    ServerAdmin admin@localhost
    DocumentRoot /var/www/example.com/public_html
    ServerName www.example.com
    ServerAlias example.com
    ErrorLog /var/www/example.com/error.log
    CustomLog  /var/www/example.com/requests.log combined|common
    <Directory /var/www/example.com/public_html>
        Options +ExecCGI -Indexes
        AddHandler cgi-script .cgi	.py
        AllowOverride All
        Require all granted
    </Directory>
    SSLEngine on
	SSLCertificateFile /var/www/example.com/ssl.cert
	SSLCertificateKeyFile /var/www/example.com/ssl.key
	SSLCertificateChainFile /var/www/example.com/ssl.ca
</VirtualHost>
```

## Enable the New Virtual Host Files
create a symbolic link for each virtual host in the sites-enabled directory:
```bash
$ sudo ln -s /etc/httpd/sites-available/example.com.conf /etc/httpd/sites-enabled/example.com.conf
$ sudo ln -s /etc/httpd/sites-available/example.com-ssl.conf /etc/httpd/sites-enabled/example.com-ssl.conf
```

### Restart Apache to make these changes take effect:
```
$ sudo apachectl restart
```

## Set Up Local Hosts File (Optional)
```
$ sudo nano /etc/hosts
    127.0.0.1   localhost
    127.0.1.1   guest-desktop
    server_ip_address example.com
```

## Test Your Results
    http://example.com


# ModSecurity
https://phoenixnap.com/kb/setup-configure-modsecurity-on-apache

```
$ sudo apt install libapache2-mod-security2
```
## Configure ModSecurity
1. Copy and rename the file:

sudo cp /etc/modsecurity/modsecurity.conf-recommended /etc/modsecurity/modsecurity.conf

2. Next, change the ModSecurity detection mode. First, move into the /etc/modsecurity folder:

sudo cd /etc/modsecurity

3. Open the configuration file in a text editor (we will be using nano):

sudo nano modsecurity.conf

Near the top, you should see an entry labeled:

SecRuleEngine DetectionOnly

Change this to read as follows:

SecRuleEngine On

4. Use CTRL+X to exit, then press y then Enter to save the changes.

5. Navigate away from the /etc/modsecurity folder:

cd

6. Restart Apache:

On Debian/Ubuntu

sudo systemctl restart apache2
