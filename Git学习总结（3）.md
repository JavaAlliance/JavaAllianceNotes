#  Git学习总结（3）

## 一、远程仓库

实际情况往往是这样，找一台电脑充当服务器的角色，每天24小时开机，其他每个人都从这个“服务器”仓库克隆一份到自己的电脑上，并且各自把各自的提交推送到服务器仓库里，也从服务器仓库中拉取别人的提交。

完全可以自己搭建一台运行Git的服务器，不过现阶段，为了学Git先搭个服务器绝对是小题大作。好在这个世界上有个叫[GitHub](https://github.com/)的神奇的网站，从名字就可以看出，这个网站就是提供Git仓库托管服务的，所以，只要注册一个GitHub账号，就可以免费获得Git远程仓库。

在继续阅读后续内容前，请自行注册GitHub账号。由于你的本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以，需要一点设置：

**第1步：**创建SSH Key。**在用户主目录下**，看看有没有.ssh目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

```
$ ssh-keygen -t rsa -C "youremail@example.com"   //这里我们填写自己的邮箱
```

你需要把邮件地址换成你自己的邮件地址，**然后一路回车**，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。

如果一切顺利的话，可以在用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。



**第2步：**登陆GitHub，打开“Account settings”，“SSH Keys”页面：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190517114215.png)

然后，点“SSH and GPG keys”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190517114345.png)

点“Add Key”，你就应该看到已经添加的Key：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190517114754.png)

为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。

当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。

最后友情提示，在GitHub上免费托管的Git仓库，任何人都可以看到喔（但只有你自己才能改）。所以，不要把敏感信息放进去。

如果你不想让别人看到Git库，有两个办法，一个是交点保护费，让GitHub把公开的仓库变成私有的，这样别人就看不见了（不可读更不可写）。另一个办法是自己动手，搭一个Git服务器，因为是你自己的Git服务器，所以别人也是看不见的。这个方法我们后面会讲到的，相当简单，公司内部开发必备。

确保你拥有一个GitHub账号后，我们就即将开始远程仓库的学习。



##  二、添加远程库

现在的情景是，你已经在本地创建了一个Git仓库后（也就是我们本地的版本库），又想在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步，这样，GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作，真是一举多得。

```
$ git remote add origin git@github.com:JavaAlliance/JavaAllianceNotes.git
```

请千万注意，把上面的`JavaAlliance`替换成你自己的GitHub账户名，否则，你在本地关联的就是我的远程库，关联没有问题，但是你以后推送是推不上去的，因为你的SSH Key公钥不在我的账户列表中。

添加后，远程库的名字就是`origin`，这是Git默认的叫法，也可以改成别的，但是`origin`这个名字一看就知道是远程库。

下一步，就可以把本地库的所有内容推送到远程库上：

如果我们在Github上的仓库是空 库，那么就执行下面这条命令

```
$ git push -u origin master
```

如果不是空库，而是一个已经存在的仓库的话，那就执行下面这条命令，把Github远程仓库里的东西拷贝到本地一份，进行远程和本地的同步

```
$  git pull origin master --allow-unrelated-histories
```

**有可能会遇到的问题：** 

**小黑框问题：** 有时候往GitHub上提交东西的时候会因为remote repository上有东西更新了但是local

repository 没有更新而造成提交失败，此时需要git pull origin master,这样就会出现一个小黑框，我们只需要按"i"

然后随便输入合并的信息，然后按Esc，最后按：wq退出

**ssh警告问题：**当你第一次使用Git的`clone`或者`push`命令连接GitHub时，会得到一个警告：

```
The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.
RSA key fingerprint is xx.xx.xx.xx.xx.
Are you sure you want to continue connecting (yes/no)?
```

这是因为Git使用SSH连接，而SSH连接在第一次验证GitHub服务器的Key时，需要你确认GitHub的Key的指纹信息是否真的来自GitHub的服务器，输入`yes`回车即可。

Git会输出一个警告，告诉你已经把GitHub的Key添加到本机的一个信任列表里了

```
Warning: Permanently added 'github.com' (RSA) to the list of known hosts.
```

这个警告只会出现一次，后面的操作就不会有任何警告了。

如果你实在担心有人冒充GitHub服务器，输入`yes`前可以对照[GitHub的RSA Key的指纹信息](https://help.github.com/articles/what-are-github-s-ssh-key-fingerprints/)是否与SSH连接给出的一致。



然后可以再执行下面这条命令，重新初始化本地仓库

```
$ git init
```

  然后在把本地的仓库内容推送到远程仓库

```
$ git push -u origin master
```

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190517145017.png)

把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。

（注意：如果第一次我们连接的远程仓库是空的话，由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，即`$ git push -u origin master`，Git不但会把本地的`master`分支内容推送到远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。）



推送成功后，可以立刻在GitHub页面中看到远程库的内容已经和本地一模一样：

从现在起，只要本地作了提交，就可以通过命令：

```
$ git push origin master
```

把本地`master`分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！



### 小结

要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`；

关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；

分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！



##  三、从远程库克隆

上次我们讲了先有本地库，后有远程库的时候，然后两者进行关联。（**本地有库，远程也有库，两者进行关联起来，并同步信息**）

接下来我们再讲一下 从远程库克隆！（**就是说本地没有库，然后从远程克隆一个到本地上**。这就是“远程库克隆”跟本文的标题二“添加远程库”讲的内容的区别）

现在，假设我们从零开发，那么最好的方式是先创建远程库，然后，从远程库克隆。

首先，登陆GitHub，创建一个新的仓库，名字叫`gitskills`：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190517151554.png)

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190517151650.png)

我们勾选`Initialize this repository with a README`，这样GitHub会自动为我们创建一个`README.md`文件。创建完毕后，可以看到`README.md`文件：



现在，远程库已经准备好了，下一步是用命令`git clone`克隆一个本地库：

```
$ git clone git@github.com:michaelliao/gitskills.git
```

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190517152007.png)

注意把Git库的地址换成你自己的，然后进入`gitskills`目录（在哪个目录执行的git clone，那么最后clone的结果就存放在哪个目录下，如果没有在任何目录下执行，那就是命令行对应的目录了，win + R > cmd 查看命令行地址，然后进该地址去找即可）看看，已经有`README.md`文件了：

```
$ cd gitskills
$ ls
README.md
```

如果有多个人协作开发，那么每个人各自从远程克隆一份就可以了。

你也许还注意到，GitHub给出的地址不止一个，还可以用`https://github.com/michaelliao/gitskills.git`（解释：采用的是http协议）这样的地址。实际上，Git支持多种协议，默认的`git://`使用ssh，但也可以使用`https`等其他协议。

使用`https`除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放http端口的公司内部就无法使用`ssh`协议而只能用`https`。

### 小结

要克隆一个仓库，首先必须知道仓库的地址，然后使用`git clone`命令克隆。

Git支持多种协议，包括`https`，但通过`ssh`支持的原生`git`协议速度最快。