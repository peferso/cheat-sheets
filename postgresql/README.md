# Installation instructions

https://www.postgresql.org/download/linux/ubuntu/

# Start service

Works for WSL ubuntu 20

```sh
sudo service postgresql start
```

# Create user

```postgresql
sudo -u postgres createuser newuser;
```

# Create database for user
```postgresql
create database newuser with owner = newuser;
grant all privileges on database newuser to newuser;
alter user newuser createdb;
```

# Set password

```
sudo -u postgres psql
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

See commands available

```psql
\?
```

## Roles and permissions

```psql
GRANT group_role TO role_name
```
