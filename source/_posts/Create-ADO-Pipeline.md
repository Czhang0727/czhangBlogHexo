---
layout: post
title: 给Hexo建立CICD Pipeline
date: 2020-12-27 09:39:57
tags:
---

上次久违的写了博客以后，发现每次需要手动更新 Github Page 有点心累，就开始折腾 CICD，为了给 Github 东家打个广告，我决定原生 Azure Devops Pipeline 来一套。

上来注册一个账号，直接输入 Github 账户密码，然后建一个 Pipeline。在 Script 项目里面添加本地更新命令行：

```bash
hexo g -d
```

咋么简单？结果当然是 GG 啦。首先我发现在终端里面出现了 “No Layout” 的错误信息。并且生成的文件没有 css 和 js，老师这肯定不对啊。仔细观摩了我的文件结构，发现 themes 下面的 Next 因为没有运行安装命令，所以在 Repo 里面只是留下了一个 Link。

看来原因是缺少了 submodule，先更新 Repo 本身

```bash
rm -rf themes/next
git rm -rf themes/next
```

然后在 azure-pipeline.yml 文件里面的 script 内加载 submodule。

```
git submodule update --init
```

顺便清理掉了以前因为傻逼才提交的 public/db.json/.deploy_git 等一票文件。更新下.gitignore。

这样文件算是顺利生成了。

然而还是提交失败，错误信息提示是没有权限。脑子一拍忽然意识到，我要更新的不是这个 Repo 本身，而是另外一个 Repo！！

查阅了 ADO pipeline 的文档，发现可以用 ssh 证书来管理。真是太妙了。

先在本地生成一个 ssh 证书，把 public 文件丢给 Github，再把 private 文件丢给 ADO Pipline，最后在 azure-pipeline.yml 里面新加一个任务“DownloadSecureFile”。

```yaml
- task: DownloadSecureFile@1
  inputs:
    secureFile: "keys"
```

插电，开机。依旧是没有权限...于是就陷入了艰难的谷歌之旅。好在阅读了 Hexo 的文档，发现他们家支持的是 Token，而且貌似因为文档更新不及时，token 的配置没有一个明确的说法，在折腾了两次以后，emm，还是决定手动更新！不就是新建文件夹然后`git push`么？老子也会啊！

于是我的脚本就膨胀成了。

```yaml
- script: |
    npm install hexo-cli -g
    yarn install
    mkdir ~/.ssh && mv $DOWNLOADSECUREFILE_SECUREFILEPATH ~/.ssh/id_rsa
    chmod 700 ~/.ssh && chmod 600 ~/.ssh/id_rsa
    ssh-keyscan -t rsa github.com >> ~/.ssh/known_hosts
    git config --global user.email "czhang0727@gmail.com"
    git config --global user.name "Azure CI/CD Agent"
    git submodule update --init
    hexo g
    ls -l public/
    mkdir deploy
    cd deploy
    git init
    git config --global user.email "czhang0727@gmail.com"
    git config --global user.name "ADO Deployment Agent"
    git remote add origin git@github.com:Czhang0727/czhang0727.github.io.git
    git fetch origin
    git checkout master
    git branch --set-upstream-to=origin/master master
    git pull
    yes | rm -rf ./*
    yes | cp -rf ../public/* ./
    git add --all
    git commit --allow-empty -m "CI/Deploy From Azure Pipeline"
    git push origin master
```

看着有点乱。不过还算是该有的都有了，这次总算是跑过了，也算是可以在圣诞假期再干些别的事情了。美滋滋。

和老婆一起吃吃饭，看看剧不香么？大家圣诞快乐！
