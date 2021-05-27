# Logrotate usage

A linux tool to rotate logs automatically so as not to use up all disk spac. Pretty handy.


### Setup logrotate

### Run logroate manually

For verbose add **-v** like example below. 

```
logrotate -v -f /etc/logrotate.d/httpd
```

### Example of logrotate

```
/var/log/httpd/*log {
    size 200M
    compress
    missingok
    notifempty
    sharedscripts
    delaycompress
    postrotate
        /bin/systemctl reload httpd.service > /dev/null 2>/dev/null || true
    endscript
}
```

Letsbreak it down.

So **/var/log/httpd/*log** is the location of log file/s.

**size 200M** is size of log file gets before being rotated.

**compress** is basically zipping up the file when rotated.

**missingok**

**notifempty**

**sharedscripts**

**delaycompress** means to compress file on next log rotation.

**postrotate** self explainatory.




