[toc]

# 下载homebrew

brew 的官方网站： http://brew.sh/  在官方网站对brew的用法进行了详细的描述

安装方法： 在Mac中打开Termal: 输入命令：

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

官方的方法需要翻墙

使用国内gitee镜像进行安装

```
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
```

![image-20220130203412290](https://gitee.com/tang-zhanpeng/blog-img/raw/master/img/image-20220130203412290.png)

# brew命令

1. 本地软件库列表：brew ls

2. 查找软件：brew search google（其中google替换为要查找的关键字）

3. 查看brew版本：brew -v 更新brew版本：brew update

4. 安装cask软件：brew install --cask firefox 把firefox换成你要安装的

5. Homebrew 常用命令一览

   $ brew --help #简洁命令帮助

   $ man brew #完整命令帮助

   $ brew install git #安装软件包(这里是示例安装的Git版本控制)

   $ brew uninstall git #卸载软件包

   $ brew search git #搜索软件包

   $ brew list #显示已经安装的所有软件包

   $ brew update #同步远程最新更新情况，对本机已经安装并有更新的软件用*标明

   $ brew outdated #查看已安装的哪些软件包需要更新

   $ brew upgrade git #更新单个软件包

   $ brew info git #查看软件包信息

   $ brew home git #访问软件包官方站

   $ brew cleanup #清理所有已安装软件包的历史老版本

   $ brew cleanup git #清理单个已安装软件包的历史版本