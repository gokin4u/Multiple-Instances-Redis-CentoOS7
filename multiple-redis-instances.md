# Run Multiple Instances of Redis on Centos 7

## create a new redis .conf file

```Bash
$ cp /etc/redis.conf /etc/redis-xxx.conf
$ sudo chown redis:redis /etc/redis-xxx.conf
```

## edit /etc/redis-xxx.conf, illustrated as below

```Bash
...
#modify pidfile
#pidfile /var/run/redis/redis.pid
pidfile /var/run/redis/redis-xxx.pid

...
#dir /var/lib/redis/
dir /var/lib/redis-xxx/

...
#modify port
#port 6379
port 6380

...
#modify logfile
#logfile /var/log/redis/redis.log
logfile /var/log/redis/redis-xxx.log

...
#modify vm-swap-file IF ANY
#vm-swap-file /tmp/redis.swap
vm-swap-file /tmp/redis-xxx.swap
...
```
## make dir /var/lib/redis-xxx

```Bash
$ mkdir -p /var/lib/redis-xxx
$ sudo chown redis:redis /var/lib/redis-xxx
```

## copy init script

```Bash
$ cp /usr/lib/systemd/system/redis.service /usr/lib/systemd/system/redis-xxx.service
```

## edit the new init script

```Bash
...

#ExecStart=/usr/bin/redis-server /etc/redis.conf --daemonize no
#ExecStop=/usr/libexec/redis-shutdown
ExecStart=/usr/bin/redis-server /etc/redis-xxx.conf --daemonize no
ExecStop=/usr/libexec/redis-xxx-shutdown


...

#RuntimeDirectory=redis
RuntimeDirectory=redis-xxx

...
```

## copy shutdown script

```Bash
$ cp /usr/libexec/redis-shutdown /usr/libexec/redis-xxx-shutdown
```

## edit the shutdown script

```Bash
...
#   SERVICE_NAME=redis
   SERVICE_NAME=redis-6380

...

#    PORT=${PORT:-6379}
    PORT=${PORT:-6380}

```


## query the status of this redis in

```Bash
$ sudo systemctl status redis-xxx
# server is stopped

# start service
$ sudo systemctl start redis-xxx 
```

## make redis-xxx service auto start

```Bash
$ sudo systemctl enable redis-xxx
```

## test redis instances
```Bash
redis-cli -p 6379
redis-cli -p xxx
```
