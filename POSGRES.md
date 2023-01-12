# POSGRES

## List
-------
```bash
$ dnf module list postgresql
```

## Enable Version
-------
```bash
$ sudo dnf module enable postgresql:13
$ sudo dnf module install postgresql:13
```
- Ubuntu
```
$ sudo apt install -y postgresql postgresql-contrib postgresql-client
$ sudo dpkg --status postgresql
```

## Remove Posgres
-------------
```
dnf module reset postgresql
```

## Show Status
--------------
```bash
service postgresql status
```

## Start Service
----------------
```
service postgresql start
```
## Get Client Version
```
$ locate bin/psql
/usr/bin/psql
/usr/lib/postgresql/10/bin/psql
/usr/lib/postgresql/11/bin/psql
/usr/lib/postgresql/12/bin/psql
/usr/lib/postgresql/13/bin/psql
/usr/lib/postgresql/14/bin/psql
$ psql -V
psql (PostgreSQL) 14.0 (Ubuntu 14.0-1.pgdg18.04+1) 
```

## Problem
----------------

```
Redirecting to /bin/systemctl start postgresql.service
Job for postgresql.service failed because the control process exited with error code.
See "systemctl status postgresql.service" and "journalctl -xe" for details.
```

### Following step solved your problem

 - step 1: create the data directory (acordingly with the PGROOT variable set before in the config file)

    ```
    sudo mkdir /var/lib/pgsql/data
    ```
 - step 2: set /var/lib/pgsql/data ownership to user 'postgres'

    ```
    chown postgres /var/lib/pgsql/data
    ```

 - step 3: As user 'postgres' start the database.
    ```
    su posgres
    initdb  -D '/var/lib/pgsql/data'
    ```

    ```
    bash-4.4$ initdb  -D '/var/lib/pgsql/data'
    The files belonging to this database system will be owned by user "postgres".
    This user must also own the server process.

    The database cluster will be initialized with locales
    COLLATE:  en_US.UTF-8
    CTYPE:    pt_PT.UTF-8
    MESSAGES: en_US.UTF-8
    MONETARY: pt_PT.UTF-8
    NUMERIC:  pt_PT.UTF-8
    TIME:     pt_PT.UTF-8
    The default database encoding has accordingly been set to "UTF8".
    The default text search configuration will be set to "portuguese".

    Data page checksums are disabled.

    fixing permissions on existing directory /var/lib/pgsql/data ... ok
    creating subdirectories ... ok
    selecting default max_connections ... 100
    selecting default shared_buffers ... 128MB
    selecting default timezone ... America/New_York
    selecting dynamic shared memory implementation ... posix
    creating configuration files ... ok
    running bootstrap script ... ok
    performing post-bootstrap initialization ... ok
    syncing data to disk ... ok

    WARNING: enabling "trust" authentication for local connections
    You can change this by editing pg_hba.conf or using the option -A, or
    --auth-local and --auth-host, the next time you run initdb.

    Success. You can now start the database server using:

        pg_ctl -D /var/lib/pgsql/data -l logfile start

    bash-4.4$ 

    ```

 - step 4: start the service as root

    ```
    postgresql-setup --initdb
    systemctl restart postgresql.service
    systemctl enable postgresql.service
    ```


 - step 5: After setting a password for the postgres user:
    ```
    $ sudo passwd postgres 
    ```

 - step 6: run the fallowing command:
    ```
    $ su - postgres -c "initdb --locale en_US.UTF-8 -D '/var/lib/pgsql/data'"
    ```


# Install pgAdmin4 (APT)

## Setup the repository


### Install the public key for the repository (if not done previously):
```
sudo curl https://www.pgadmin.org/static/packages_pgadmin_org.pub | sudo apt-key add
```

## Create the repository configuration file:
```
sudo sh -c 'echo "deb https://ftp.postgresql.org/pub/pgadmin/pgadmin4/apt/$(lsb_release -cs) pgadmin4 main" > /etc/apt/sources.list.d/pgadmin4.list && apt update'
```

## Install pgAdmin

### Install for both desktop and web modes:
```
sudo apt install pgadmin4
```

### Install for desktop mode only:
```
sudo apt install pgadmin4-desktop
```


### Install for web mode only: 
```
sudo apt install pgadmin4-web 
```

### Configure the webserver, if you installed pgadmin4-web:
```
sudo /usr/pgadmin4/bin/setup-web.sh
```

## Open Port for Comunications
------
```bash
$ firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3
  sources: 
  services: cockpit dhcpv6-client ssh
  ports: 
  protocols: 
  forward: no
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules: 

$ firewall-cmd --get-services
    ... pop3 pop3s postgresql privoxy prometheus ....

$ firewall-cmd --get-zones
  block dmz drop external home internal nm-shared public trusted work
# For example let’s open postgresql service port for zone public:
firewall-cmd --zone=public --permanent --add-service=postgresql
# OR  preconfigured services use the --add-port option. For example let’s open TCP port 8080 for zone public:
$ firewall-cmd --zone=public --permanent --add-port 5432/tcp
# Reload
$ firewall-cmd --reload
# Confirm that port or service was opened successfully:
$ firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3
  sources: 
  services: cockpit dhcpv6-client postgresql ssh
  ports: 5432/tcp
  protocols: 
  forward: no
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules: 

```
```
sudo ufw allow 5432/tcp
iptables -I INPUT 1 -m tcp -p tcp --dport 5432 -j ACCEPT 
service iptables save 
service iptables restart 
```

### Edit postgresql.conf
------

```
sudo su - postgres
locate postgresql.conf
```
```
    postgres@srvcentos ~]$ locate postgresql.conf
    /etc/pcp/postgresql/pmdapostgresql.conf
    /etc/postgresql-setup/upgrade/postgresql.conf
    /usr/lib/tmpfiles.d/pcp-pmda-postgresql.conf
    /usr/lib/tmpfiles.d/postgresql.conf
    /usr/share/pgsql/postgresql.conf.sample
    /var/lib/pcp/pmdas/postgresql/pmdapostgresql.conf
    /var/lib/pgsql/data/postgresql.conf
```

```
nano /var/lib/pgsql/data/postgresql.conf
```

```
#------------------------------------------------------------------------------
# CONNECTIONS AND AUTHENTICATION
#------------------------------------------------------------------------------

# - Connection Settings -

listen_addresses = '*'          # what IP address(es) to listen on;
                                        # comma-separated list of addresses;
                                        # defaults to 'localhost'; use '*' for all
                                        # (change requires restart)
port = 5432                             # (change requires restart)
max_connections = 100                   # (change requires restart)

```

### Check Port Status
```
sudo lsof -i -P -n | grep LISTEN
sudo netstat -tulpn | grep LISTEN
[root@srvcentos ~]# netstat -na | grep 5432
tcp        0      0 0.0.0.0:5432            0.0.0.0:*               LISTEN     
tcp6       0      0 :::5432                 :::*                    LISTEN     
unix  2      [ ACC ]     STREAM     LISTENING     194118   /var/run/postgresql/.s.PGSQL.5432
unix  2      [ ACC ]     STREAM     LISTENING     194120   /tmp/.s.PGSQL.5432

```

------
## Create USER
-----
Creating user, database and adding access on PostgreSQL

### Using PGSQL shell
```
sudo -u postgres psql
```

#### Run
```bash
postgres=# create database mydb;
postgres=# create user myuser with encrypted password 'mypass';
postgres=# grant all privileges on database mydb to myuser;
# Exit
postgres=# \q
```

### Using PGSQL utility binaries
- Creating user
```
$ sudo -u postgres createuser <username>
```
- Creating Database
```
$ sudo -u postgres createdb <dbname>
```
- Giving the user a password
```
$ sudo -u postgres psql
psql=# alter user <username> with encrypted password '<password>';
```
- Granting privileges on database
```
psql=# grant all privileges on database <dbname> to <username> ;
```


## Testing Connection
---------

### Using NMAP check port connectivity
```
 nmap -p <port> <host>
```
```
$ nmap -p 5432 172.19.0.1
Starting Nmap 7.80 ( https://nmap.org ) at 2022-01-29 17:50 WET
Nmap scan report for feup (172.19.0.1)
Host is up (0.00044s latency).

PORT     STATE  SERVICE
5432/tcp closed postgresql

Nmap done: 1 IP address (1 host up) scanned in 10.49 seconds

```

# Where is psql
```
$ which psql
$ whereis psql
$ ps aux | grep postgres
```

# Login to the PostgreSQL Server
Default PostgreSQL Superuser "postgres"

The PostgreSQL installation creates a "UNIX USER" called postgres, who is ALSO the "Default PostgreSQL's SUPERUSER". The UNIX USER postgres cannot login interactively to the system, as its password is not enabled.
Login as PostgreSQL Superuser "postgres" via "psql" Client

To run psql using "UNIX USER" postgres, you need to invoke "sudo -u postgres psql", which switches to "UNIX USER" postgres and run the psql command.
```
$ sudo -u postgres psql
   -- Run command "psql" as UNIX USER "postgres".
   -- Enter the CURRENT SUPERUSER password for sudo.
psql (10.6 (Ubuntu 10.6-0ubuntu0.18.04.1))
Type "help" for help.

postgres=# help 
You are using psql, the command-line interface to PostgreSQL.
Type:  \copyright for distribution terms
       \h for help with SQL commands
       \? for help with psql commands
       \g or terminate with semicolon to execute query
       \q to quit

-- Display version
postgres=# SELECT version();
     version                                                   
---------------------------------------------------
 PostgreSQL 10.6 (Ubuntu 10.6-0ubuntu0.18.04.1) ...
press q to quit display

-- Quit
postgres=# \q
```

# Change Peer authentication to Password authentication
## Peer authentication

    The peer authentication method works by obtaining the client's operating system user name from the kernel and using it as the allowed database user name (with optional user name mapping). This method is only supported on local connections.

## Password authentication

    The password-based authentication methods are md5 and password. These methods operate similarly except for the way that the password is sent across the connection, namely MD5-hashed and clear-text respectively.

    If you are at all concerned about password "sniffing" attacks then md5 is preferred. Plain password should always be avoided if possible. However, md5 cannot be used with the db_user_namespace feature. If the connection is protected by SSL encryption then password can be used safely (though SSL certificate authentication might be a better choice if one is depending on using SSL).

## Change pg_hba.conf file.

```
$ locate pg_hba.conf
/usr/share/pgsql/pg_hba.conf.sample
/var/lib/pgsql/data/pg_hba.conf
```

```
$ nano /var/lib/pgsql/data/pg_hba.conf
```
```
This line:

local   all             postgres                                peer

Should be:

local   all             postgres                                md5

```

```
# TYPE  DATABASE        USER            ADDRESS                 METHOD

# "local" is for Unix domain socket connections only
local   all             all                                     md5
# IPv4 local connections:
host    all             all             127.0.0.1/32            md5  
# IPv6 local connections:
host    all             all             ::1/128                 md5  
```

## Login With
postgresql client
```bash
# install
$ sudo apt install postgresql-client
```
```
$ psql -h 127.0.0.1 -U postgres

$ psql -U postgres

$ psql -h postgres.example.com -U postgres -W 
$ psql -h 172.19.0.1 -U postgres -W
$ psql -h 172.19.0.1 -p 55432 -U postgres -W
$ psql -h localhost -p 5432 -U postgres testdb
```

## Commands
### Show Databases
```
postgres=# \l
postgres=# \l+
```

### Connect to Database
```
postgres=# \c sales
Password: 
psql (14.0 (Ubuntu 14.0-1.pgdg18.04+1), server 9.6.22)
You are now connected to database "sales" as user "postgres".

```

### Show Tables
```
sales=# CREATE TABLE leads (id INTEGER PRIMARY KEY, name VARCHAR);
CREATE TABLE
sales=# \dt
  List of relations
 Schema | Name  | Type  |  Owner   
--------+-------+-------+----------
 public | leads | table | postgres
(1 row)

```


# Incoming Connection
```bash
# fedora or centos
$ yum install iftop -y

# ubuntu or debian
$ sudo apt-get install iftop

# Centos (base repo)
$ yum install iptraf

# fedora or centos (with epel)
$ yum install iptraf-ng -y

# ubuntu or debian
$ sudo apt-get install iptraf iptraf-ng
```
```
$ sudo iptraf
$ sudo iftop -n
$ pktstat -n
```


# How to close ports on RHEL 8 / CentOS 8 Linux step by step instructions



    First check for already opened ports or services. Take a note of the zone, protocol as well as port or service you wish to close:

    # firewall-cmd --list-all

    Close port or service. The below command will close the http service in the public zone:

    # firewall-cmd --zone=public --permanent --remove-service http

    In case you wish to close a specific port use the --remove-port option. For example let’s close the TCP 8080 port:

    # firewall-cmd --zone=public --permanent --remove-port 8080

    Reload the firewall settings:

    # firewall-cmd --reload

    Confirm that port or service was closed successfully:

    # firewall-cmd --list-all

# PostgreSQL: Reset password of PostgreSQL on Ubuntu
- Login with psql as the postgres superuser with this shell command:

```
$ sudo -u postgres psql 

```
- Run SQL command:
```
postgres=# ALTER USER postgres PASSWORD 'newpassword';
```
