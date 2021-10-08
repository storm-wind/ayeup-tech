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

# MySQL Replication

#### Show status of secondary replication server

Log into MySQL shell and run this,

```
SHOW SLAVE STATUS \G
``` 

Then you'll see something like this,

```
MariaDB [(none)]> SHOW SLAVE STATUS \G
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: hostname.example.com
                  Master_User: replication_user
                  Master_Port: 3306
                Connect_Retry: 10
              Master_Log_File: primary1-bin.000198
          Read_Master_Log_Pos: 204348276
               Relay_Log_File: hostname-relay-bin.000043
                Relay_Log_Pos: 204348574
        Relay_Master_Log_File: primary1-bin.000198
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
              Replicate_Do_DB:
          Replicate_Ignore_DB:
           Replicate_Do_Table:
       Replicate_Ignore_Table:
      Replicate_Wild_Do_Table:
  Replicate_Wild_Ignore_Table:
                   Last_Errno: 0
                   Last_Error:
                 Skip_Counter: 0
          Exec_Master_Log_Pos: 204348276
              Relay_Log_Space: 204348929
              Until_Condition: None
               Until_Log_File:
                Until_Log_Pos: 0
           Master_SSL_Allowed: No
           Master_SSL_CA_File:
           Master_SSL_CA_Path:
              Master_SSL_Cert:
            Master_SSL_Cipher:
               Master_SSL_Key:
        Seconds_Behind_Master: 0
Master_SSL_Verify_Server_Cert: No
                Last_IO_Errno: 0
                Last_IO_Error:
               Last_SQL_Errno: 0
               Last_SQL_Error:
  Replicate_Ignore_Server_Ids:
             Master_Server_Id: 1
               Master_SSL_Crl:
           Master_SSL_Crlpath:
                   Using_Gtid: No
                  Gtid_IO_Pos:
      Replicate_Do_Domain_Ids:
  Replicate_Ignore_Domain_Ids:
                Parallel_Mode: conservative
                    SQL_Delay: 0
          SQL_Remaining_Delay: NULL
      Slave_SQL_Running_State: Slave has read all relay log; waiting for the slave I/O thread to update it
1 row in set (0.00 sec)

```
---

####  Turn off GTID

I had to do this as the version I was replicating from was 5.5 to 10.2. 5.5 doesn;t support GTID judging by the errors so I tunred it off.

```
mysql> stop slave;
Query OK, 0 rows affected (0.00 sec)
 
mysql> change master to master_auto_position=0;
Query OK, 0 rows affected (0.00 sec)

 


mysql> change master to master_host='master-1.db.example.com' , master_port = 3306 , master_user = 'replicate' , master_password='welcome' , master_log_file = 'bin_log.000003' , master_log_pos = 191, master_connect_retry = 5;
Query OK, 0 rows affected, 2 warnings (0.00 sec)
 
mysql> show warnings;
+-------+------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Level | Code | Message                                                                                                                                                                                                                                                                              |
+-------+------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Note  | 1759 | Sending passwords in plain text without SSL/TLS is extremely insecure.                                                                                                                                                                                                               |
| Note  | 1760 | Storing MySQL user name or password information in the master info repository is not secure and is therefore not recommended. Please consider using the USER and PASSWORD connection options for START SLAVE; see the 'START SLAVE Syntax' in the MySQL Manual for more information. |
+-------+------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
2 rows in set (0.00 sec)


mysql> start slave;
Query OK, 0 rows affected (0.00 sec)


mysql> SHOW SLAVE STATUS \G

```

This will show GTID status still but we can make sure its not using it by running this,

```
SHOW PROCESSLIST \G
```

Look for ***Command: Binlog Dump*** which was ***Command: Binlog Dump GTID***


