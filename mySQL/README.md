# Basic introduction

## Installation of mySQL server on Linux OS

There are several necessary steps to configure a mySQL database in Linux. 
Although I have done it in an Amazon Linux EC2 instance, they should be similar in a different Linux OS.

These steps can be found in [this Ansible playbook](https://github.com/peferso/terraform-demo/blob/main/Utilities/ansible-playbooks/db-server-setup.yml) that I wrote to automatically configure a mySQL database server in an Amazon Linux EC2 instance.

The main steps are:
* Get the repository of mySQL server
* Install it
* Enable the automatic activation of the mysqld server at boot
* Manually start the mysqld service
* Retrieve initial temporary root password
* Set a new root password

## Allowing remote users to connect to the database server using mySQL client

If we want to allow remote database users to connect to our mySQL server we must perform several configurations in the server host:
* Find out the port used by the mysqld service (default is 3306)
* Enable inbound tcp traffic through mysqld service port from client servers with a security group rule
* Enable remote hosts connections to mySQL server modifying the configuration file (`/etc/my.cnf`), setting the **bind-address** parameter to:
```sh
bind-address = 0.0.0.0
```
This will allow connections from any remote ip address.
* Restart mysqld service

Once this is done, a remote user must exists in order to connect from the client host. We must create this remote user as root: 
```sql
CREATE USER 'DBUSER'@'%' IDENTIFIED BY 'PASSWORD';
GRANT ALL PRIVILEGES ON * . * TO 'DBUSER'@'%';
FLUSH PRIVILEGES;
ALTER USER 'DBUSER'@'%' IDENTIFIED WITH mysql_native_password BY 'PASSWORD';
```
Replace `DBUSER` and `PASSWORD` by a valid user name and password. 
To automate this task, this queries can be sent using the command line as follows:
```sh
mysql -e "CREATE USER 'DBUSER'@'%' IDENTIFIED BY 'PASSWORD';" -uroot -pROOTPASSWORD
...
```

After ssh to the client server, we can access the remote mySQL database as follows:
```sh
mysql -u'DBUSER' -h'IP_OF_SERVER_DATABASE' -p'PASSWORD'
```

## Users management
Show users:
```sql
select * from mysql.user;
select Host, User, password_last_changed, plugin from mysql.user;
```

Show columns info
```sql
desc mysql.user;
```

Create a new user and grant all privileges:
```sql
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
```sql
ALTER USER 'yourusername'@'localhost' IDENTIFIED WITH mysql_native_password BY 'youpassword';
```

# Joins

## Concept
A select query can be performed on two tables (A and B), so that the query outputs columns from both tables.
Rows in both tables will be joined by using a common column, present in both tables (lets say ID_A and ID_B).
```sql
SELECT column1, column2, ...
FROM A
<JOIN_TYPE> JOIN B
ON A.ID_A = B.ID_B
WHERE conditions
ORDER BY value
```

## Types of JOIN operations

Now, lets discuss the four types of `JOIN` commands:
* `INNER JOIN`: prints out only rows which have matching **ID** values.
* `LEFT JOIN`: prints out every row of table A filling missing values of table B with nulls (rows of A with ID_A values missing in ID_B will have nulls in B columns)
* `RIGHT JOIN`: prints out every row of table B filling missing values of table A with nulls (rows of B with ID_B values missing in ID_A will have nulls in A columns)
* `FULL JOIN`: also called `FULL OUTER JOIN`, prints out every row of A and B filling with nulls the columns with unmatched ID values. This is not supported by MySQL.

# Other queries

## Concatenate columns
```sql
SELECT COUNT(DISTINCT CONCAT(`id`, `file`)) FROM table;
```

## Show databases

```sql
SHOW DATABASES;
```

## Check max characters admitted in column

```sql
SELECT CHARACTER_MAXIMUM_LENGTH FROM information_schema.COLUMNS WHERE TABLE_SCHEMA = 'my_schema' AND TABLE_NAME = 'tab' AND COLUMN_NAME = 'coll';
```

## Alter table workaround for very large tables:

```sql
CREATE TABLE raw_data_new LIKE raw_data;
ALTER TABLE raw_data_new MODIFY report_id VARCHAR(256);
INSERT INTO raw_data_new SELECT * FROM raw_data;
RENAME TABLE raw_data TO raw_data_old;
RENAME TABLE raw_data_new TO raw_data;
DROP TABLE raw_data_old;
```

***

Return to **[main page](../README.md)**
