---
layout: post
title: 树莓派4B 玩耍记录 （1）
date: 2020-12-24
tags:
---

前几天有点馋，我和家长算计着从国内折腾一波零食快递到爱尔兰。正好我们买了自己到房子，虽然家具什么都买了，电器还是少了一些。一合计一商量，我们就把当贝 F3 塞进了购物车。看电子器件到时候，无意见看到了树莓派 4B 了，现在这玩意再也不是 800Mhz 垃圾 CPU 了，1.4Ghz 4c + 8G 内存硬件上看着还是很美好的。自己一上头，就买了一个。

大概等待了三周拿到了我的树莓派，直接开始上手。大致思考了一下自己到需求，写了以下列表：

1. 需要一个轻量级到 Linux 编程环境，支持 Typecript 和 Python
2. 有一个家用到 NSA 硬盘
3. 玩一些复古游戏，GBA 啥的
4. 给投影仪提供片源

回想了以下以前把玩到记录，基本上就是一个树莓派干一个任务。然而既然过了这么多年硬件性能上去了，那么我浪一些也完全没有问题对吧！可以四倍任务对吧！

这次到树莓派因为内存加到了 8G，直接运行 32 位 OS 估计就要吃屎了。研究了下貌似官方整合了 Raspbian，发布了 Raspberry Pi OS。提供的 Lite 版本纯命令行，虽然很变态，但是我喜欢。步骤基本就是下载镜像文件然后拿官方提供的工具点点点。没啥难度，注意 Rom 必须自己下，不然会得到一个 32 位的 OS，你的 8G ram 分分钟变 4G。

[链接](https://www.raspberrypi.org/software/operating-systems/)在这里。

接上电源，插电！开机！[配合视频味道更佳](https://www.bilibili.com/video/av242531951/)。一上来先配置用户，把默认密码给改了。

```bash
sudo adduser [username]
sudo adduser [username]
sudo passed pi # default user, change the login password.
```

由于想写代码还是需要一个 GUI 的，所以按照官方步骤安装 GUI。

```bash
sudo apt-get install -y raspberrypi-ui-mods rpi-chromium-mods
```

因为这个安装会默认把开机修改成桌面，所以我们还需要去`sudo raspi-config` 里面修改以下开机选项。

1. 上个 Wifi，没网玩个卵子！  
   Software Option -> Wireless Lan
2. 2020 了，赶紧的上个 1080P，眯着眼睛看 4k 屏里的小字快要疯了！
   Display Option -> Resolution
3. 修改下键盘，安装 OS 的时候读取了我的地区，键盘编程了英标英标最垃圾了！  
   Localisation Option -> Locale

然后，硬是没找到开机设置命令行，默默到走进了桌面点点点...

这样就得到了一个基本可以跑到 OS。先开个坑，下次继续。瞄了一眼系统资源，带桌面占用内存 200MB，1%CPU，很好，我对性能很自信！
