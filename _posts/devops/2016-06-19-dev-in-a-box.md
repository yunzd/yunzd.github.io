---
layout: post
title: 程序猿的容器微数据中心
modified:
categories: devops
description:
tags: [rancher, docker, virutalbox, mac, git]
image:
  feature:
  credit:
  creditlink:
comments: true
share: true
date: 2016-06-19T07:06:11+00:00
---
简介是这样的。

[TOC]

## 说明

## 创建 Rancher 服务器

### 生成虚拟机
用 docker-machine 创建 rancher 服务器。


`docker-machine create rancher --driver virtualbox --virtualbox-cpu-count "1" --virtualbox-disk-size "8000" --virtualbox-memory "1024" --virtualbox-boot2docker-url=/Users/martin/Downloads/boot2docker.iso && eval $(docker-machine env rancher)`


## 导入 Rancher 服务器镜像

`docker images
docker load < ~/Downloads/rancher-all/rancher-server-stable.tar
docker images
docker run -d --restart=always --name rancher-srv -p 8080:8080 rancher/server:stable 
docker logs -f rancher-srv`

docker-machine ip rancher

open http://Rancher_Server_IP:8080 


echo "ifconfig eth1 192.168.99.50 netmask 255.255.255.0 broadcast 192.168.99.255 up" | docker-machine ssh rancher sudo tee /var/lib/boot2docker/bootsync.sh > /dev/null

docker-machine restart 
docker-machine ls
docker-machine regenerate-certs rancher -f
docker-machine ls
eval $(docker-machine env rancher)

docker ps
docker load < ~/Downloads/rancher-all/rancher-server-stable.tar
docker images
docker run -d --restart=always --name rancher-srv -p 8080:8080 rancher/server:stable 
docker logs -f rancher-srv
open http://192.168.99.50:8080 

docker-machine create node1 --driver virtualbox --virtualbox-cpu-count "1" --virtualbox-disk-size "80000" --virtualbox-memory "2048" --virtualbox-boot2docker-url=/Users/martin/Downloads/boot2docker.iso


echo "ifconfig eth1 192.168.99.60 netmask 255.255.255.0 broadcast 192.168.99.255 up" | docker-machine ssh node1 sudo tee /var/lib/boot2docker/bootsync.sh > /dev/null
docker-machine regenerate-certs node1 -f
docker-machine ssh ndoe1
sudo mkdir /mnt/sda1/var/lib/rancher 


docker@node1:/mnt/sda1/var/lib/boot2docker$ cat bootsync.sh
ifconfig eth1 192.168.99.60 netmask 255.255.255.0 broadcast 192.168.99.255 up

sudo mkdir /mnt/sda1/var/lib/rancher/
sudo ln -s /mnt/sda1/var/lib/rancher/ /var/lib/


docker load < ~/Downloads/rancher-all/rancher-agent-v1.0.1.tar
docker load < ~/Downloads/rancher-all/rancher-agent-instance-v0.8.1.tar
docker load < ~/Downloads/rancher-all/jenkins.tar

docker run -d --privileged -v /var/run/docker.sock:/var/run/docker.sock -v /var/lib/rancher:/var/lib/rancher rancher/agent:v1.0.1 http://192.168.99.50:8080/v1/scripts/B589B8DC7898DD0378C9:1466244000000:WBES1W1ZZUwhj2xHb9GKb8ra0dQ

docker load < ~/Downloads/rancher-all/jenkins.tar

docker run -d --name jekins --privileged -p 9001:8080 -v /var/lib/docker/:/var/lib/docker/ -v /var/run/docker.sock:/var/run/docker.sock -v /usr/bin/docker:/usr/bin/docker  -v /lib64/libdevmapper.so.1.02:/usr/lib/libdevmapper.so.1.02  --label io.rancher.container.network=true jenkins


docker-machine create mirror --driver virtualbox --virtualbox-cpu-count "1" --virtualbox-disk-size "8000" --virtualbox-memory "512" --virtualbox-boot2docker-url=/Users/martin/Downloads/boot2docker.iso 

docker load < ~/Downloads/rancher-all/registry.tar
docker run -d -p 80:5000 --restart=always --name registry registry:2



## 容器运行节点 Rancher Agent 节点
使用 `docker-machine` 命令创建容器运行节点。

```
docker-machine create node1 --driver virtualbox --engine-insecure-registry 192.168.99.20:5000 --virtualbox-cpu-count "1" --virtualbox-disk-size "80000" --virtualbox-memory "1024" --virtualbox-boot2docker-url=/Users/martin/Downloads/boot2docker.iso && eval $(docker-machine env node1)
```

测试运行一个容器，使用 mirror 中的 busybox 镜像。

```
docker pull 192.168.99.20:5000/busybox:latest 
```

在界面上运行一个 busybox 容器。

加载 Rancher Agent 的镜像

```
docker load < ~/Downloads/rancher-all/rancher-agent-v1.0.1.tar
docker load < ~/Downloads/rancher-all/rancher-agent-instance-v0.8.1.tar
```

注册容器节点到 Rancher Server。

```
docker run -d --privileged -v /var/run/docker.sock:/var/run/docker.sock -v /var/lib/rancher:/var/lib/rancher rancher/agent:v1.0.1 http://192.168.99.102:8080/v1/scripts/CC8E5E4C452801B868CE:1466431200000:U4dxw4pmwyMmEJ2M9ib8lgxwnBQ
```

> 注意：以上命令需要到您的 Rancher Server 的页面上获取，否则参数都是不对的。

现在在 Hosts 页面上应该能看到该刚创建的节点。在页面上创建一个最小化的容器（如 busybox），来拉起 Network Agent 容器。

## 构建和测试代码
下载投票实例程序。

```
git clone https://github.com/martinliu/example-voting-app.git
```

进入该程序的目录，修改所有 image 的来源镜像库，修改为指向本地的 mirror 服务器。
1. result/tests/Dcokerfile -> FROM 192.168.99.20:5000/node
2. result/Dockerfile -> FROM 192.168.99.20:5000/node:5.11.0-slim
3. vote/Dockerfile -> FROM 192.168.99.20:5000/python:2.7-alpine
4. worker/Dockerfile -> FROM 192.168.99.20:5000/microsoft/dotnet:1.0.0-preview1
5. docker-compose.yml -> image: 192.168.99.20:5000/redis:alpine
6. docker-compose.yml -> image: 192.168.99.20:5000/postgres:9.4

由于以上应用在构建的过程中需要在线安装各种软件包，最好先翻墙，确认你有足够稳定的国外的互联网访问，建议翻墙到美国，然后在执行项目的构建命令。

```
ping facebook.com
64 bytes from 173.252.90.132: icmp_seq=0 ttl=79 time=4187.066 ms
64 bytes from 173.252.90.132: icmp_seq=1 ttl=79 time=3186.904 ms
64 bytes from 173.252.90.132: icmp_seq=2 ttl=79 time=2515.415 ms
64 bytes from 173.252.90.132: icmp_seq=6 ttl=79 time=296.457 ms
64 bytes from 173.252.90.132: icmp_seq=7 ttl=79 time=410.215 ms
^C
--- facebook.com ping statistics ---
8 packets transmitted, 5 packets received, 37.5% packet loss
round-trip min/avg/max/stddev = 296.457/2119.211/4187.066/1537.275 ms

```

以上结果表明，翻墙成功，以上结果显示翻墙的效果比较差，延迟和丢包都比较严重，可能到只构建的时候下载软件包失败。

构建投票应用成功的完整过程示例文件 [build.txt](build.txt) 。

构建完毕之后，可以检查一下是否生产了目标镜像文件，如果输出如下所示，则表明本次本地的项目集成构建成功。


```
$ docker images                                                                                                                            
REPOSITORY                            TAG                 IMAGE ID            CREATED             SIZE
examplevotingapp_result               latest              9bb4126b0905        5 minutes ago       225.8 MB
examplevotingapp_worker               latest              292396a5aba4        6 minutes ago       644.1 MB
examplevotingapp_vote                 latest              28052191beea        10 minutes ago      68.31 MB

```

在当前 node1 节点上做本地的集成结果的功能测试，用 docker-compose 启动这个项目。先检查 compose 文件，然后运行 up。



```$ docker-compose config                                                                                                                      
networks: {}
services:
  db:
    image: 192.168.99.20:5000/postgres:9.4
  redis:
    image: 192.168.99.20:5000/redis:alpine
    ports:
    - '6379'
  result:
    build:
      context: /Users/martin/Documents/GitHub/example-voting-app/result
    command: nodemon --debug server.js
    ports:
    - 5001:80
    - 5858:5858
    volumes:
    - /Users/martin/Documents/GitHub/example-voting-app/result:/app:rw
  vote:
    build:
      context: /Users/martin/Documents/GitHub/example-voting-app/vote
    command: python app.py
    ports:
    - 5000:80
    volumes:
    - /Users/martin/Documents/GitHub/example-voting-app/vote:/app:rw
  worker:
    build:
      context: /Users/martin/Documents/GitHub/example-voting-app/worker
version: '2.0'
volumes: {}

$ docker-compose up                                                                                                                            
Recreating examplevotingapp_vote_1
Recreating examplevotingapp_worker_1
Starting examplevotingapp_db_1
Starting examplevotingapp_redis_1
Recreating examplevotingapp_result_1
Attaching to examplevotingapp_db_1, examplevotingapp_redis_1, examplevotingapp_worker_1, examplevotingapp_result_1, examplevotingapp_vote_1
redis_1   |                 _._
redis_1   |            _.-``__ ''-._
redis_1   |       _.-``    `.  `_.  ''-._           Redis 3.2.1 (00000000/0) 64 bit
redis_1   |   .-`` .-```.  ```\/    _.,_ ''-._
db_1      | LOG:  database system was shut down at 2016-06-20 09:58:19 UTC
redis_1   |  (    '      ,       .-`  | `,    )     Running in standalone mode
redis_1   |  |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6379
redis_1   |  |    `-._   `._    /     _.-'    |     PID: 1
redis_1   |   `-._    `-._  `-./  _.-'    _.-'
redis_1   |  |`-._`-._    `-.__.-'    _.-'_.-'|
redis_1   |  |    `-._`-._        _.-'_.-'    |           http://redis.io
redis_1   |   `-._    `-._`-.__.-'_.-'    _.-'
redis_1   |  |`-._`-._    `-.__.-'    _.-'_.-'|
db_1      | LOG:  MultiXact member wraparound protections are now enabled
redis_1   |  |    `-._`-._        _.-'_.-'    |
redis_1   |   `-._    `-._`-.__.-'_.-'    _.-'
db_1      | LOG:  database system is ready to accept connections
redis_1   |       `-._    `-.__.-'    _.-'
redis_1   |           `-._        _.-'
redis_1   |               `-.__.-'
redis_1   |
redis_1   | 1:M 20 Jun 10:13:36.216 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
redis_1   | 1:M 20 Jun 10:13:36.216 # Server started, Redis version 3.2.1
redis_1   | 1:M 20 Jun 10:13:36.216 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
db_1      | LOG:  autovacuum launcher started
redis_1   | 1:M 20 Jun 10:13:36.216 # WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
redis_1   | 1:M 20 Jun 10:13:36.216 * The server is now ready to accept connections on port 6379
vote_1    |  * Running on http://0.0.0.0:80/ (Press CTRL+C to quit)
vote_1    |  * Restarting with stat
result_1  | [nodemon] 1.9.2
result_1  | [nodemon] to restart at any time, enter `rs`
result_1  | [nodemon] watching: *.*
result_1  | [nodemon] starting `node --debug server.js`
result_1  | Debugger listening on port 5858
vote_1    |  * Debugger is active!
vote_1    |  * Debugger pin code: 139-254-286
worker_1  | Found redis at 172.19.0.2
result_1  | Mon, 20 Jun 2016 10:13:40 GMT body-parser deprecated bodyParser: use individual json/urlencoded middlewares at server.js:67:9
result_1  | Mon, 20 Jun 2016 10:13:40 GMT body-parser deprecated undefined extended: provide extended option at ../node_modules/body-parser/index.js:105:29
result_1  | App running on port 80
result_1  | Connected to db

```

打开浏览器测试 vote 应用。

```
open http://192.168.99.114:5000

```

正常显示结果如下图所示：
![voting](/images/voting-1.jpg)


打开浏览器测试 result 应用。

```
open http://192.168.99.114:5001

```

正常显示结果如下图所示：
![result](/images/result-1.jpg)


在 Rancher 的 hosts 界面中应该看到这些运行的容器。
![voting-in-ranche](/images/voting-in-rancher-1.jpg)


至此所有关于应用构建和功能测试的过程完成，按 ctl + c 结束 docker-compose up 的运行。

```
^CGracefully stopping... (press Ctrl+C again to force)
Stopping examplevotingapp_worker_1 ... done
Stopping examplevotingapp_result_1 ... done
Stopping examplevotingapp_vote_1 ... done
Stopping examplevotingapp_db_1 ... done
Stopping examplevotingapp_redis_1 ... done

```


## 安装 rancher-compose 命令
在Rancher 的界面右下角，点击 Download CLI， 把压缩包下载到程序代码目录，解压缩后得到 rancher-compose 可执行文件。

## 构建 rancher-compose.yml 文件
参考 docker-compose.yml 文件构建 rancher-compose.yml 文件如下。

## 创建 API Key


Access key  9F4FCAC33A8F44856CA1
Security Key ZbERnGaNSjKqTTXhRBUJD3JBumP9eyTvC7PGmaL1

./rancher-compose --url http://192.168.99.104:8080 --access-key 9F4FCAC33A8F44856CA1 --secret-key ZbERnGaNSjKqTTXhRBUJD3JBumP9eyTvC7PGmaL1 -p voting-app up --pull -d --upgrade pyapp

./rancher-compose --url http://192.168.99.104:8080 --access-key 9F4FCAC33A8F44856CA1 --secret-key ZbERnGaNSjKqTTXhRBUJD3JBumP9eyTvC7PGmaL1 voting-app up --pull -d --upgrade votingapp


## 测试flask demo

加载相关镜像
docker load < ~/Downloads/redis.tar
docker load < ~/Downloads/python-with-flask-redis.tar

 




