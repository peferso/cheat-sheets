# Installation instructions

https://www.postgresql.org/download/linux/ubuntu/

# Start service

Works for WSL ubuntu 20

```sh
sudo service postgresql start
```

# Create user

```sh
sudo -u postgres createuser newuser
```

# Create database for user

```postgresql
create database newuser with owner = newuser;
grant all privileges on database newuser to newuser;
```

Grant additional permissions to create dbs if necessary:
```postgresql
alter user newuser createdb;
```

# Set password

Login as admin

```sh
sudo -u postgres psql
```

Change pass

```postgresql
alter user newuser with encrypted password 'apass'
```


# Edit hba_file

Login with postgres user:
```sh
sudo -u postgres psql
```
Find location of hba file
```postgresql
SHOW hba_file;
```
backup the file (example):
```sh
sudo cp /etc/postgresql/15/main/pg_hba.conf /etc/postgresql/15/main/pg_hba.conf_bck
```
edit file and replace with sudo the hba_file and replace
```
local   all             all                                     peer
```
with
```
local   all             all                                     md5
```
to allow local password connections

# Restart the server
sudo service postgresql restart

# login with user

```sh
psql -U newuser -h localhost
```

# Commands

| Command | Description |
| ------- | ----------- |
| \l+     | List databases info and size |
| \c DATABASE | connect to DATABASE |
| \dt+ | list tables info |
| \d+ TABLE | show table columns details |
| \q | exit |
| \? | Show help |


## Roles and permissions

```postgresql
GRANT group_role TO role_name
```

## Create a dump

```sh
pg_dump DATABASE_NAME \
	--encoding=UTF8 \
	--file=DUMP_FILE_NAME.sql \
	--format=p \ # plain format
	--no-owner \ # avoid ownership statements, so that the user importing the dump later will own the tables
	--verbose -h localhost -U postgres; # connection parameters, use required user
```

or in cmd (windows)

```sh
pg_dump "DATABASE_NAME" \
	--encoding "UTF8" \
	--file "DUMP_FILE_NAME.sql" \
	--format "p" \ # plain format
	--no-owner \ # avoid ownership statements, so that the user importing the dump later will own the tables
	--verbose -h "localhost" -U "postgres"; # connection parameters, use required user
```

## Import a dump

Create a target database:

```sh
psql -U ${db_user} -h localhost -c "DROP DATABASE IF EXISTS ${import_db_name}";
psql -U ${db_user} -h localhost -c "CREATE DATABASE ${import_db_name} OWNER=${db_user}";
``` 

Import using pg_dump
```sh
pg_restore ${dump_file} \
        -U ${db_user}\
        -h localhost \
        --dbname=${import_db_name}  \
	    --no-owner \
	    --role=${db_user} \
        --no-acl \
        --verbose 2>&1| tee -a $logfile
```

Import using psql if format is plain (sql script)
```sh
psql -f ${dump_file} \
        -U ${db_user}\
        -h localhost \
        -d ${import_db_name} 2>&1| tee -a $logfile
```
