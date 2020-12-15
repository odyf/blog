---
title: docker-安装
tags: docker 经验
abbrlink: 54680
date: 2020-10-13 20:17:29
---

## 公网环境脚本安装docker

```
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

## 国内阿里云镜像加速

```
mkdir -p /etc/docker
tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://0z9mn9x7.mirror.aliyuncs.com"]
}
EOF

```

### 管理

```
systemctl daemon-reload
systemctl restart docker
systemctl enable docker
```

### 安装docker-compose

1.下载（官方地址：[https://docs.docker.com/compose/install/]

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

github很慢的话可以搜索一下它的镜像站。来加速\

```
sudo curl -L https://get.daocloud.io/docker/compose/releases/download/1.24.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose

```

2.安装 赋予执行权限

```
sudo chmod +x /usr/local/bin/docker-compose
```

3.查看版本

```
docker-compose version
```

4.卸载docker-compose

```
sudo rm /usr/local/bin/docker-compose
```