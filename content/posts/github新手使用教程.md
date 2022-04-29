+++
title = "Github新手使用教程"
date = "2022-04-29T22:39:59+08:00"
author = "[Wizzz]"
authorTwitter = "" #do not include @
cover = ""
tags = ["github"]
keywords = ["", ""]
description = "给纯新手阅读的github使用教程"
showFullContent = false
readingTime = false
hideComments = false

+++

应要求写的一份

## 为纯新手考虑的github使用教程！

首先，github的国内访问一直不稳定，虽然可以用改host的方式浅解决一下，但因为这是为纯新手准备的所以建议直接用魔法进行超空间信息连接。因为是黑魔法所以方法不列。

### 注册

请在github.com

上注册一个帐号。

### git

然后，进入git官网

https://git-scm.com/

点右侧的Download for [你的操作系统]，下载完成后安装步骤如：

1. 
   点右侧的Download for Windows，下载完成后安装步骤如：
   
   windows： https://www.jianshu.com/p/414ccd423efc

2.
   Mac用户请转如下博客进行安装与基本使用教程 https://www.csdn.net/tags/MtjaIgxsMzUzNjUtYmxvZwO0O0OO0O0O.html

3.  [ Linux: ] 用linux的人不会连git都不知道怎么安吧

### 在github上对项目进行贡献

因为写这篇文章真正的目的和我懒得写的缘故，所以如何创建自己的仓库我就略了。

当然，这个项目必须你为项目所有者或者所有者把你设置为合作者才可以直接对其修改的。

如果不是合作者的话，点击右上方的fork按钮按这个分支创建完全属于自己的新仓库，修改后向原作者提交合并请求，原作者同意后即可。

下面默认你为合作者。

登录github，在左上角的搜索栏中搜索关键词，比如https://github.com/iku50/iku50.github.io

点击进入该库后，找到绿色的code按钮，默认是https的clone方式，这就对了。点右侧的复制按钮将此地址一键复制。

在本地的某个空文件夹内，windows是用右键选中用git bash打开，输入命令

```shell
git clone [刚刚复制的url地址]
```

等待！

然后等它下载完毕就可以看到整个项目都已经在你的文件夹内了。

然后我们试图往文件夹内加点东西。

比如把一张图片放进去？

就像你平常那样操作就好了。

然后用git bash输入命令。

```shell
git add . //这个点指的是加入所有文件到暂存区,或者你也可以把.改为具体的文件名。
```

```shell
git commit -m "更改信息,这里填啥都可以，主要是填你做的哪些改变"//指提交信息到本地仓库
```

```shell
git push origin main:main//前面的origin是远程主机名（一般都默认叫这个），前一个main是本地分支名，后一个是远程分支名，像这种远程分支和本地分支一个名字的可以省略掉后面的:main，即

git push origin main
```

当然有时git会提醒你远程和本地分支不同步所以无法上传，这是因为有其他人更新了远程分支，这时，需要用

```shell
git pull origin main:main//即从远程的origin主机的main分支合并到本地的main分支里面。这里后面的main是本地分支名，如果远程分支是与当前分支合并那么可以省略。
```

然后再试一遍push看看！应该可以上传了。

更多的分支操作可以查看以下教程https://www.runoob.com/git/git-tutorial.html

然后登录github找到该项目，可以看到你已经成功了！

### 不需要敲代码的方式！

在提醒之下，我突然回忆起不敲代码也能上传文件和下载文件！

这样连安装git都不用！只要注册就好了！

#### 下载项目（如果只要上传文件的话这步也能略！）

还是那个绿色的code按钮，点击选择Download Zip！

于是就直接下载好了压缩包。

#### 上传文件

先在项目仓库上切换到需要上传到的文件夹。

点code旁边的add file按钮，选择upload files。

直接把要上传的文件拖到Drag...那个区域

默认吧默认就行。

点绿色的commit changes就好了。