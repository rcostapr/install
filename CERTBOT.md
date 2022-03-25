# Install Certbot
https://www.inmotionhosting.com/support/website/ssl/lets-encrypt-ssl-ubuntu-with-certbot/

First, install PIP:
Copy
sudo apt install python3 python3-venv libaugeas0
Set up a virtual environment:
Copy
sudo python3 -m venv /opt/certbot/
Copy
sudo /opt/certbot/bin/pip install --upgrade pip
Install Certbot on Apache (or NGINX):
Copy
sudo /opt/certbot/bin/pip install certbot certbot-apache
Copy
sudo /opt/certbot/bin/pip install certbot certbot-nginx
Create a symlink to ensure Certbot runs:
Copy
sudo ln -s /opt/certbot/bin/certbot /usr/bin/certbot



Create an SSL Certificate with Certbot
Run Certbot to create SSL certificates and modify your web server configuration file to automatically redirect HTTP requests to HTTPS. Or, add “certonly” to create the SSL certificates without modifying system files (recommended if hosting multiple sites that may not receive an SSL).

Create SSL certs for all domains and configure redirects in the web server:
Copy
sudo certbot --apache
Copy
sudo certbot --nginx

Create SSL certs for a specified domain (recommended for using your system hostname):
Copy
sudo certbot --apache -d example.com -d www.example.com

Only install SSL certs:
Copy
sudo certbot certonly --apache
Copy
sudo certbot certonly --nginx
Enter an email address for renewal and security notices
Agree to the terms of service.
Specify whether to receive emails from EFF.
If prompted, choose whether to redirect HTTP traffic to HTTPS – 1 (no redirect, no further changes to the server) or 2 (redirect all requests to HTTPS).

sudo certbot --apache

phsrv.energiasimples.pt

Deploying certificate
Successfully deployed certificate for phsrv.energiasimples.pt to /etc/apache2/sites-available/000-default-le-ssl.conf
Congratulations! You have successfully enabled HTTPS on https://phsrv.energiasimples.pt

