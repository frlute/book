# 服务器搭建

## Redis Server 部分
该部分内容，适应需要安装redis来报存游戏玩家数据的服务器，不需要安装Redis的服务器，可以忽略。

### 下载并安装Redis
CentOS 中 epel 库的 redis.x86_64 的版本太低（2.4），所以，这里采用源码编译的方式来安装 Redis （从官网下载最新稳定版的源代码，需要访问外网）：
```
cd /data/soft
sudo wget http://download.redis.io/releases/redis-2.8.19.tar.gz
sudo tar xzvf redis-2.8.19.tar.gz
cd redis-2.8.19
sudo make
sudo make install
```

执行完成后，会在`/usr/local/bin/`目录下，多出几个文件，包括：

    redis-benchmark
    redis-check-aof 
    redis-check-dump
    redis-cli 
    redis-server

### 建立必要的目录
为redis的运行，建立必要的目录，包括（以admin帐号执行）:
**旧目录**

    $ sudo mkdir -p /etc/redis
    $ mkdir -p /data/redis/db
    $ mkdir -p /data/redis/log

**变形金刚新目录**

    $ sudo mkdir -p /data/opt/redis
    $ mkdir -p /data/redis/db
    $ mkdir -p /data/opt/log/redis

### 修改配置及启动管理
运行 utils/install_server.sh 来初始化安装第一个 redis 运行实例。根据提示，确定:
>$   sudo /data/soft/redis-2.8.19/utils/install_server.sh

 - 端口（6305）
 - 配置文件（/data/opt/etc/redis/6305.conf)
 - 日志文件路径（/data/opt/log/redis)
 - 数据目录（/data/redis/db)
 - 执行文件，服务器（/usr/local/bin/redis-server)
 - 执行文件，客户端（/usr/local/bin/redis-cli）


