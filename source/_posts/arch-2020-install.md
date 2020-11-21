---
title: arch-2020-install
tags:
  - arch
  - install
abbrlink: 54552
date: 2020-11-19 13:39:04
---

### 网络

```
systemctl start dhcpcd
```

### 启动sshd

```
systemctl start sshd
```

#### 设置root密码

```
passwd
```

### 同步时间启用NTP

```
timedatectl set-ntp true
timedatectl status
```

### 分区EFl

```把DOS disklabel改为GPT disklabel```

![分区EFl](/img/lala.im_2020-08-08_09-49-20.png)

### 创建根分区

![根分区](/img/lala.im_2020-08-08_09-53-03.png)

### 创建文件系统btrfs

```
mkfs.fat -F32 /dev/sda1
mkfs.btrfs -m single -L btrfs-arch /dev/sda2
```

### 挂载分区

```
mount -o compress=lzo /dev/sda2 /mnt
mkdir /mnt/boot
mount /dev/sda1 /mnt/boot
```



## 装相关软件

基础系统

```
pacstrap /mnt base linux linux-firmware nano openssh grub efibootmgr
```

#### 生成fstab

```
genfstab -U /mnt >> /mnt/etc/fstab
```

#### chroot到系统

```
arch-chroot /mnt
```

##### 设置root密码并建立用户

```
passwd
# -G参数设置用户组，-m开关建立用户目录，username为你的用户名
useradd -G wheel -m username
# 给新用户设置密码。密码要输入两次，没有回显
passwd username
```

##### 安装sudo

```
pacman -S sudo vim vi
```

##### 编辑sudo配置文件并用vim打开

```
sudo EDITOR=vim visudo
```

##### 添加下面一行

```
%wheel ALL=(ALL) ALL
```

设置时区：

```
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
hwclock --systohc
```

本地化设置：

```
nano /etc/locale.gen
```

去掉下面的注释：

```
en_US.UTF-8 UTF-8
zh_CN.UTF-8 UTF-8
```

注：你需要桌面环境支持中文，就必须在这里配置zh_CN的locale。

重新生成配置：

```
locale-gen
```

创建locale.conf配置文件：

```
nano /etc/locale.conf
```

写入如下配置：

```
LANG=zh_CN.UTF-8
```

设置hostname：

```
echo ody > /etc/hostname
```

编辑hosts列表：

```
nano /etc/hosts
```

写入如下配置：

```
127.0.0.1    localhost
::1    localhost
127.0.0.1    ody
```

网络这块，由于我们需要用到桌面环境，所以选择使用networkmanager是更明智的。

如果你的网络是DHCP自动分配的，那么安装好了之后设置NetworkManager开机自启即可，无需做其他配置：

```
pacman -S networkmanager
systemctl enable NetworkManager
```

最后配置grub：

```
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg
```

### sshd开机启动

```
systemctl enable sshd
```





### 退出

PS: 基础环境安装完成

## 参考链接

[安装archlinuxUEFI+DDE桌面环境+常用软件](https://lala.im/7280.html)

[Arch Linux + KDE安装教程](http://anclark.github.io/2020/02/14/Struggle_with_Linux/Arch%20Linux%E4%B8%8EKDE%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B/)

