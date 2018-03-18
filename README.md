## guide to set up remote xdebug in docker step by step

Goal:

- debug PHP file inside docker
- PHP file could be using different interpreter than host machine (e.g php5 on host vs php7 in container)
- seamless IDE integration


## basic project and hello


```
alias dc=docker-compose
cd php7-project
dc build .
dc run --entrypoint /php7-project/src/later.php php7-project
```

Output of var_dump should be colorized i.e xdebug enabled

```
/php7-project/src/later.php:4:
string(2) "hh"
hello% 
```

## set up IP address to connect from container to host

```
apt-get install -y net-tools iputils-ping
ifconfig
# use ip from eth0
# verify ip can be contacted from container
```


## update php.ini file 

```
xdebug.remote_enable=1
xdebug.remote_autostart=0
xdebug.idekey=phpstorm
xdebug.default_enable=1
xdebug.remote_connect_back=0
xdebug.profiler_enabled=0
xdebug.remote_host=192.168.0.100
xdebug.remote_port=9000
```

## set up phpstorm ide


create dbg proxy

create debug configuration

create server and map paths


## set breaking point and test

dc run --entrypoint /php7-project/src/later.php php7-project
