
Show users:
```sh
select * from mysql.user;
select Host, User, password_last_changed from mysql.user;
```

Show columns info
```sh
desc mysql.user;
```

Create a new user and grant all privileges:
```sh
CREATE USER '{{ db_user }}'@'{{ db_uhst }}' IDENTIFIED BY '{{ db_pass }}';
GRANT ALL PRIVILEGES ON * . * TO '{{ db_user }}'@'{{ db_uhst }}';
FLUSH PRIVILEGES;
```

Execute queries from the command line, example:
```sh
mysql -e "CREATE USER '{{ db_user }}'@'{{ db_uhst }}' IDENTIFIED BY '{{ db_pass }}';" -uroot -p{{ mysqlrootpasswd }}
```

Connect to a remote mysql server
```sh
mysql -uUSER -hHOSTIP -pPASSWORD
```

Change authentication method for users:
```sh
ALTER USER 'yourusername'@'localhost' IDENTIFIED WITH mysql_native_password BY 'youpassword';
```
