---
layout: post
title: 树莓派4B 玩耍记录 （2）
date: 2020-12-27 20:42:45
tags:
  - tech-blog
categories:
  - tech-blog
---

基本上折腾好了树莓派的操作系统，也修改了默认登陆为终端。我接下来开始准备折腾下自己的编程环境。倒也不是需要直接在树莓派上面写代码，不过我还是很希望可以提供一个远程的代码环境。虽然我依旧有了 github 的 workspace，免费的真香，但是笔记树莓派可以直接接触硬件，有些 OS 相关的代码大概还是得在这玩意上面写。

因为淘宝给我发了 64GB 的内存卡，感觉有点不够花。一想还是得用云平台，翻出了自己的 MSDN 账号，搞了 140 刀每月的 Azure 用量。开了一个 Storage Account。这样可以把一部分不常用的文件塞进远程托管了。

![Computer Image Storage](https://parallelcomputing.readthedocs.io/zh/latest/_images/CengCiCunChu.jpg)

至于打包完成的软件，其实也没有必要堆在本地，直接塞进 Docker 然后把镜像放远程就好。这样基本可以保证我的本地只有代码和当前执行的一些依赖。最后再搞一个自动运行的清理编译缓存的 cron，世界就清净了。

废话少说，先开云平台的账号，我这里选择的巨硬的 Azure，主要就是 Credit 给的多，我直呼爸爸！我用了两个产品，[Azure Container Retristry](https://azure.microsoft.com/en-us/services/container-registry/) 和 [Azure Storage Account](https://docs.microsoft.com/en-us/azure/storage/common/storage-account-overview).

第一个用于托管 Docker 镜像，一个用于托管文件。基本就是去官网点点点。折腾完了以后我开始调教[Azure Cli](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)，我现在也没有 GUI，有个命令行还是不能少的。

然后安装完我发现，这个傻逼 CLI 居然给 Debian 的安装包有问题。安装完直接内部指定了 AMD64 版本的 Python，而我们可爱的树莓派，是屌丝 ARMHF 的。你是不是逗我玩？

因为是 Python 的依赖关系，那我们就直接跳过包管理呗。

```bash
pip3 install azure-cli

```

然后开始认证自己的账号：

```bash
az login
az account show
```

一切 OK。再来搞一个 Docker，CE 版本的 Docker 原生支持 ARM。这里需要注意的是，Docker 会吃掉一定内存，所以官方的原话也是，你的树莓派要是没有个 2G 内存，你就特么别出来玩，~~别出来丢人，赶紧的花钱买个好的~~。

```bash
sudo apt-get update && sudo apt-get upgrade
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# Add User to docker Group
sudo usermod -aG docker [user_name]
```

测试了以下看起来一切 OK，命令如下。“Hello World” 真是测试神 Container。然后给他推上 Azure。本地干净了。

```bash
docker run hello-world
# push image to remote
az acr login --name [acr registry name]
docker login [acr registry name].azurecr.io
docker tag helo-world [acr registry name].azurecr.io/hello-world
docker push [acr registry name].azurecr.io/hello-world
docker rmi [acr registry name].azurecr.io/hello-world
docker rmi hello-world
```

然后开始做远程文件夹，直接支持了 SMB3.0，在树莓派上面其实只要挂在上就可以了。Azure Storage Account 专门提供了脚本生成器，我们需要做的是写入本地文件，给足够的权限运行就 ok。

挂载完了以后，加上一个超链接：

```bash
mount --rbind [mount driver path] [easy access path]
```

由于重启会导致超链接失效，所以还需要去 `/etc/fstab` 添加开机重链接项：

```bash
[mount driver path] [easy access path] cifs rw,relatime,vers=3.0,cache=strict,username=raspberrystoragecz,credentials=/etc/smbcredentials/raspberrystoragecz.cred,uid=0,noforceuid,gid=0,noforcegid,addr=20.150.40.104,file_mode=0777,dir_mode=0777,soft,persistenthandles,nounix,serverino,mapposix,rsize=1048576,wsize=1048576,bsize=1048576,echo_interval=60,actimeo=1 0 0
```

这样你放入 `[easy access path]` 的所有文件，就会自动同步到云端。基本上是一个究极简化版本的 NSA。因为我家还有当贝 F3，后面我配置当贝的时候用了这个 NSA 当作传文件的转载。（拔插 USB 太精神污染）。
