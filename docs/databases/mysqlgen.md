# General MYSQL/MARIADB Commands

---
### Quickly check integrity of MySQL DB
```
mysqlcheck --all-databases --quick --fast
```

### Override MySql file limits
```
systemctl edit mysqld.service
```

then add this

```
[service]
LimitNOFILE=10000
```

### Change parameters in MYSQL
Navigate to **/etc/my.cnf **

### Increase MySQL/MariaDB max_connections online (without a restart)
You can use `SHOW VARIABLE LIKE 'max_connections';` to check the number of max connections allowed.

```
mysql> SHOW VARIABLE LIKE 'max_connections';
+-----------------+-------+
| Variable_name   | Value |
+-----------------+-------+
| max_connections | 151   |
+-----------------+-------+
1 row in set (0.01 sec)

```

You might want to see how many connections you have in your database. You can use the below query to check that.

```
mysql> SELECT COUNT(1) FROM information_schema.processlist;
+----------+
| COUNT(1) |
+----------+
|      100 |
+----------+
1 row in set (0.00 sec)

```

Let’s assume, we need to increase maximum connections allowed to 250. Please run below query.

```
mysql> SET GLOBAL max_connections = 250;
Query OK, 0 rows affected (0.00 sec)

```

You can verify the change by running `SHOW VARIABLE LIKE 'max_connections';`.

```
mysql> SHOW VARIABLE LIKE 'max_connections';
+-----------------+-------+
| Variable_name   | Value |
+-----------------+-------+
| max_connections | 250   |
+-----------------+-------+
1 row in set (0.01 sec)

```

But our work is not done yet. If service restarts max_connection will reset to the old value. To make the change permanent, we need to change the configuration file. Find the relevant configuration file. It can be one of below files. (Depends on your installation.)

```
/etc/mysql/mysql.conf.d/mysqld.cnf
/etc/mysql/conf.d/mysql.cnf
/etc/mysql/mysql.cnf
/etc/mysql/my.cnf
/etc/my.cnf

```

Once you manage to locate the file, look for the line “`max_connections = 151`” and change it to “`max_connections = 250`”. Make sure it is not commented. (If there is a # in the begin of that line please remove that #. )

---

