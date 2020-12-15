---
title: Traefik-学习记录
date: 2020-11-29 17:01:42
tags: 
- Traefik
- docker
---

在写这篇文章的时候，网上关于这个Traefik代理软件的资料还是很少，基本上B站上有一个UP主在更新这个系列的视频，和官网上的文章，零零散散的，不知道以后会火起来不。

## 创建初始化文件

```
mkdir -p data/configurations
touch docker-compose.yml
touch data/traefik.yml
touch data/acme.json
touch data/configurations/dynamic.yml
chmod 600 data/acme.json
```

#### 添加创建docker-compose内容

```
vim docker-compose.yml
```

```
version: '3.7'

services:
  traefik:
    image: traefik:latest
    container_name: traefik
    restart: always
    security_opt:
      - no-new-privileges:true
    ports:
      - 80:80
      - 443:443
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - ./data/traefik.yml:/traefik.yml:ro
      - ./data/acme.json:/acme.json
      # Add folder with dynamic configuration yml
      - ./data/configurations:/configurations
    networks:
      - proxy
    labels:
      - "traefik.enable=true"
      - "traefik.docker.network=proxy"
      - "traefik.http.routers.traefik-secure.entrypoints=websecure"
      - "traefik.http.routers.traefik-secure.rule=Host(`traefik.yourdomain`)"
      - "traefik.http.routers.traefik-secure.middlewares=user-auth@file"
      - "traefik.http.routers.traefik-secure.service=api@internal"

networks:
  proxy:
    external: true
```

#### 添加全局配置文件内容

```
vim /data/traefik.yml
```

```
api:
  dashboard: true

entryPoints:
  web:
    address: :80
    http:
      redirections:
        entryPoint:
          to: websecure

  websecure:
    address: :443
    http:
      middlewares:
        - secureHeaders@file
      tls:
        certResolver: letsencrypt
              
providers:
  docker:
    endpoint: "unix:///var/run/docker.sock"
    exposedByDefault: false
  file:
    filename: /configurations/dynamic.yml

certificatesResolvers:
  letsencrypt:
    acme:
      email: admin@yourdomain
      storage: acme.json
      keyType: EC384
      httpChallenge:
        entryPoint: web
        
  buypass:
    acme:
      email: admin@yourdomain
      storage: acme.json
      caServer: https://api.buypass.com/acme/directory 
      keyType: EC256
      httpChallenge:
        entryPoint: web
```

#### 添加动态配置文件内容

```
vim /data/configurations/dynamic.yml
```

```
# Dynamic configuration
http:
  middlewares:
    secureHeaders:
      headers:
        sslRedirect: true
        forceSTSHeader: true
        stsIncludeSubdomains: true
        stsPreload: true
        stsSeconds: 31536000
                
    # UserName : admin
    # Password : qwer1234          
    user-auth:
      basicAuth:
        users:
          - "admin:$apr1$tm53ra6x$FntXd6jcvxYM/YH0P2hcc1"
          
tls:
  options:
    default:
      cipherSuites:
        - TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
        - TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
        - TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
        - TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
        - TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305
        - TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305
      minVersion: VersionTLS12
```

默认登录用户为admin密码为qwer1234

#### 文件介绍

- docker-compose.yml（容器启动文件）
- traefik.yml(静态全局配置文件重启生效)
- dynamic.yml（动态配置文件无需重启生效）

当我们启动一个 Docker 服务后，Traefik 就能发现服务并为其创建默认路由，需要注意的一点是，如果 Traefik 要能够与新的 Docker 服务进行通信，必须将其加入到同一网络

#### 启动容器

```
docker-compose up -d
```

#### 反向代理容器端口

```
version: '3.5'
services: 
  whoami:
    image: containous/whoami
    ### 以下都是需要的选项
      labels:
      - "traefik.enable=true"
      - "traefik.docker.network=proxy"
      - "traefik.http.routers.whoami-secure.entrypoints=websecure"
      - "traefik.http.routers.whoami-secure.rule=Host(`wh.gao4.top`)"
      - "traefik.http.routers.whoami-secure.service=whoami-service"
      - "traefik.http.services.whoami-service.loadbalancer.server.port=80"
    networks:
      - proxy
networks:
  proxy:
    external: true
```

### 反向代理局域网内的服务

暂无资料