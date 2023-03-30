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
