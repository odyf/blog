---
title: arch-2020-xfce桌面安装美化
tags:
  - arch
  - 美化
  - xfce
  - lightdm
abbrlink: 45862
date: 2020-11-21 11:08:19
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

pacman 卸载操作

```
sudo pacman -Rs
```

2020-11-21更新 已安装sddm管理器lightdm安装后登陆的一瞬间画面撕裂sddm就没有这个问题

sddm安装

```
sudo pacman -S sddm
```

开机启动sddm

```
sudo systemctl enable sddm
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

#### 安装声音驱动

```
sudo pacman -S alsa-utils pulseaudio pulseaudio-alsa
```

xfce4 panel里面的插件配合使用,必须要有这个进程

```
sudo pacman -S pavucontrol 
```

#### 桌面解压软件

```
sudo pacman -S thunar-archive-plugin xarchiver zip unzip p7zip arj lzop cpio unrar
```

#### 文件管理器中访问共享

```
sudo pacman -S gvfs gvfs-smb sshfs
```

#### 禁止主板蜂鸣提示音

重启生效

```
echo "blacklist pcspkr" > /etc/modprobe.d/nobeep.conf
exit
```

禁止充电嘟嘟声音

```
在BIOS里使用左右方向键切换到“Configuration”配置项;Power Beep这个选项关闭就行
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

桌面搜索软件

```
sudo pacman -S albert
```

笔记本电脑电池优化

```
sudo pacman -S tlp
sudo systemctl start tlp 
sudo systemctl enable tlp

```

v2ray

```
yay -S qv2ray-dev-git v2ray

```

启用NTFS支持

```
yay -S ntfs-3g-fuse
```

安装媒体组件

```
sudo pacman -S libva libva-intel-driver libva-vdpau-driver libva-utils
# 检查硬件加速库状态，会显示libva库的版本与显卡信息
vainfo
```

lvc播放器

```
sudo pacman -S vlc 
```

Git图形化工具gitkraken

```
yay -S gitkraken
```

汉化gitkraken 

```
git clone https://github.com/k-skye/gitkraken-chinese.git
cd gitkraken-chinese
sudo cp gitkraken-chinese/strings.json   /opt/gitkraken/resources/app.asar.unpacked/src/

# yay查看安装后的包路径命令
yay -Ql gitkraken
```

