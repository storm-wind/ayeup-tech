# Typical Operations of MySQL/MariaDB

---
#### How to purge binary logs

```
mysql -u root -p
```

```
SHOW BINARY LOGS;
```

and then purge from selectd binary files. Command below will purge logs from last week.

```
PURGE BINARY LOGS BEFORE DATE(NOW() - INTERVAL 7 DAY) + INTERVAL 0 SECOND;
```

You can set binary logs to expire as well. In the `/etc/my.cnf`.
```
expire_logs_days = 7;
```

To set it without restart MYSQL you can run this,

```
SET GLOBAL expire_logs_days = 7;
```

Also setting max binlog file size as above.
```
max_binlog_size = 100M
```

```
SET GLOBAL max_binlog_size = 100M;
```

---





