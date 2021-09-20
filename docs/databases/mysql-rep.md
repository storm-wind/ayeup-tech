# MySQL Replication and associated Commands

### How to setup MySQL/MariaDB replication between two hosts


---

### Working with Binary Log files

Binary log files keep track of any changes to the database, no changes then no logs.
Binary log files are setup as part of replication - see above.

#### How to Expire Binary logs after a period of time
When I setup replication the binary log files quickly filled up the filesystem and with me not being a DBA didn't realise.
So I had to setup log files to expire. The default is ***0*** whcih means they never expire.

Heres how to do it
```
mysql -u <username> -p

SHOW VARIABLES LIKE 'expire_logs_days';
    -> ;
+------------------+-------+
| Variable_name    | Value |
+------------------+-------+
| expire_logs_days | 0     |
+------------------+-------+
1 row in set (0.00 sec)


SET GLOBAL expire_logs_days=<num of days>;

```

#### Show Replication Status

```
SHOW SLAVE STATUS\G
```

And you'll get something like this.

```
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: source1
                  Master_User: root
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.000004
          Read_Master_Log_Pos: 931
               Relay_Log_File: slave1-relay-bin.000056
                Relay_Log_Pos: 950
        Relay_Master_Log_File: mysql-bin.000004
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
          Exec_Master_Log_Pos: 931
              Relay_Log_Space: 1365
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
  Replicate_Ignore_Server_Ids: 0

```
---

#### 
