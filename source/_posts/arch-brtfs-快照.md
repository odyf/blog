---
title: arch-brtfs-快照-arch总结
tags:
  - brtfs
  - arch
  - 快照
  - 恢复系统
abbrlink: 10377
date: 2020-11-23 08:34:34
---

## 序言

在学习arch之前ody特意学习了文件系统相关的的知识SDS(软件定义存储)brtfs，zfs,nas存储，可以说这个方案适合有一定linux知识又不想在系统出错的情况下，```在实体机器上快速恢复系统```,以下是我的分区

![分区结构](/img/2020-11-23_08-59-48.png)

前面的arch安装与美化也可以看出ody的电脑只有一个一盘分区也是只分了sda1 boot分区与sda2 /分区这两个分区，然后/分区有文件系统是btrfs,我们需要的就是btrfs文件系统的高级特性快照，适合以下情况，更新系统出错、安装软件报错

### 安装与使用

安装

```
sudo pacman -S  snapper
```

创建配置文件

```
sudo snapper -c 配置名称  create-config 子卷路径
```

删除snapper配置文件

```
sudo snapper -c 配置名称 delete-config
```

列出配置文件

```
sudo snapper list-configs
```

#### 拍摄快照

```
sudo snapper -c 配置名称 create 选项
```



|        选项         |        描述        |
| :-----------------: | :----------------: |
|         -p          |    打印快照编号    |
| -c<number\|timeline |    指定清理算法    |
|  -description=描述  | 在快照后面添加描述 |

##### 举例

在根目录下拍摄一个arch-a配置文件的并描述为```根实验tets```的快照并打印编号

```
sudo snapper -c aech-a create --description=根实验test -p
```

列出快照

```
sudo snapper -c 配置名称 list
```

举例

列出arch-a的所有快照

```
sudo snapper -c arch-a list
```

![实验结果](/img/2020-11-23_10-24-56.png)

##### 删除快照

删除快照1

```
sudo snapper -c arch-a delete 1
```

批量删除

删除快照1删除快照2删除快照3

```
sudo napper -c arch-a delete 1..2...3
```

#### 回滚快照

基本用法

建议回滚前建立一个快照

```
sudo snapper -c 配置名称 undochange 快照编号..0
```

举例

回滚配置名称为arch-a 快照编号3的命令

```
sudo snapper -c arch-a undochange 3..0
```

以上就是常用命令