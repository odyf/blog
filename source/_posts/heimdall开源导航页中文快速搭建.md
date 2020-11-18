---
title: heimdall开源导航页中文快速搭建
tags: heimdall 中文 快速 搭建
abbrlink: 39422
date: 2020-10-15 16:53:24
---

## [仓库地址](https://github.com/odyf/heimdall-cn)





# 序言

结合网上资料，让喜欢这个导航页的用户有一个快速搭建的环境

加上本人喜欢docker 命令行 *不太喜欢面板添加参数*

就有了这个项目

## 特点

1. 中文
2. 国内gitee仓库加速
3. 快速只需要几步复制粘贴的命令就可以搭建



## 命令

```
cd /home
git clone https://github.com/odyf/heimdall-cn.git
cd heimdall-cn
docker-compose up
git checkout app/SupportedApps.php
git checkout lan/en/app.php
docker-compose up -d
```

## 访问

在**docker-compose.yml**配置文件里有下面参数

```
    ports:
      - 33:80
      - 44:443
```

访问http://ip:33