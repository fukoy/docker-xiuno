## 说明
本仓库是使用docker运行xiuno论坛

默认数据库地址：`mysql`  帐号： `xiuno`  密码： `password`

访问 `ip:8080`  进行安装

可以使用  `宝塔面板`  或者  `nginx proxy manger`  进行反代

## 使用方法

拉取仓库

```
git clone https://github.com/fukoy/docker-xiuno.git
cd docker-xiuno
```

docker-compose命令
```
#docker-compose运行
sudo docker-compose up -d

#docker-compose停止
sudo docker-compose down

#docker-compose重启
sudo docker-compose restart
```

## 其他说明

docker-compose文件

``` 
version: '3'

services:
  web:
    image: fukoy/nginx-php-fpm:php7.3
    ports:
      - '8080:80' #开放端口8080
    restart: always
    volumes:
      - ./www:/usr/share/nginx/ #xiuno网站文件
      - ./nginx/conf.d:/etc/nginx/conf.d #nginx配置文件夹
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf #nginx配置文件
    depends_on:
      - mysql
    networks:
      - web

  mysql:
    image: mariadb
    restart: always
    expose:
      - '3306' #不开放端口
    volumes:
      - ./mysql/data:/var/lib/mysql
      - ./mysql/logs:/var/log/mysql
      - ./mysql/conf:/etc/mysql/conf.d
    env_file:
      - mysql.env
    networks:
      - web

networks:
  web:
```

数据库配置

```
# MySQL的root用户默认密码，这里自行更改
MYSQL_ROOT_PASSWORD=password

# MySQL镜像创建时自动创建的数据库名称
MYSQL_DATABASE=xiuno

# MySQL镜像创建时自动创建的用户名
MYSQL_USER=xiuno

# MySQL镜像创建时自动创建的用户密码
MYSQL_PASSWORD=password

# 时区
TZ=Asia/Shanghai
```

