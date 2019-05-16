 Git学习总结一

##  一、在Windows上安装Git

在Windows上使用Git，可以从Git官网直接[下载安装程序](https://git-scm.com/downloads)，（网速慢的同学请移步[国内镜像](https://pan.baidu.com/s/1kU5OCOB#list/path=%2Fpub%2Fgit)），然后按默认选项安装即可。

安装完成后，在开始菜单里找到“Git”->“Git Bash”，蹦出一个类似命令行窗口的东西，就说明Git安装成功！

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190516195434.png)

安装完成后，还需要最后一步设置，在命令行输入：

```
$ git config --global user.name "Your Name"     //我们可以自定义用户名
$ git config --global user.email "email@example.com"  //我们也可以自定义邮箱
```

在这里我举例说明

```
$ git config --global user.name "JavaAllianceNotes"
$ git config --global user.email "1964327886@qq.com"
```

因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和Email地址。你也许会担心，如果有人故意冒充别人怎么办？这个不必担心，首先我们相信大家都是善良无知的群众，其次，真的有冒充的也是有办法可查的。

注意`git config`命令的`--global`参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。



## 二、创建版本库

什么是版本库呢？版本库又名仓库，英文名**repository**，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，这个目录里的每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

所以，创建一个版本库非常简单，首先，选择一个合适的地方，创建一个空目录：

```
$ mkdir JavaAlliance
$ cd JavaAlliance
$ pwd
/d/JavaAlliance     //我们是在D盘下创建的目录JavaAlliance
```

`pwd`命令用于显示当前目录

如果你使用Windows系统，为了避免遇到各种莫名其妙的问题，请确保目录名（包括父目录）不包含中文。

第二步，通过`git init`命令把这个目录变成Git可以管理的仓库：

```
$ git init
Initialized empty Git repository in /d/JavaAlliance/.git/
```

瞬间Git就把仓库建好了，而且告诉你是一个空的仓库（empty Git repository），细心的读者可以发现当前目录下多了一个`.git`的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。

如果你没有看到`.git`目录，那是因为这个目录默认是隐藏的，用`ls -ah`命令就可以看见。

**注意：**也不一定必须在空目录下创建Git仓库，选择一个已经有东西的目录也是可以的。不过，不建议你使用自己正在开发的公司项目来学习Git，否则造成的一切后果概不负责。



使用Windows的童鞋要特别注意：

千万不要使用Windows自带的**记事本**编辑任何文本文件。原因是Microsoft开发记事本的团队使用了一个非常弱智的行为来保存UTF-8编码的文件，他们自作聪明地在每个文件开头添加了0xefbbbf（十六进制）的字符，你会遇到很多不可思议的问题，比如，网页第一行可能会显示一个“?”，明明正确的程序一编译就报语法错误，等等，都是由记事本的弱智行为带来的。建议你下载[Notepad++](http://notepad-plus-plus.org/)代替记事本，不但功能强大，而且免费！记得把Notepad++的默认编码设置为UTF-8 without BOM即可：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190516200453.png)

言归正传，现在我们用Notepad++编写一个`readme.txt`文件，内容如下：

```
I am JavaAllianceNotes
Welcome your coming!!!
```

然后把该文件放到我们刚才创建的版本库目录下，D:\JavaAlliance\目录或者其子目录下即可

因为这是一个Git仓库，放到其他地方,就算Git再厉害也找不到这个文件。

**第一步**，用命令`git add`告诉Git，把文件添加到仓库：

```
$ git add readme.txt
```

执行上面的命令，没有任何显示，这就对了，Unix的哲学是“没有消息就是好消息”，说明添加成功。

**第二步**，用命令`git commit`告诉Git，把文件提交到仓库：

 （小疑问：第一步已经添加到仓库了，为什么还要提交到仓库 ？

  解答：`git commit`命令，`-m`后面输入的是本次提交的说明，如果仅靠git add操作只能说是把文件放入了仓库，但是还是不能正式使用，还需要“提交”才行，而git commit 操作相当于“提交”操作，并在提交文件之后要给该添加行为做一个说明，所以文件执行git add之后一定要git commit才生效）

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190516201715.png)

`git commit`命令执行成功后会告诉你，`1 file changed`：1个文件被改动（我们新添加的readme.txt文件）；`2 insertions`：插入了两行内容（readme.txt有两行内容）。

总结：为什么Git添加文件需要`add`，`commit`一共两步呢？**因为`commit`可以一次提交很多文件**，所以你可以多次`add`不同的文件，比如：

```
$ git add file1.txt    
$ git add file2.txt 
$ git add file3.txt
$ git commit -m "add  files" //可以看到上面两行都是执行的git add操作，而这一行代码git commit就把
                               上面的add的3个文件一起提交了
```



## 三、git status和git diff命令

按前面的操作之后，我们已经成功地添加并提交了一个readme.txt文件，现在，是时候继续工作了，于是，我们继续修改readme.txt文件，改成如下内容：

```
I am JavaAllianceNotes
Welcome your coming!!!
Thank you<A3><A1>
```

现在，运行`git status`命令看看结果：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190516203047.png)

`git status`命令可以让我们时刻掌握仓库当前的状态，上面的命令输出告诉我们，`readme.txt`被修改过了，但还没有提交该修改结果。

虽然Git告诉我们`readme.txt`被修改了，但如果能看看具体修改了什么内容，自然是很好的。比如你休假两周从国外回来，第一天上班时，已经记不清上次怎么修改的`readme.txt`，所以，需要用`git diff`这个命令看看：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190516203522.png)

`git diff`顾名思义就是查看difference，显示的格式正是Unix通用的diff格式，可以从上面的命令输出看到我们在原readme.txt文件的末尾又添加了一行”Thank you<A3><A1> “

知道了对`readme.txt`作了什么修改后，再把它提交到仓库就放心多了，提交修改和提交新文件是一样的两步，第一步是`git add`：

```
$ git add readme.txt
```

同样没有任何输出。在执行第二步`git commit`之前，我们再运行`git status`看看当前仓库的状态：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190516204255.png)

`git status`告诉我们，将要被提交的修改包括`readme.txt`，下一步，就可以放心地提交了：

```
$ git commit -m "add thank you<A3><A1>"
```

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190516204455.png)

提交后，我们再用`git status`命令看看仓库的当前状态：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190516204534.png)

Git告诉我们当前没有需要提交的修改，而且，工作目录是干净（working tree clean）的。

总结：就是每次修改了文件之后，都要重新git add ,然后git commit一下才能将修改之后的结果提交到版本库中去





##  四、版本回退

像这样，你不断对文件进行修改，然后不断提交修改到版本库里，就好比玩RPG游戏时，每通过一关就会自动把游戏状态存盘，如果某一关没过去，你还可以选择读取前一关的状态。有些时候，在打Boss之前，你会手动存盘，以便万一打Boss失败了，可以从最近的地方重新开始。Git也是一样，每当你觉得文件修改到一定程度的时候，就可以“保存一个快照”，这个快照在Git中被称为`commit`。一旦你把文件改乱了，或者误删了文件，还可以从最近的一个`commit`恢复，然后继续工作，而不是把几个月的工作成果全部丢失。

当然了，在实际工作中，我们脑子里怎么可能记得一个几千行的文件每次都改了什么内容，不然要版本控制系统干什么。版本控制系统肯定有某个命令可以告诉我们历史记录，在Git中，我们用`git log`命令查看：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190516210601.png)

`git log`命令显示从最近到最远的提交日志

如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数：   oneline就是”一行“的意思

需要友情提示的是，你看到的一大串类似`a58ef0825b...`的是`commit id`（版本号），和SVN不一样，Git的`commit id`不是1，2，3……递增的数字，而是一个SHA1计算出来的一个非常大的数字，用十六进制表示，而且你看到的`commit id`和我的肯定不一样，以你自己的为准。为什么`commit id`需要用这么一大串数字表示呢？因为Git是分布式的版本控制系统，后面我们还要研究多人在同一个版本库里工作，如果大家都用1，2，3……作为版本号，那肯定就冲突了。

每提交一个新版本，实际上Git就会把它们自动串成一条时间线。如果使用可视化工具查看Git历史，就可以更清楚地看到提交历史的时间线：



好了，现在我们启动时光穿梭机，准备把`readme.txt`回退到上一个版本，怎么做呢？

首先，Git必须知道当前版本是哪个版本，在Git中，用`HEAD`表示当前版本，也就是最新的提交`a58ef0825b...`（注意我的提交ID和你的肯定不一样），上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`。

现在，我们要把当前版本回退到上一个版本，就可以使用`git reset`命令：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190516213011.png)

`--hard`参数有啥意义？这个后面再讲，现在你先放心使用。

看看`readme.txt`的内容是不是之间的版本，发现果然被还原了

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190516211129.png)

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190516211018.png)

我们用`git log`再看看现在版本库的状态：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190516211327.png)

最新的那个版本`Thank you<A3><A1>`已经看不到了！好比你从21世纪坐时光穿梭机来到了19世纪，想再回去已经回不去了，肿么办？

办法其实还是有的，只要上面的命令行窗口还没有被关掉，你就可以顺着往上找啊找啊，找到那个`Thank you<A3><A1>`的`commit id`是`a58ef0825b...`，于是就可以指定回到未来的某个版本：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190516211552.png)

版本号(commit id)没必要写全，前几位就可以了，Git会自动去找。当然也不能只写前一两位，因为Git可能会找到多个版本号，就无法确定是哪一个了。

再小心翼翼地看看`readme.txt`的内容：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190516211926.png)

果然，我胡汉三又回来了。

Git的版本回退速度非常快，因为Git在内部有个指向当前版本的`HEAD`指针，当你回退版本的时候，Git仅仅是把HEAD从指向`Thank you<A3><A1>`

然后顺便把工作区的文件更新了。所以你让`HEAD`指向哪个版本号，你就把当前版本定位在哪。

现在，你回退到了某个版本，关掉了电脑，第二天早上就后悔了，想恢复到新版本怎么办？找不到新版本的`commit id`怎么办？

在Git中，总是有后悔药可以吃的,Git提供了一个命令`git reflog`用来记录你的每一次命令：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190516212246.png)

终于舒了口气，从输出可知，你每次修改操作的版本号id都展示出来了，现在，你又可以乘坐时光机回到未来了。

### 小结

现在总结一下：

- `HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。
- 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。
- 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。
