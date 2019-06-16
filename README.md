## guide to set up remote xdebug in docker step by step

Goal:

- debug PHP file inside docker
- PHP file could be using different interpreter than host machine (e.g php5 on host vs php7 in container)
- seamless IDE integration


## basic project and hello in CLI


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


### create dbg proxy

![Alt text](img/proxy.png?raw=true "Title")

### create debug configuration

![Alt text](img/debug.png?raw=true "Title")

### create debug remote servers configuration and map the paths

![Alt text](img/servers.png?raw=true "Title")


### set breaking point

![Alt text](img/breakpoint.png?raw=true "Title")

### start debugger to listen for incoming connection

![Alt text](img/listen.png?raw=true "Title")

### run script in docker

```
dc run --entrypoint /php7-project/src/later.php php7-project
```

![Alt text](img/cli.png?raw=true "Title")

### code steps through debugger

![Alt text](img/code-debug.png?raw=true "Title")





## Apache2 / Web browser version

CLI uses PHP_XDEBUG_ENABLED environment variable to start the debugger

Apache: for web server, send a request to the port to start the debugger session e.g http://localhost:7074/index.php/?XDEBUG_SESSION_START=phpstorm

Apache will not care about PHP_XDEBUG_ENABLED environment variable