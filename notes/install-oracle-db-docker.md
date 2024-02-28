
## Installing Oracle DB Image on docker via Oracle Container Registry

Oracle Container Registry : 
    
    https://container-registry.oracle.com/ords/f?p=113:1:106386474561346:::1:P1_BUSINESS_AREA:3&cs=3NeuwSCIQrMvqEDjW93Niv9PnX5CduZXydCmzGVsDSHFOXdg7yGe7zBI-T6fblwt0hmoagdKlsA1nQXEd9KmbWw


Oracle XE Release 21c Docker Image Link : 

    https://container-registry.oracle.com/ords/f?p=113:4:106386474561346:::4:P4_REPOSITORY,AI_REPOSITORY,AI_REPOSITORY_NAME,P4_REPOSITORY_NAME,P4_EULA_ID,P4_BUSINESS_AREA_ID:803,803,Oracle%20Database%20Express%20Edition,Oracle%20Database%20Express%20Edition,1,0&cs=3RikTwiJsA4i7LdiNPf3hRrqlQTZZpQal4jKX6603frUbGMMdLXj2ZlQ0RLFohH-VJ0CBt860dSltJ7slGPqE-A


### 1. Pull Latest Image

    sudo docker pull container-registry.oracle.com/database/express:latest

    $ sudo docker images
        REPOSITORY                                       TAG       IMAGE ID       CREATED         SIZE
        container-registry.oracle.com/database/express   latest    8da8cedb7fbf   6 months ago    11.4GB
        hello-world                                      latest    d2c94e258dcb   10 months ago   13.3kB

Note : running docker commands without sudo each time :

        $ sudo -s
        root@arjunb-Vostro-3480:/home/arjunb# docker images
        REPOSITORY                                       TAG       IMAGE ID       CREATED         SIZE
        container-registry.oracle.com/database/express   latest    8da8cedb7fbf   6 months ago    11.4GB
        hello-world                                      latest    d2c94e258dcb   10 months ago   13.3kB

### 2. Starting an Oracle Database Server Instance

2.1 Run oracle image - 

    $ docker run -d --name oracle-db-xe container-registry.oracle.com/database/express
    0e3fc10f515e6fd8b1b71e364bb4f0e627777129f07cd3ee6aaf72aa24d492a1

2.2 check status - The Oracle Database server is ready to use when the STATUS field shows (healthy) in the docker ps output.

   $ docker ps
    CONTAINER ID   IMAGE                                            COMMAND                  CREATED              STATUS                        PORTS  NAMES
    0e3fc10f515e   container-registry.oracle.com/database/express   "/bin/bash -c $ORACL…"   About a minute ago   Up About a minute (healthy)             oracle-db-xe

Note :When the container is started, a random password is used for the SYS, SYSTEM and PDBADMIN users. This is termed as the default password.

### 3. Changing the Default Password for SYS User 

Note that the container must be running before you run the script.

    $ docker exec oracle-db-xe ./setPassword.sh Admin@1234

Output :

    The Oracle base remains unchanged with value /opt/oracle

    SQL*Plus: Release 21.0.0.0.0 - Production on Tue Feb 27 15:51:45 2024
    Version 21.3.0.0.0

    Copyright (c) 1982, 2021, Oracle.  All rights reserved.


    Connected to:
    Oracle Database 21c Express Edition Release 21.0.0.0.0 - Production
    Version 21.3.0.0.0

    SQL> 
    User altered.

    SQL> 
    User altered.

    SQL> 
    Session altered.

    SQL> 
    User altered.

    SQL> Disconnected from Oracle Database 21c Express Edition Release 21.0.0.0.0 - Production
    Version 21.3.0.0.0

### 4. Database Alert Logs
You can access the database alert log by using the following command, where <oracle-db> is the name of the container: $ docker logs <oracle-db>

    $ docker logs oracle-db-xe

### 5. Connecting from Within the Container
You can connect to the Oracle Database server by running a SQL*Plus command from within the container using one of the following commands:

    $ docker exec -it <oracle-db> sqlplus / as sysdba
    $ docker exec -it <oracle-db> sqlplus sys/<your_password>@XE as sysdba
    $ docker exec -it <oracle-db> sqlplus system/<your_password>@XE
    $ docker exec -it <oracle-db> sqlplus pdbadmin/<your_password>@XEPDB1

Example : 

    arjunb@arjunb-Vostro-3480:~$ sudo docker exec -it oracle-db-xe sqlplus sys/admin1234@XE as sysdba

    SQL*Plus: Release 21.0.0.0.0 - Production on Tue Feb 27 17:26:16 2024
    Version 21.3.0.0.0

    Copyright (c) 1982, 2021, Oracle.  All rights reserved.


    Connected to:
    Oracle Database 21c Express Edition Release 21.0.0.0.0 - Production
    Version 21.3.0.0.0

    SQL> exit


---------------------
### New Schema/User Creation :



root@arjunb-Vostro-3480:/home/arjunb# docker exec -it oracle-db-xe sqlplus sys/admin1234@XE as sysdba

SQL*Plus: Release 21.0.0.0.0 - Production on Tue Feb 27 18:23:16 2024
Version 21.3.0.0.0

Copyright (c) 1982, 2021, Oracle.  All rights reserved.


Connected to:
Oracle Database 21c Express Edition Release 21.0.0.0.0 - Production
Version 21.3.0.0.0

#### Create USER :

SQL> alter session set "_ORACLE_SCRIPT"=true;

Session altered.

SQL> create user mng_pics identified by 123456789 
  2  ;

User created.

SQL> select username from dba_users;


#### Create tablespace :

SQL> create tablespace mngpics_tabspace
  2  datafile 'mngpics_tabspace.dat'
  3  size 50M autoextend on;

Tablespace created.



#### Create temporary tablespace :

SQL> create temporary tablespace mngpics_tabspace_temp
  2  tempfile 'mngpics_tabspace_temp.dat'
  3  size 20M autoextend on;

Tablespace created.

#### Re-Create User with table space :

SQL> drop user mng_pics;

User dropped.


SQL> create user mng_pics identified by 123456789 
  2  default tablespace mngpics_tabspace
  3  temporary tablespace mngpics_tabspace_temp;

User created.

#### Grant Some privileges :

SQL> grant create session to mng_pics;

Grant succeeded.

SQL> grant create table to mng_pics;

Grant succeeded.

SQL> grant unlimited tablespace to mng_pics;

Grant succeeded.
