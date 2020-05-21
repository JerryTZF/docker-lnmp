# docker-lnmp
部署docker的Nginx、Mysql(5.6)、PHP(7.3.7|自定义扩展)集成镜像
## 项目结构
```
├── app
│   └── src
│       ├── empty_tp5
│       ├── nayuki_applets
│       ├── project
│       └── static_resources
├── docker-compose.yml
├── mysql
│   ├── conf
│   │   └── my.cnf
│   └── mysqldb
│       ├── auto.cnf
│       ├── demo
│       ├── ibdata1
│       ├── ib_logfile0
│       ├── ib_logfile1
│       ├── mysql
│       └── performance_schema
├── nginx
│   ├── cert
│   │   ├── 3906991_www.tzf-foryou.xyz.key
│   │   └── 3906991_www.tzf-foryou.xyz.pem
│   ├── conf.d
│   │   ├── index_test.conf
│   │   └── nayuki_applets.conf
│   ├── etc
│   │   └── letsencrypt
│   ├── log
│   │   ├── access.log
│   │   └── error.log
│   ├── nginx.conf
│   └── www
├── php-fpm
│   ├── Dockerfile
│   ├── php-fpm.conf
│   ├── php.ini
│   ├── sources.list
│   ├── var
│   └── www.conf
└── README.md
```

## 使用
- **在构建镜像前要确保对应映射的配置确定存在于宿主机对应的位置**
- 在docker-compose.yml所在目录执行
- 请务必保证国内镜像源文件和Dockerfile在同一目录
- 需要重新构建时，```docker-compose down```后删除镜像，再次构建
- 具体应用映射关系见 docker-compose.yml
```
第一次构建镜像建议不要后台运行
docker-compose up
后续启动、重启
docker-compose up -d | docker-compose restart
```
## 注意
- 自定义的PHP镜像没有安装composer，宿主机如果没有php环境，请使用容器php
- PHP镜像build完后，需要手动把对应的.so扩展写入对应的php.ini文件，然后重启容器
- nginx虚拟域名在conf.d目录，没有设置vhost
- 一定注意对应的映射关系，nginx的server的root要写容器内的路径
- 定时任务已经在php镜像中构建，第一次启动请进入容器手动启动```/etc/init.d/cron start ```，添加新任务需要进入容器添加```crontab -e ```
- nginx 、定时任务请务必注意目录映射关系
```
./app/src:/usr/share/nginx/html
```
- mysql容器启动后如果需要navicat链接
    -  需要设置mysql允许远程连接相关配置
    -  查看mysql容器ip ```docker inspect container_name | grep IPAddress```
    -  配置SSH代理为宿主机ip、端口、密码
    -  常规配置容器ip、端口、密码