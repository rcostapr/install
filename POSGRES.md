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
```
sudo ufw allow 5432/tcp
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

### Listen Ports
```
sudo lsof -i -P -n | grep LISTEN
sudo netstat -tulpn | grep LISTEN
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
```