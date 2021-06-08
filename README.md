## 项目说明

> 项目部署规范
  - CentOS Linux release 7.8.2003 (Core)
  - Docker version >= 19.03.12

> 初始化命令
- 添加一个www账号，用于PHP容器内的代码管理
```
useradd -u 1001 www
```
## 项目目录结构
> 初始的目录结构如下：
~~~
lnmp  部署目录（或者子目录）
├─mysql           数据库目录
├─nginx           nginx应用目录
│  ├─cert         证书目录
│  ├─log          日志目录(修改到/data/logs/nginx)
│  ├─nginx                  配置目录
│  │  ├─conf.d              站点配置文件目录
│  │  ├─nginx.conf          主要配置文件
│  │  ├─enable-php-tp5.conf TP-url重写规则
│  │  └─ ...
├─php             php应用目录
│  ├─etc          etc配置
│  │  ├─php       php配置文件目录
│  │  │  ├─conf.d
│  │  │  │  └─ ...
│  │  │  ├─php.ini  主配置文件(★)
│  │  │  └─ ...
│  │  ├─php-fpm.d   fpm配置文件目录
│  │  │  ├─www.conf 服务配置文件(★)
│  │  │  └─ ...
│  │  ├─php-fpm.conf    主配置文件
│  │  └─ ...
├─redis           redis应用目录
│  ├─data         数据目录
│  ├─redis.conf   主配置文件
│  └─ ...
├─project         工程目录
│  ├─php          PHP工程
│  └─ ...
└─ ...
~~~
## 工程连接容器服务配置
> mysql
```
内部连接
host => mysql_dk

外部连接
host => 服务器IP
```
> redis
```
[
    // redis主机
    'host'           => '127.0.0.1',
    // redis端口
    'port'           => 6379,
    // 密码
    'password'       => '',
    // 默认库
    'select'         => 0,
]
```
## 容器命令
> docker-compose
```
docker-compose up       // 直接启动 可以看到日志输出
docker-compose up -d    // 后台启动
docker-compose stop     // 停止项目
docker-compose restart  // 重启项目
docker-compose down     // 会停掉容器,并删除掉容器
```
> php
```
php目录composer update
docker exec -it php_dk su www -c "cd /data/project/php/yxqz.manage01.jingh5.com && composer update"

不同工程自己修改路径
```

> nginx
```
测试
docker exec -it nginx_dk nginx -t

重载
docker exec -it nginx_dk nginx -s reload
```

> redis
```
连接
docker exec -it redis_dk redis-cli -h redis_dk
```
