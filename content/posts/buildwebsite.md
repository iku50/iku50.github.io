---
title: "建站！"
date: 2022-04-25T14:41:07+08:00
draft: false
description: "本文用来展示用hugo等工具建站的流程"
author: "[Wizzz]"
tags: ["hugo","website"]
---

## 用hugo+github pages托管+阿里云ecs建站

之前是用wordpress建站的，但实在受不了它每次超慢的线上操作，而且对md格式文件的支持也不够好。因此在查询资料后决定通过hugo建立静态博客。

### 前期准备

#### 0. 资源准备

1. 阿里云ECS服务器（或者别的什么腾讯云等服务器也行，因为github pages托管在国内访问速度太慢，所以用DNS解析到国内的服务器绑定的域名上）。
2. go环境安装。
3. 一个自己拥有的独立域名（10块钱买一个不成问题）。

#### 1. hugo安装

由于我是manjaro系统，且之前早就已经安装好了go环境，所以我只需要用snap直接安装hugo即可。

```shell
sudo snap install hugo          
```

然后检查一下版本看是否安装成功。

```shell
>>> hugo version    
hugo v0.96.0-2fd4a7d3d6845e75f8b8ae3a2a7bd91438967bbb linux/amd64 BuildDate=2022-03-26T09:15:58Z VendorInfo=mage
```

#### 2. github pages库创建

在自己的github上建一个新的仓库，名为`用户名.github.io`。

注意该仓库必须是public的。

在本地创建一个新文件夹，用git把仓库克隆下来。

```shell
git clone <仓库的url>
```

### 建站

#### 1. 本地网站部署

在此目录下打开终端，敲入一下命令创建网站。

```shell
hugo new site .  #如果显示说此目录下已经有文件就在后面加个' --force'强制操作
```

在跳出来的信息中。

```shell
Congratulations! Your new Hugo site is created in /home/rickeee/website/test.

Just a few more steps and you're ready to go:

1. Download a theme into the same-named folder.
   Choose a theme from https://themes.gohugo.io/ or
   create your own with the "hugo new theme <THEMENAME>" command.
2. Perhaps you want to add some content. You can add single files
   with "hugo new <SECTIONNAME>/<FILENAME>.<FORMAT>".
3. Start the built-in live server via "hugo server".

Visit https://gohugo.io/ for quickstart guide and full documentation.

```

可以看到`Choose a theme from https://themes.gohugo.io/`这一项，直接点链接或者复制到浏览器打开挑选喜欢的主题。

![hugothemecut](/media/buildwebsite/hugothemecut.png)

挑选好后查看其github页面获取url，再在网站目录里用git下载。

```shell
git init
git submodule add <主题的url地址>
```

采用`submodule`管理theme会比较好更新管理。

进入theme文件夹找到该主题。进入该主题文件夹找到`exampleSite/config.toml`文件，复制下来并覆盖掉根目录下的同名文件。

打开该文件，修改一下要修改的属性（网站名，副标题，该主题希望你进行选择的一些参数等）（一般旁边有注释帮助理解）（还有一些必须修改的稍后讲）

可以在根目录打开终端用以下命令构建网站：

```shell
hugo
```

开启网站服务

```shell
hugo server
```

在浏览器中打开`http://localhost:1313`看看效果

#### 4. github pages托管

记得之前的`config.toml`文件吗，其中有一项是需要修改的。

```toml
baseURL = "https://<github仓库的url>"
```

在根目录用git push到远程仓库上

在github上设置page参数

![githubsettingcut](/media/buildwebsite/githubsettingcut.png)

把source选项中的`/(root)`改成`/docs`。方便后面生成。

在本地的网站根目录下打开终端改生成网页的目录到docs文件夹下（默认会生成在public文件下所以需要改）

```shell
hugo -d docs
```

然后就可以打开`https://<库url>`看到自己的网页了

#### 5. DNS解析设置

在如果是阿里云上购买的域名，那么在阿里云的控制台上进入域名设置界面，为服务器绑定域名（主机记录为@，值为服务器的公网ip地址），并添加一个CNAME类型的记录，主机记录可以是`blog`，值就是github pages部署的自己网站的地址。

设置完后在网站根目录的static文件夹下创建一个CNAME文件，打开并写入设置的域名（若前面主机记录是blog即`blog.<你的域名>`）。

重新生成一下并`push`到github上。

改变github pages设置中的custom domian为设置的新域名。

![blogsetingdomaincut](/media/buildwebsite/blogsetingdomaincut.png)

然后就可以用自己的域名访问了。

### 其他注意事项

写博客时用：

```shell
hugo new posts/<文章名.md>
```

创建新文章。

写完后记得把文章头部的`draft`属性设置为`false`，不然就是默认为草稿不生成文章。
