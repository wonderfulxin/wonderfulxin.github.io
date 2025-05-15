---
date: 2019-03-17
title: 使用 Git上传代码到coding.net代码仓库详解
tags:
  - 其他
---

摘要:使用 Git上传代码到coding.net代码仓库详解
<!--more-->

1.创建本地代码库

在本地创建一个文件夹，作为你上传代码的本地仓库，接下来就要把这个仓库与coding服务器端进行配置

在这个文件夹内点击右键，选择Git Bash Here，首先要初始化本地仓库，输入”git init”命令

![avatar](/img/20161110143539889.png)

接下来进行远程代码库克隆（前提：自行在coding中建立一个项目，空项目即可），输入”git clone https：//xxxxxx”命令，命令中的url获取方式如图： 
![avatar](/img/20161110143829127.png)

左下角HHTPS处即为要输入的url

将这个url粘贴到命令中，进行远程仓库的克隆，克隆时会出现输入账号密码的环节（coding注册时的账号和密码） 
![avatar](/img/20161110144142460.png)

输入账号密码之后便可以完成克隆。

2.代码推送

克隆之后在原来的文件夹中会多出一个文件夹（即从代码库中下载的文件夹），例如 
![avatar](/img/20161110144711307.png)

这时候，本地仓库的配置就完成了，将要上传的代码文件放入这个文件夹中，接下来要查看一下本地仓库的状态，一检查配置是否成功，进而进行代码的推送，输入命令”git status”

例如放入了一个“stringKMP.c”文件之后输入命令”git status”

![avatar](/img/20161110145017808.png)



输入git status命令后，会发现以红色字体打印出来的“stringKMP.c”，说明该文件存在于本地仓库，但并未推送到云端， 接下来，输入”git add 文件名”命令，可以再输入命令”git status”进行状态检查，如下
![avatar](/img/20161110145224254.png)


会发现出现了“new file”。

接下来， 输入”git commit -m “代码备注随便写” “命令提交

然后输入”git push origin master”命令推送到云端，origin是服务器，master是分枝。
![avatar](/img/20161110145428374.png)


一切结束后，输入”git status”查看本地代码状态，会用绿字显示，表示上传成功，进入coding.Net的项目主页，你会发现自己在本地推送的代码已经出现在项目中。
![avatar](/img/20161110153244326.png)

以上就是使用Git上传代码到coding的流程，希望对大家有所帮助。

再补充一个可以上传git到github的图片
![avatar](/img/gitshiyong.png)
