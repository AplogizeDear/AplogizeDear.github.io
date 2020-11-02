---
title: homebrew使用
date: 2020-11-02 22:29:04
categories: 知识
tag: homebrew
---


1. ##### 附上homeBrew的官方链接 

   https://brew.sh/index_zh-cn

2. ##### 安装homeBrew

   ```
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
   ```

3. ##### 使用Homebrew 安装 你的mac没有预装但你需要的东西，如果想要安装应用要用下面那条命令哦

   比如：brew install wget

   ​			brew install git

   wget：用来下载文件的。可以控制下载文件的进度，比较稳定。

   基本上不会丢包，有些网站只有专门的代理才可以下载，相信wget可以帮到你。

4. ##### 用brew cask 安装应用

   Brew cask 安装

   ```
   brew install  brew-cask 
   ```

   比如：

   ```
   brew cask install firefox
   brew cask install qq
   ```

5. #### 什么？homebrew安装很慢！！

   清华大学有个镜像，可以换成这个。

   https://mirrors.tuna.tsinghua.edu.cn/help/homebrew/

   

   这是另一篇教程https://juejin.im/post/6850418117194022919

   简单概述为

   ```
   cd "$(brew --repo)"
   git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git
   cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
   git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git
   
   brew update
   ```

   这里有个一个命令解决下载缓慢的问题

   ```
   /bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
   ```

   在你的mac终端跑一下就可以了，时间可能比较长。
   
6. #### 卸载

   ```
    /bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/HomebrewUninstall.sh)"
   ```

7. 实在不会安装了

   https://zhuanlan.zhihu.com/p/111014448


