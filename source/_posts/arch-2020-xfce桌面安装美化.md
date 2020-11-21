---
title: arch-2020-xfce桌面安装美化
date: 2020-11-21 11:08:19
tags:
- arch
- 美化
- xfce
- lightdm
---

### 更换源

nano  /etc/pacman.conf末尾添加下面

```
[archlinuxcn]
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```

nano /etc/pacman.d/mirrorlist 文件顶端添加

```
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
```

更新软件包缓存

```
sudo pacman -Syy
```

安装密匙

```
sudo pacman -S archlinuxcn-keyring
```

### 安装桌面环境XFCE

#### 安装X

```
sudo pacman -S xorg
```

#### 安装桌面相关软件

```
sudo pacman -S xfce4 xfce4-goodies
```

#### 安装字体

```
sudo pacman -S ttf-dejavu wqy-bitmapfont wqy-microhei wqy-zenhei noto-fonts noto-fonts-emoji
```

#### 安装网络管理

```
sudo pacman -S networkmanager network-manager-applet
```

然后启用:

```
sudo systemctl enable NetworkManager.service
```

禁用wifi-menu:

```
sudo systemctl disable netctl.service
```

PS:[参考链接](https://bbs.archlinuxcn.org/viewtopic.php?id=3105)

#### 安装显示管理器

所谓登录界面

```
sudo pacman -S lightdm lightdm-gtk-greeter lightdm-gtk-greeter-settings
```

开机自启动

```
sudo systemctl enable lightdm
```

### 安装输入法

安装软件包

```
sudo pacman -S --noconfirm fcitx5 fcitx5-qt fcitx5-gtk fcitx5-qt4 fcitx5-chinese-addons fcitx5-configtool fcitx5-material-color fcitx5-pinyin-moegirl fcitx5-pinyin-zhwiki
```

编辑环境变量

```
nano ~/.xprofile
```

填入以下参数并注销桌面生效

```
export QT_IM_MODULE=fcitx5
export GTK_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
fcitx5 &
```

#### 安装Dock停靠栏

按住CTRL加鼠标左键会弹出设置界面

```
sudo pacman -S plank
```

##### 设置plank开机启动

在设置管理器里面找到Session and Startup（会话和启动），在Application Autostart（应用程序自启动）里面点击Add（添加）按钮，新增Plank登录自启动。

![plank](/img/20615571-3c9ebed44346d1f2.png)

#### 安装常用软件

yay包管理器

```
sudo pacman -S yay
```

安装yay编译工具

```
sudo pacman -S base-devel 
```

安装 snapper 快照管理实用工具

```
sudo pacman -S snapper 
```

谷歌浏览器

```
sudo pacman -S google-chrome
```

网易云音乐

```
sudo pacman -S netease-cloud-music
```

MD编辑器

```
sudo pacman -S typora
```

OBS录屏软件

```
sudo pacman -S obs-studio
```

