---
title: resilio-国内-Linux-install
date: 2020-12-15 19:01:46
tags: 
- resilio
- Linux
---

### 序言

在谷歌搜索引擎找了一下相关脚本，脚本拉取的文件是官网，而resilion已经被GG了，所以这个脚本在国外的服务器上是可行可用的，而在国内就没法了，刚好coding上线了新功能，一个小型的网盘可直链，就替换相关文件，使脚本国内服务器上可用，通过服务器centos7 其发行版本理论上也可以用的。

### 使用方法

```
#安装必要的软件包
yum -y install wget unzip
#下载脚本
wget https://github.com/odyf/Resilio-Sync/archive/master.zip
#解压并安装
unzip master.zip && cd Resilio-Sync-master && chmod +x *sync.sh && ./sync.sh
```

## 脚本地址

[我是链接](https://github.com/odyf/Resilio-Sync)