# HAProxy

Haproxy is a kick ass proxy which is scalable and has a multitude of uses.

### HAProxy Example

An example config of HAProxy that is a tcp pass through.

```
#---------------------------------------------------------------------
# Example configuration for a possible web application.  See the
# full configuration options online.
#
#   https://www.haproxy.org/download/1.8/doc/configuration.txt
#
#---------------------------------------------------------------------

#---------------------------------------------------------------------
# Global settings
#---------------------------------------------------------------------
global
    # to have these messages end up in /var/log/haproxy.log you will
    # need to:
    #
    # 1) configure syslog to accept network log events.  This is done
    #    by adding the '-r' option to the SYSLOGD_OPTIONS in
    #    /etc/sysconfig/syslog
    #
    # 2) configure local2 events to go to the /var/log/haproxy.log
    #   file. A line like the following can be added to
    #   /etc/sysconfig/syslog
    #
    #    local2.*                       /var/log/haproxy.log
    #
    log         127.0.0.1 local2

    chroot      /var/lib/haproxy
    pidfile     /var/run/haproxy.pid
    maxconn     4000
    user        haproxy
    group       haproxy
    daemon

    # turn on stats unix socket
    stats socket /var/lib/haproxy/stats

    # utilize system-wide crypto-policies
    ssl-default-bind-ciphers PROFILE=SYSTEM
    ssl-default-server-ciphers PROFILE=SYSTEM

#---------------------------------------------------------------------
# common defaults that all the 'listen' and 'backend' sections will
# use if not designated in their block
#---------------------------------------------------------------------
defaults
    mode                    http
    log                     global
    option                  httplog
    option                  dontlognull
    option http-server-close
    option forwardfor       except 127.0.0.0/8
    option                  redispatch
    retries                 3
    timeout http-request    10s
    timeout queue           1m
    timeout connect         10s
    timeout client          1m
    timeout server          1m
    timeout http-keep-alive 10s
    timeout check           10s
    maxconn                 3000

#---------------------------------------------------------------------
# main frontend which proxys to the backends
#---------------------------------------------------------------------
#frontend main
#    bind *:5000
#    acl url_static       path_beg       -i /static /images /javascript /stylesheets
#    acl url_static       path_end       -i .jpg .gif .png .css .js

#    use_backend static          if url_static
#    default_backend             app
frontend https_in
        mode tcp
        option tcplog
        bind *:443
        acl tls req.ssl_hello_type 1
        tcp-request inspect-delay 5s
        tcp-request content accept if tls

        acl <server1> req.ssl_sni -i <sub domain name>
        acl <server1> req.ssl_sni -i <sub domain name>
        acl <server2> req.ssl_sni -i <sub domain name>
        acl <server2> req.ssl_sni -i <sub domain name>

        use_backend <server1> if <server1>
        use_backend <server2> if <server2>

#---------------------------------------------------------------------
# static backend for serving up images, stylesheets and such
#---------------------------------------------------------------------
#backend static
#    balance     roundrobin
#    server      static 127.0.0.1:4331 check

backend <server1>
        mode tcp
        option tcplog
        option ssl-hello-chk
        server <server1> <ip address of backend server>:<port>

backend <server2>
        mode tcp
        option tcplog
        option ssl-hello-chk
        server <server2> <ip address of backend server>:<port>

#---------------------------------------------------------------------
# round robin balancing between the various backends
#---------------------------------------------------------------------
#backend app
#    balance     roundrobin
#    server  app1 127.0.0.1:5001 check
#    server  app2 127.0.0.1:5002 check
#    server  app3 127.0.0.1:5003 check
#    server  app4 127.0.0.1:5004 check
 
```
***

### The Stats Section

This is a stats page you can login to, to view the HAProxy stats.

```
mode http
        bind <ip address of host>:<port> # 
        ssl crt /etc/ssl/<cert chain - pem format>
        stats enable
        stats show-legends
        stats hide-version
        stats realm Haproxy\ Statistics
        stats uri /stats
        stats refresh 5s
        stats auth <username>:<password>

```

***

### Another example

Here is an example of frontends listening on different ports and implementing STunnel to enccrpt traffic.


