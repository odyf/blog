---
title: arch-lightdm-xfce登录管理器美化
tags:
  - arch
  - xfce
  - lightdm
  - 美化
abbrlink: 54556
date: 2020-11-21 12:25:41
---

不知道是主题问题还是登录的一瞬间有一个画面撕裂的问题

文章[参考链接](https://www.v2ex.com/t/630589)

### 安装lightdm-webkit2-greeter

```
sudo pacman -S lightdm-webkit2-greeter
```

打开文件 

```
nano /etc/lightdm/lightdm.conf
```

找到 第102行参数greeter-session，修改为下面的内容

```
greeter-session=lightdm-webkit2-greeter
```

注意上面参数修改配置行102行

![ greeter-session](/img/Snipaste_2020-11-21_12-34-16.png)

#### 主题存放路径

```
#下载的主题保存在 /usr/share/lightdm-webkit/themes/
#可以直接用 git clone 到这个路径下
```

移动到主题目录

```
cd /usr/share/lightdm-webkit/themes/
```

GIt clone 一个Github上星星最多的主题

```
git clone https://github.com/NoiSek/Aether.git
```

#### 设置登录主题

编辑配置文件

```
sudo nano /etc/lightdm/lightdm-webkit2-greeter.conf
```

找到 ```webkit_theme```，修改

```
webkit_theme = Aether
```

## 主题欣赏

#### [lightdm-webkit-theme-aether](https://github.com/NoiSek/Aether)

![Aether](/img/theme-showcase.gif)

#### [lightdm-webkit-theme-litarvan](https://github.com/Litarvan/lightdm-webkit-theme-litarvan)

![litarvan](/img/68747470733a2f2f6c6974617276616e2e6769746875622e6.png)

![litarvan1](/img/68747470733a2f2f6c6974617276616e2e6769746875622e7.png)

#### [lightdm-webkit-theme-tendou](https://github.com/codehearts/lightdm-webkit-theme-tendou/)

![lightdm-webkit-theme-tendou](/img/screenshot.png)

