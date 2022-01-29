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