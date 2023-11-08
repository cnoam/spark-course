
status : tested on ubuntu.

# Installing Postgres in Docker

## in Windows
Use docker:

* install Docker Desktop
* signup to dockerhub.com and signin
* continue like in ubuntu

## Installing Postgres in ubuntu (from command line)
Just run the next step! The DB engine will be downloaded automatically by docker.

# Preparing the database
It is easy to create the database (once) from the command line.

**Remember that once a Docker container is removed, all its data is lost UNLESS the data is stored outside the container**

The data for this container is stored in the host (it is declared in the docker-compose.yml) so the database will persist between activations of the container.


We first need to start the container and then connect to the running container:

In terminal 1: 
```
cd postgres_in_docker # if you are not already in this directory
docker-compose up -d
```
<br>Note: if you get an error complaining on missing network, retry with `docker-compose up -d --force-recreate`

Then, after the command finishes (and the DB runs in the background), run (in the same terminal):

```docker exec -it psqlserver psql -U postgres```

This will open the database management console.

Once in the psql console:

```
postgres=# ALTER USER postgres PASSWORD 'postgres';
postgres=# CREATE DATABASE bids_db;
postgres=# \connect bids_db;
```
And see which databases are here:
```
bids_db=# \list
                                                List of databases
   Name    |  Owner   | Encoding |  Collate   |   Ctype    | ICU Locale | Locale Provider |   Access privileges   
-----------+----------+----------+------------+------------+------------+-----------------+-----------------------
 bids_db   | postgres | UTF8     | en_US.utf8 | en_US.utf8 |            | libc            | 
 postgres  | postgres | UTF8     | en_US.utf8 | en_US.utf8 |            | libc            | 
 template0 | postgres | UTF8     | en_US.utf8 | en_US.utf8 |            | libc            | =c/postgres          +
           |          |          |            |            |            |                 | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.utf8 | en_US.utf8 |            | libc            | =c/postgres          +
           |          |          |            |            |            |                 | postgres=CTc/postgres
(4 rows)
```




## Read CSV file into a table

In the PSQL shell:
```
create table players(Name varchar, age int, occupation varchar);
\copy players from '/data/small.csv' csv header;
```

Verify the data is loaded:
```
select * from players;
  name   | age | occupation  
---------+-----+-------------
 Alice   |  25 |  Rocker
 Bob     |  30 |  Assasin
 Charlie |  50 |  politician
 David   |  10 |  racer
(4 rows)

```
<hr>
In postgress AUTOCOMMIT is ON by default, so every insert is committed.<br>
Disable it by using manual transaction ( add BEGIN )
or in psql console:

```
bids_db-# \set AUTOCOMMIT off
bids_db-# \echo :AUTOCOMMIT
off
```

### Useful (postgres) SQL commands for your reference
```
\h       help for SQL commands
TRUNCATE TABLE bids;
DROP TABLE bids;
DROP DATABASE bids_db;
select count(*) from bids;
```

### Useful postgres commands
```
\?                   help for psql cli commands <br>
\d+ tablename        print the columns <br>
\c dbname            connect
```

# Cleaning up

* To stop the postgres server: `docker-compose down` from the directory containing the yml file.
* To remove the image: `docker rmi postgres`
* To remove the docker network (if it is not in use): `docker network prune`
* To remove the storage where the database is stored: `docker volume rm postgres_in_docker_pgdata`
