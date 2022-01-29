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
    sudo mkdir /var/lib/postgres/data
    ```
 - step 2: set /var/lib/postgres/data ownership to user 'postgres'

    ```
    chown postgres /var/lib/postgres/data
    ```

 - step 3: As user 'postgres' start the database.
    ```
    sudo -i -u postgres
    initdb  -D '/var/lib/postgres/data'
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
    $ su - postgres -c "initdb --locale en_US.UTF-8 -D '/var/lib/postgres/data'"
    ```