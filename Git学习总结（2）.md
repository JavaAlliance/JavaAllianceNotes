# Git学习总结（2）

##  一、工作区和暂存区

Git和其他版本控制系统如SVN的一个不同之处就是有暂存区的概念。

先来看名词解释。

#### 工作区（Working Directory）

就是你在电脑里能看到的目录，比如D:\JavaAlliance\这个目录就是一个工作区

 ![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190516213846.png)



#### 版本库（Repository）

工作区有一个隐藏目录`.git`，这个.git目录不算工作区，而是Git的版本库。（**区别：**D:\JavaAlliance是工作区，但是该目录下的.git目录不是工作区）

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个**分支**`master`，以及指向`master`的一个指针叫`HEAD`。（**解释：**这些结构我们知道就行了，而不是说我们点击”.git“隐藏文件去发现这样的stage文件和master文件，你是找不到的）

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190516214236.png)

分支和`HEAD`的概念我们以后再讲。

前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：

第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，现在，`git commit`就是往`master`分支上提交更改。

你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

俗话说，实践出真知。现在，我们再练习一遍，先对`readme.txt`做个修改，比如把readme.txt里面的所有内容改成下面的：

```
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
```

然后我们再在D:\JavaAlliance下新建一个名为LICENSE的文本文件，内容可以随便写

先用`git status`查看一下状态：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190516220018.png)



Git非常清楚地告诉我们，`readme.txt`被修改了（因为上面的是modified：），而`LICENSE`还从来没有被添加过，所以它的状态是`Untracked`。

现在，使用两次命令`git add`，把`readme.txt`和`LICENSE`都添加后，用`git status`再查看一下：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190516220228.png)

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190516220316.png)

现在，暂存区的状态就变成这样了：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190516220423.png)

所以，`git add`命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行`git commit`就可以一次性把暂存区的所有修改提交到分支。

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190516220516.png)

一旦提交后，如果你又没有对工作区做任何修改，那么工作区就是“干净”的：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190516220604.png)

现在版本库变成了这样，暂存区就没有任何内容了：（因为已经用git commit提交过去了）

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190516220651.png)

##  二、管理修改

现在，假定你已经完全掌握了暂存区的概念。下面，我们要讨论的就是，为什么Git比其他版本控制系统设计得优秀，因为Git跟踪并管理的是修改，而非文件。

你会问，什么是修改？比如你新增了一行，这就是一个修改，删除了一行，也是一个修改，更改了某些字符，也是一个修改，删了一些又加了一些，也是一个修改，甚至创建一个新文件，也算一个修改。

为什么说Git管理的是修改，而不是文件呢？我们还是做实验。第一步，对readme.txt做一个修改，比如在末尾加一行内容：

```
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes. //这是添加的一行
```

然后，git add添加，再git status查看：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190516221247.png)

然后，再修改readme.txt：

```
$ cat readme.txt 
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.  //在Git tracks changes后加了of files
```

提交git commit：(其实这一步已经漏了一步操作，不知道各位看出来没有，我们接着往后看，就当是个伏笔)

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190516221611.png)

提交后，再看看状态：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190516221752.png)

咦，怎么第二次的修改没有被提交？

别激动，我们回顾一下操作过程：

第一次修改 -> `git add` -> 第二次修改 -> `git commit`

你看，我们前面讲了，Git管理的是修改，当你用`git add`命令后，在工作区的第一次修改被放入暂存区，准备提交，但是，在工作区的第二次修改并没有放入暂存区，所以，`git commit`只负责把暂存区的修改提交了，也就是第一次的修改被提交了，第二次的修改不会被提交。

提交后，用`git diff HEAD -- readme.txt`命令可以查看工作区和版本库里面最新版本的区别：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190516222005.png)

可见，第二次修改确实没有被提交

那怎么提交第二次修改呢？你可以继续`git add`再`git commit`，也可以别着急提交第一次修改，先`git add`第二次修改，再`git commit`，就相当于把两次修改合并后一块提交了：

第一次修改 -> `git add` -> 第二次修改 -> `git add` -> `git commit`

好，现在，把第二次修改提交了，然后开始小结。

**总结：**每次修改，如果不用`git add`到暂存区，那就不会加入到`commit`中。



##  三、撤销修改

现在是凌晨两点，你正在赶一份工作报告，头脑不是很清晰，假设我们对工作区的readme.txt中的内容做了修改，比如在该文件末尾加了一行“My stupid boss still prefers SVN.”

```
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
My stupid boss still prefers SVN.
```

在你准备提交前，你突然醒悟过来，你猛然发现了添加的内容里面含有骂老板的话“`stupid boss`”可能会让你丢掉这个月的奖金！

既然错误发现得很及时，就可以很容易地纠正它。下面有两种方案

**第一种方法：**你可以手动打开该文件删掉最后一行，手动把文件恢复到上一个版本的状态。（此方案会不会有弊端？？？）

**第二种方法：**如果用`git status`查看一下：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190517095314.png)

git status`命令可以让我们时刻掌握仓库当前的状态，上面的命令输出告诉我们，`readme.txt`被修改过了，但还没有提交该修改结果。

从上图中可以发现，`git checkout -- file`可以丢弃工作区的修改：

```
git checkout -- readme.txt
```

命令`git checkout -- readme.txt`意思就是，把`readme.txt`文件在工作区的修改全部撤销，这里有两种情况：

一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。

现在，看看`readme.txt`的文件内容：

```
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
```

文件内容果然复原了。

`git checkout -- file`命令中的`--`很重要，没有`--`，就变成了“切换到另一个分支”的命令，我们在后面的分支管理中会再次遇到`git checkout`命令。



现在假定是凌晨3点，你不但写了一些胡话，还`git add`到暂存区了：（**既修改了工作区，又把工作区的该文件添加到了暂存区**）

```
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
My stupid boss still prefers SVN.

$ git add readme.txt  //添加到暂存区
```

庆幸的是，在`commit`之前，你发现了这个问题。用`git status`查看一下，修改只是添加到了暂存区，还没有提交：

```
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)  //unstage的英文意思是“撤销”

	modified:   readme.txt
```

从上面的git显示的信息中，用命令`git reset HEAD <file>`可以把暂存区的修改撤销掉（unstage），重新放回工作区：

```
$ git reset HEAD readme.txt
Unstaged changes after reset:
M	readme.txt
```

`git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用`HEAD`时，表示最新的版本。

（小疑问：这个命令不是跟撤销有关的吗，为什么又跟最新版本扯上联系了？

解答：将你暂存区的代码撤销修改回到远程仓库最新的代码，相当于把你当前的代码已经修改但未提交的代码用远程仓库最新的一次提交来覆盖）

再用`git status`查看一下，现在暂存区是干净的，工作区有修改：

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt
```

还记得如何丢弃工作区的修改吗？

```
$ git checkout -- readme.txt

$ git status
On branch master
nothing to commit, working tree clean
```



现在，假设你不但改错了东西，还从暂存区提交到了版本库，怎么办呢？还记得[版本回退](https://www.liaoxuefeng.com/wiki/896043488029600/897013573512192)一节吗？可以回退到上一个版本。不过，这是有条件的，就是你还没有把自己的本地版本库推送到远程。还记得Git是分布式版本控制系统吗？我们后面会讲到远程版本库，一旦你把`stupid boss`（代表骂老板的话）提交推送到远程版本库，你就真的惨了……

### 小结

又到了小结时间。

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file>`，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考[版本回退](https://www.liaoxuefeng.com/wiki/896043488029600/897013573512192)一节，不过前提是没有推送到远程库。
