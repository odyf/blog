---
title: wallabag-docker-compose
tags: 
- wallabag
- docker
abbrlink: 30550
date: 2020-11-18 10:23:46
---

# 序言

在这个互联网信息爆炸的时代，跟上时代的步伐，保存信息，随时随地的阅读成了需求，虽然很小众，但经验一定是一点一点积累的，而```wallabag```就是这样的一个工具，当你看到一篇好的文章，无论是移动客户端还是电脑PC端，虽然现在有全平台一个账号走天下，但有时意外随时可能发生，万一这个网站不运营了，收藏的书签地址就毫无意义，而```wallabag```就是这样一个脱机工具，它会以富文本的方式，很好的保存网页到本地，如果网站允许还可以把图片自动转换为本地的图片，浏览器插件，安卓客户端基本上覆盖了全终端，加上是开源，本地可以搭建，可以说一次搭建终身受益。



## 参考

[个人信息管理系统](http://zh.wikipedia.org/wiki/个人信息管理系统)（*Personal Information Manager*，简称PIM）是一种提供个人信息组织管理功能的一系列流程。其目的是为了便于保存、记录、回溯和管理各种个人信息。而稍后读这种服务就是 PIM 流程中的一部分，属于信息的收集部分，也是整个 PIM 流程中的「输入」部分。我们每天都会从各种渠道获取到各种信息，普通的新闻属于不需要进行再处理的信息内容，也可称作**「阅后即焚」**。而需要进行二次处理的内容即是有价值的内容，稍后读在信息的收集中占据一定分量，如日常的 Safari 浏览，可以通过 Reading Lists（阅读列表）进行后续处理，并且它支持离线阅读。第三方的稍后读服务有常见的 Pocket、Instapaper 等，它们都有着共同的特点：优化显示内容、离线浏览、只能分类以及多重显示方式。



## 搭建



nano docker-compose.yml

```
version: '3'
services:
  wallabag:
    image: wallabag/wallabag
    environment:
      - MYSQL_ROOT_PASSWORD=wallaroot
      - SYMFONY__ENV__DATABASE_DRIVER=pdo_mysql
      - SYMFONY__ENV__DATABASE_HOST=db
      - SYMFONY__ENV__DATABASE_PORT=3306
      - SYMFONY__ENV__DATABASE_NAME=wallabag
      - SYMFONY__ENV__DATABASE_USER=wallabag
      - SYMFONY__ENV__DATABASE_PASSWORD=wallapass
      - SYMFONY__ENV__DATABASE_CHARSET=utf8mb4
      - SYMFONY__ENV__MAILER_HOST=127.0.0.1
      - SYMFONY__ENV__MAILER_USER=~
      - SYMFONY__ENV__MAILER_PASSWORD=~
      - SYMFONY__ENV__FROM_EMAIL=wallabag@example.com
      - SYMFONY__ENV__DOMAIN_NAME=http://f.gao4.top:86
    ports:
      - 86:80
    volumes:
      - ./images:/var/www/wallabag/web/assets/images
  db:
    image: mariadb
    environment:
      - MYSQL_ROOT_PASSWORD=wallaroot
    volumes:
      - ./data:/var/lib/mysql
  redis:
    image: redis:alpine
```

## 需要注意参数

```
- SYMFONY__ENV__DOMAIN_NAME=http://f.gao4.top:86
```

```
- 86:80
```

PS: 第一个是需要修改的域名加端口第二个是把外网86端口映射到容器80端口

### 转换本地图片下载

默认不开 开启抓取的网页图片本地转换需要用默认的wallabag登录开启把```0```改为```1``` 其他账号没有这个选项

![设置路径](/img/Snipaste_2020-11-18_11-07-10.png)

## 启动

docker-compose up -d

## 优点

docker-compose推荐使用这个方式进行启动容器，优点是全平台迁移，无论是群晖，威联通，unraid 还是Linux一些发行版，只需要把wallabag目录迁移到新机器上```docker-compose up -d``` 启动就行，以防止突然迁移出现```忘记当初怎么启动这个``` 容器的情况。所以说推荐以这个方式启动容器而且挂载卷在``` - ./data:/var/lib/mysql```我的写法不是绝对路径，是当前的目录创建一个目录的写法。可以说几乎不修改什么。

![预览](/img/Snipaste_2020-11-18_10-51-49.png)