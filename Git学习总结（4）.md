#  Git学习总结（4）

## 一、分支管理

分支在实际中有什么用呢？假设你准备开发一个新功能，但是需要两周才能完成，第一周你写了50%的代码，如果立刻提交，由于代码还没写完，不完整的代码库会导致别人不能干活了（因为你没有写完代码就提交到master去了，所以远程的仓库里面就是有bug的）。如果等代码全部写完再一次提交，又存在丢失每天进度的巨大风险。

现在有了分支，就不用怕了。你创建了一个属于你自己的分支，别人看不到，别人还继续在原来的分支上正常工作，而你在自己的分支上干活，想提交就提交，直到开发完毕后，再一次性把自己的分支合并到原来的分支上，这样，既安全，又不影响别人工作。

其他版本控制系统如SVN等都有分支管理，但是用过之后你会发现，这些版本控制系统创建和切换分支比蜗牛还慢，简直让人无法忍受，结果分支功能成了摆设，大家都不去用。

但Git的分支是与众不同的，无论创建、切换和删除分支，Git在1秒钟之内就能完成！无论你的版本库是1个文件还是1万个文件。



##  二、创建与合并分支

在[版本回退](https://www.liaoxuefeng.com/wiki/896043488029600/897013573512192)里，你已经知道，每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在Git里，这个分支叫主分支，即`master`分支。`HEAD`严格来说不是指向提交，而是指向`master`，`master`才是指向提交的，所以，`HEAD`指向的就是当前分支。（**请看下图中HEAD和master的位置关系就知道了**）

一开始的时候，`master`分支是一条线，Git用`master`指向最新的提交，再用`HEAD`指向`master`，（**请看下图中HEAD和master的位置关系就知道了**）就能确定当前分支(**解释：HEAD指向谁，谁就是当前分支，而下图中HEAD指向master**)，以及当前分支的提交点：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190517155806.png)

每次提交，`master`分支都会向前移动一步，这样，随着你不断提交，`master`分支的线也越来越长：（**请看上图中HEAD和master的位置关系就知道了**）

当我们创建新的分支，例如`dev`时，Git新建了一个指针叫`dev`，指向`master`相同的提交，再把`HEAD`指向`dev`，就表示当前分支在`dev`上（**解释：HEAD指向谁，谁就是当前分支，而下图中HEAD指向dev**）：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190517160112.png)

你看，Git创建一个分支很快，因为除了增加一个`dev`指针，改改`HEAD`的指向，工作区的文件都没有任何变化！

不过，从现在开始，对工作区的修改和提交就是针对`dev`分支了，比如新提交一次后，`dev`指针往前移动一步，而`master`指针不变(**解释：从下图中可以看到dev确实是向前移了一步，而master没有变化**)：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190517160507.png)

假如我们在`dev`上的工作完成了，就可以把`dev`合并到`master`上。Git怎么合并呢？最简单的方法，就是直接把`master`指向`dev`的当前提交，就完成了合并，同时HEAD指针指回master：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190517160616.png)

所以Git合并分支也很快！就改改指针，工作区内容也不变！

合并完分支后，甚至可以删除`dev`分支。删除`dev`分支就是把`dev`指针给删掉，删掉后，我们就剩下了一条`master`分支：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190517160728.png)

真是太神奇了，你看得出来有些提交是通过分支完成的吗？

下面开始实战。

首先，我们创建`dev`分支，然后切换到`dev`分支：

```
$ git checkout -b dev   //git checkout命令加上-b参数表示创建并切换分支
Switched to a new branch 'dev'
```

`git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令：

```
$ git branch dev
$ git checkout dev
Switched to branch 'dev'
```

然后，用`git branch`命令查看当前分支：   git branch命令会列出所有分支，当前分支前面会标一个`*`号。

```
$ git branch
* dev       //*指向dev，说明当前我们在dev分支上
  master
```

然后，我们就可以在`dev`分支上正常提交，比如对

readme.txt做个修改，加上一行：

```
Creating a new branch is quick.
```

然后提交：

```
$ git add readme.txt  //加入暂存区
$ git commit -m "branch test"  //提交到版本库
[dev b17d20e] branch test
 1 file changed, 1 insertion(+)
```

现在，`dev`分支的工作完成，我们就可以切换回`master`分支：

```
$ git checkout master
Switched to branch 'master'
```

切换回`master`分支后，再查看一个readme.txt文件，刚才添加的内容不见了！因为那个提交是在`dev`分支上，而`master`分支此刻的提交点并没有变：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190517161503.png)

现在，我们把`dev`分支的工作成果合并到`master`分支上：（解释：`git merge`命令用于合并指定分支到当前分支。合并后，再查看readme.txt的内容，就可以看到，和`dev`分支的最新提交是完全一样的。）

```
$ git merge dev    //我们当前在master分支，执行git merge dev命令把dev分支的工作成果合并过来
Updating d46f35e..b17d20e
Fast-forward
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

注意到上面的`Fast-forward`信息，Git告诉我们，这次合并是“快进模式”，也就是直接把`master`指向`dev`的当前提交（解释：也就是直接把master指向的位置调到dev指向的位置，因为没有合并冲突，有冲突的情况在下面会说到的，先不急），所以合并速度非常快

当然，也不是每次合并都能`Fast-forward`，我们后面会讲其他方式的合并。

合并完成后，就可以放心地删除`dev`分支了：

```
$ git branch -d dev    //-d是删除的意思
Deleted branch dev (was b17d20e).
```

删除后，查看`branch`，就只剩下`master`分支了：

```
$ git branch
* master
```

因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在`master`分支上工作效果是一样的，但过程更安全。

### 小结

Git鼓励大量使用分支：

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`

创建+切换分支：`git checkout -b <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`



## 三、解决冲突

人生不如意之事十之八九，合并分支往往也不是一帆风顺的。

准备新的`feature1`分支，继续我们的新分支开发：

```
$ git checkout -b feature1  //创建+切换分支：git checkout -b <name>
Switched to a new branch 'feature1'
```

修改`readme.txt`最后一行，改为：

```
Creating a new branch is quick AND simple.
```

在`feature1`分支上提交： （**只是在本地版本库的feature1分支上改变了，而本地版本库的master分支是没有改变的**）

```
$ git add readme.txt //添加到暂存区

$ git commit -m "AND simple"  //提交版本库
[feature1 14096d0] AND simple
 1 file changed, 1 insertion(+), 1 deletion(-)
```

切换到`master`分支：

```
$ git checkout master   //切换分支：git checkout <name>
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)
```

Git还会自动提示我们当前`master`分支（虽然远程 的可以有master分支，但是本地的版本库也可以有master分支）比远程的`master`分支要超前1个提交。

在本地版本库的`master`分支上把`readme.txt`文件的最后一行改为：

```
Creating a new branch is quick & simple.
```

然后执行下面的代码提交（**只改变本地版本库的master分支**）

```
$ git add readme.txt 
$ git commit -m "& simple"
[master 5dc6824] & simple
 1 file changed, 1 insertion(+), 1 deletion(-)
```

现在，`master`分支和`feature1`分支各自都分别有新的提交，变成了这样：（**feature1和master分支都各进了一步，如下图**）



![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190517163329.png)

这种情况下，Git无法执行“快速合并”，只能试图把各自的修改合并起来，但这种合并就可能会有冲突，我们试试看：

```
$ git merge feature1
Auto-merging readme.txt
CONFLICT (content): Merge conflict in readme.txt
Automatic merge failed; fix conflicts and then commit the result.
```

果然冲突了！Git告诉我们，`readme.txt`文件存在冲突，必须手动解决冲突后再提交。`git status`也可以告诉我们冲突的文件：

```
$ git status
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

	both modified:   readme.txt      //这就是冲突的文件

no changes added to commit (use "git add" and/or "git commit -a")
```

我们可以直接查看readme.txt的内容：

```
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files. //以上四行是原文件
<<<<<<< HEAD
Creating a new branch is quick & simple.
=======                                  //Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容
Creating a new branch is quick AND simple.
>>>>>>> feature1
```

Git用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容，我们修改如下后保存：

```
Creating a new branch is quick and simple.
```

再提交：

```
$ git add readme.txt    //添加到暂存区
$ git commit -m "conflict fixed"  //提交到版本库
[master cf810e4] conflict fixed
```

现在，`master`分支和`feature1`分支变成了下图所示：（**解释：**因为我们当前在本地版本库的master分支上，可以看到我们把当前分支上的内容提交到了版本库中，所以master在本地版本库中又进了一步）

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190517164843.png)

用带参数的`git log`也可以看到分支的合并情况：

```
$ git log --graph --pretty=oneline --abbrev-commit
*   cf810e4 (HEAD -> master) conflict fixed
|\  
| * 14096d0 (feature1) AND simple
* | 5dc6824 & simple
|/  
* b17d20e branch test
* d46f35e (origin/master) remove test.txt
* b84166e add test.txt
* 519219b git tracks changes
* e43a48b understand how stage works
* 1094adb append GPL
* e475afc add distributed
* eaadf4e wrote a readme file
```

最后，删除`feature1`分支：

```
$ git branch -d feature1
Deleted branch feature1 (was 14096d0).
```

工作完成。

### 小结

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

用`git log --graph`命令可以看到分支合并图。



##  四、分支管理策略

通常，合并分支时，如果可能，Git会用`Fast forward`模式（解释：就是快速合并的模式），但这种模式下，删除分支后，会丢掉分支信息。

如果在强制禁用`Fast forward`模式之后，再去执行合并操作的话，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

下面我们实战一下`--no-ff`方式的`git merge`：  请注意`--no-ff`参数，表示禁用`Fast forward`

首先，仍然创建并切换`dev`分支：

```
$ git checkout -b dev  //创建dev分支，并切换成dev分支
Switched to a new branch 'dev'
```

修改readme.txt文件，并提交一个新的commit：

```
$ git add readme.txt  //添加到暂存区
$ git commit -m "add merge"  //提交到版本库
[dev f52c633] add merge
 1 file changed, 1 insertion(+)
```

现在，我们切换回`master`：

```
$ git checkout master  
Switched to branch 'master'
```

准备合并`dev`分支，请注意`--no-ff`参数，表示禁用`Fast forward`：

```
$ git merge --no-ff -m "merge with no-ff" dev     //请看下面的解释
Merge made by the 'recursive' strategy.
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

因为本次合并要创建一个新的commit（**原理：如果在强制禁用`Fast forward`模式（快速合并）之后，再去执行合并操作的话，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。**），所以加上`-m`参数，把commit描述写进去。  

合并后，我们用`git log`看看分支历史：

```
$ git log --graph --pretty=oneline --abbrev-commit
*   e1e9c68 (HEAD -> master) merge with no-ff
|\  
| * f52c633 (dev) add merge
|/  
*   cf810e4 conflict fixed
```

可以看到，不使用`Fast forward`模式，merge后就像这样：（**原理：如果在强制禁用`Fast forward`模式（快速合并）之后，再去执行合并操作的话，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。**）

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190517171108.png)

### 分支策略

在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；

你和你的小伙伴们每个人都有自己的分支，时不时地往`dev`分支上合并就可以了。（解释：所有人都在自己本地分支上干活，然后时不时的往公共的dev分支上提交修改（如下图所示），下图中的bob和michael都把自己的分支内容往公共dev分支上提交即可（**公共dev分支是不稳定的**），最后dev再往最终master分支上提交）

所以，团队合作的分支看起来就像这样：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190517171522.png)

### 小结

Git分支十分强大，在团队开发中应该充分应用。

合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并