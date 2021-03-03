
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
Now, lets discuss the four types of `JOIN` commands:
* `INNER JOIN`: prints out only rows which have matching **ID** values.
* `LEFT JOIN`: prints out every row of table A filling missing values of table B with nulls (rows of A with ID_A values missing in ID_B will have nulls in B columns)
* `RIGHT JOIN`: prints out every row of table B filling missing values of table A with nulls (rows of B with ID_B values missing in ID_A will have nulls in A columns)
* `FULL JOIN`: also called `FULL OUTER JOIN`, prints out every row of A and B filling with nulls the columns with unmatched ID values.
