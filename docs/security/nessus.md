## A quick guide to carrying out some Sysadmin tasks on Nessus

Version 7.x which is still supported.

Version 8.x is the latest but due to the issues we had with this version, we should stay on version 7 for the foreseeable future or until we need to upgrade.

The Nessus modules are automatically upgraded but the Nessus engine(components) won't be due to comments mentioned above.

#### Where are the logs?

```
/opt/nessus/var/nessus/logs/nessusd.messages
```

#### Stop rebuild plugins and start
```
service nessusd stop
/opt/nessus/sbin/nessusd -R
service nessusd start
```

#### Generate a bug report
The only time you'll need to do this is when you have to engage with the Tenable support.
```
/opt/nessus/sbin/nessuscli bug-report-generator
```

#### Re-registering Nessus from CLI
```
service nessusd stop
/opt/nessus/sbin/nessuscli fix --reset # /opt/nessus/sbin/nessuscli fetch --register <activation code> 
/opt/nessus/sbin/nessusd -R # service nessusd start
```

#### Get the Scanner Metrics from CLI
```
/opt/nessus/sbin/nessuscli fix --list | grep metrics
```

#### Folders for Backup/Revcovery
```
/opt/nessus/var/nessus/global.db
/opt/nessus/var/nessus/master.key
/opt/nessus/var/nessus/policies.db
/opt/nessus/var/nessus/users
/opt/nessus/etc/nessus 
```

#### Nessus Tuning
Disable "thorough test" in scan policy. This can consume more resource
Reduce "max host per scan" in scanner settings but this is a try it and see approach.

#### Nessus Scan Schedules
|Description|Frequency|Day of Week|Time|
|:---------:|:-------:|:---------:|:--:|
|External - Site 2 Full Scan|Weekly|Saturday|02:30|
|External - PCI Scan, Site 2 networks|Monthly|Saturday|12:00|

