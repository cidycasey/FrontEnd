# Git&GitHub

## 1.简介

Git是目前世界上最先进的分布式版本控制系统（没有之一）。

**版本控制系统**

![example](E:\MarkDown\FrontEnd\Git&Github\images\example.png)

### Git 的诞生

Linus花了两周时间自己用C写了一个分布式版本控制系统，这就是Git！一个月之内，Linux系统的源码已经由Git管理了！牛是怎么定义的呢？大家可以体会一下。

Git迅速成为最流行的分布式版本控制系统，尤其是2008年，GitHub网站上线了，它为开源项目免费提供Git存储，无数开源项目开始迁移至GitHub，包括jQuery，PHP，Ruby等等。

### 集中式VS分布式

Linus一直痛恨的CVS及SVN都是集中式的版本控制系统，而Git是分布式版本控制系统，集中式和分布式版本控制系统有什么区别呢？

- 先说集中式版本控制系统，版本库是集中存放在中央服务器的，而干活的时候，用的都是自己的电脑，所以要先从中央服务器取得最新的版本，然后开始干活，干完活了，再把自己的活推送给中央服务器。中央服务器就好比是一个图书馆，你要改一本书，必须先从图书馆借出来，然后回到家自己改，改完了，再放回图书馆。
- 集中式版本控制系统最大的毛病就是必须联网才能工作

<img src="https://www.liaoxuefeng.com/files/attachments/918921540355872/0" alt="img" style="zoom: 67%;" />

- 分布式版本控制系统根本没有“中央服务器”，每个人的电脑上都是一个完整的版本库，这样，你工作的时候，就不需要联网了，因为版本库就在你自己的电脑上。既然每个人电脑上都有一个完整的版本库，那多个人如何协作呢？比方说你在自己电脑上改了文件A，你的同事也在他的电脑上改了文件A，这时，你们俩之间只需把各自的修改推送给对方，就可以互相看到对方的修改了。

- 分布式版本控制系统的安全性要高很多，因为每个人电脑里都有完整的版本库，某一个人的电脑坏掉了不要紧，随便从其他人那里复制一个就可以了

- 在实际使用分布式版本控制系统的时候，其实很少在两人之间的电脑上推送版本库的修改，因为可能你们俩不在一个局域网内，两台电脑互相访问不了，也可能今天你的同事病了，他的电脑压根没有开机。

  因此，分布式版本控制系统通常也有一台充当“中央服务器”的电脑，但这个服务器的作用仅仅是用来方便“交换”大家的修改，没有它大家也一样干活，只是交换修改不方便而已。

<img src="https://www.liaoxuefeng.com/files/attachments/918921562236160/0" alt="img" style="zoom:67%;" />

​	CVS作为最早的开源而且免费的集中式版本控制系统，直到现在还有不少人在用。由于CVS自身设计的问题，会造成提交文件不完整，版本库莫名其妙损坏的情况。同样是开源而且免费的SVN修正了CVS的一些稳定性问题，是目前用得最多的集中式版本库控制系统。

## 2.安装

- 下载连接：https://github.com/waylau/git-for-win
- Git下载安装及设置详细教程：https://www.jianshu.com/p/a152f82c5e4a
- bin目录添加到环境变量

![1569983963895](E:\MarkDown\FrontEnd\Git&Github\images\1569983963895.png)

或：设置，在命令行输入：

```c
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

注意`git config`命令的`--global`参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。

因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和Email地址。你也许会担心，如果有人故意冒充别人怎么办？这个不必担心，首先我们相信大家都是善良无知的群众，其次，真的有冒充的也是有办法可查的。

### 创建版本库

**版本库又名仓库**，英文名**repository**，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

1. E:/(根目录) 右键 > Git Bash Here

   <img src="E:\MarkDown\FrontEnd\Git&amp;Github\images\1570850856787.png" alt="1570850856787" style="zoom:33%;" />

2. 选择一个合适的地方，创建一个空目录：

   ```c
   $ mkdir learngit
   $ cd learngit
   $ pwd
   ```

   ![1570850741132](E:\MarkDown\FrontEnd\Git&Github\images\1570850741132.png)

3. 通过`git init`命令把这个目录变成Git可以管理的仓库：

   ```c
   $ git init
   $ ls -ah
   ```

   当前目录下多了一个`.git`的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。

   如果你没有看到`.git`目录，那是因为这个目录默认是隐藏的，用`ls -ah`命令就可以看见。

   ![1570851191639](E:\MarkDown\FrontEnd\Git&Github\images\1570851191639.png)

## 3.版本管理（回退、撤销修改、删除文件）

### 把文件添加到版本库

- 首先这里再明确一下，所有的版本控制系统，其实只能跟踪文本文件的改动，比如TXT文件，网页，所有的程序代码等等，Git也不例外。

- 版本控制系统可以告诉你每次的改动，比如在第5行加了一个单词“Linux”，在第8行删了一个单词“Windows”。而图片、视频这些二进制文件，虽然也能由版本控制系统管理，但没法跟踪文件的变化，只能把二进制文件每次改动串起来，也就是只知道图片从100KB改成了120KB，但到底改了啥，版本控制系统不知道。Microsoft的Word格式是二进制格式。

- 因为文本是有编码的，比如中文有常用的GBK编码，日文有Shift_JIS编码，如果没有历史遗留问题，强烈建议使用标准的UTF-8编码，所有语言使用同一种编码，既没有冲突，又被所有平台所支持。

- **使用Windows的童鞋特别注意：**

  千万不要使用Windows自带的**记事本**编辑任何文本文件。原因是Microsoft开发记事本的团队使用了一个非常弱智的行为来保存UTF-8编码的文件，他们自作聪明地在每个文件开头添加了0xefbbbf（十六进制）的字符，你会遇到很多不可思议的问题，比如，网页第一行可能会显示一个“?”，明明正确的程序一编译就报语法错误，等等，都是由记事本的弱智行为带来的。

- 建议下载[Notepad++](http://notepad-plus-plus.org/)代替记事本，不但功能强大，而且免费！记得把Notepad++的默认编码设置为UTF-8 without BOM即可：

  修改：设置>首选项>

  ![1570852056619](E:\MarkDown\FrontEnd\Git&Github\images\1570852056619.png)

### 具体步骤

第一步，用命令`git add`告诉Git，把文件添加到仓库：

```
$ git add readme.txt
```

执行上面的命令，没有任何显示，这就对了，Unix的哲学是“没有消息就是好消息”，说明添加成功。

第二步，用命令`git commit`告诉Git，把文件提交到仓库：

```
$ git commit -m "wrote a readme file"
[master (root-commit) eaadf4e] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
```

简单解释一下`git commit`命令，`-m`后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

`git commit`命令执行成功后会告诉你，`1 file changed`：1个文件被改动（我们新添加的readme.txt文件）；`2 insertions`：插入了两行内容（readme.txt有两行内容）。

为什么Git添加文件需要`add`，`commit`一共两步呢？因为`commit`可以一次提交很多文件，所以你可以多次`add`不同的文件，比如：

```
$ git add file1.txt
$ git add file2.txt file3.txt
$ git commit -m "add 3 files."
```

```c
$ git status
```

![1570860089184](E:\MarkDown\FrontEnd\Git&Github\images\1570860089184.png)

```
$ git diff
```

用`git diff HEAD -- readme.txt`命令可以查看**工作区**和**版本库**里面最新版本的区别：

![1570860485170](E:\MarkDown\FrontEnd\Git&Github\images\1570860485170.png)

```c
$ git status
On branch master
nothing to commit, working tree clean
```

Git告诉我们当前没有需要提交的修改，而且，工作目录是干净（working tree clean）的。

- 要随时掌握工作区的状态，使用`git status`命令。
- 如果`git status`告诉你有文件被修改过，用`git diff`可以查看修改内容。

------

Git也是一样，每当你觉得文件修改到一定程度的时候，就可以“保存一个快照”，这个快照在Git中被称为`commit`。一旦你把文件改乱了，或者误删了文件，还可以从最近的一个`commit`恢复，然后继续工作，而不是把几个月的工作成果全部丢失。

版本控制系统肯定有某个命令可以告诉我们历史记录，在Git中，我们用`git log`命令查看：

### 版本回退

准备把`readme.txt`回退到上一个版本，也就是`add distributed`的那个版本

首先：Git必须知道当前版本是哪个版本，在Git中，用`HEAD`表示当前版本，也就是最新的提交`1094adb...`（注意我的提交ID和你的肯定不一样），上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`。

我们要把当前版本`append GPL`回退到上一个版本`add distributed`，就可以使用`git reset`命令

------

最新的那个版本`append GPL`已经看不到了！好比你从21世纪坐时光穿梭机来到了19世纪，想再回去已经回不去了，肿么办？

办法其实还是有的，只要上面的命令行窗口还没有被关掉，你就可以顺着往上找啊找啊，找到那个`append GPL`的`commit id`是`1094adb...`，于是就可以指定回到未来的某个版本。版本号没必要写全，前几位就可以了，Git会自动去找。当然也不能只写前一两位，因为Git可能会找到多个版本号，就无法确定是哪一个了。

在Git中，总是有后悔药可以吃的。当你用`$ git reset --hard HEAD^`回退到`add distributed`版本时，再想恢复到`append GPL`，就必须找到`append GPL`的commit id。Git提供了一个命令`git reflog`用来记录你的每一次命令。

------

总结：

- `HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。

- 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。

  ![1570862854764](E:\MarkDown\FrontEnd\Git&Github\images\1570862854764.png)

- 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。

  ![1570862946982](E:\MarkDown\FrontEnd\Git&Github\images\1570862946982.png)

### 工作区和暂存区

#### 工作区（Working Directory）

就是你在电脑里能看到的目录，比如我的`learngit`文件夹就是一个工作区。

#### 版本库（Repository）

工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。



前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：

第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，现在，`git commit`就是往`master`分支上提交更改。

先用`git status`查看一下状态：

```c
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

 Untracked files:
  (use "git add <file>..." to include in what will be committed)

	LICENSE

no changes added to commit (use "git add" and/or "git commit -a")
```

Git非常清楚地告诉我们，`readme.txt`被修改了，而`LICENSE`还从来没有被添加过，所以它的状态是`Untracked`。

### 管理修改

为什么Git比其他版本控制系统设计得优秀，因为Git跟踪并管理的是**修改**，而非文件。

### 撤销修改

命令`git checkout -- readme.txt`意思就是，把`readme.txt`文件在工作区的修改全部撤销，这里有两种情况：

一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和**版本库**一模一样的状态.``git commit`

一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到**添加到暂存区（stage）**后的状态。`git add`

总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。

![img](E:\MarkDown\FrontEnd\Git&Github_Note_LiaoXuefeng_images\1570863605073.jpg)

`git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用`HEAD`时，表示最新的版本。

**小结：**

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。（没有add 到 stage 暂存区）

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file>`，就回到了场景1，第二步按场景1操作。（add 到了 stage 暂存区，但是没有 commit 到 master 分支）

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考[版本回退](https://www.liaoxuefeng.com/wiki/896043488029600/897013573512192)一节，不过前提是没有推送到远程库。

### 删除文件

在Git中，删除也是一个修改操作。`git checkout -- test.`，其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

- 你通常直接在文件管理器中把没用的文件删了，或者用`rm`命令删了（文件工作区）；
- 这个时候，Git知道你删除了文件，因此，工作区和版本库就不一致了，`git status`命令会立刻告诉你哪些文件被删除了；
- 现在你有两个选择，一是**确实要从版本库中删除该文件**，那就用命令`git rm`删掉，并且`git commit`：

```c
$ git rm test.txt
rm 'test.txt'

$ git commit -m "remove test.txt"
[master d46f35e] remove test.txt
 1 file changed, 1 deletion(-)
 delete mode 100644 test.txt
```

- 另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本。

  ```c
  $ git checkout -- test.txt
  ```

  从来没有被添加到版本库就被删除的文件，是无法恢复的！

## 4.远程仓库

Git是分布式版本控制系统，同一个Git仓库，可以分布到不同的机器上。怎么分布呢？最早，肯定只有一台机器有一个原始版本库，此后，别的机器可以“克隆”这个原始版本库，而且每台机器的版本库其实都是一样的，并没有主次之分。

好在这个世界上有个叫[GitHub](https://github.com/)的神奇的网站，从名字就可以看出，这个网站就是提供Git仓库托管服务的，所以，只要注册一个GitHub账号，就可以免费获得Git远程仓库。

由于你的本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以，需要一点设置：

1. 创建SSH Key。在用户主目录`C:\Users\CIDY CASEY\`下，看看有没有.ssh目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key。
2. 你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。

 3. 登陆GitHub，打开“Account settings”，“SSH Keys”页面。然后，点“Add SSH Key”，填上任意Title【test】，在Key文本框里粘贴`id_rsa.pub`文件的内容。


为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。

当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。

最后友情提示，在GitHub上免费托管的Git仓库，任何人都可以看到喔（但只有你自己才能改）

### 添加远程仓库

```c
$ git remote add origin git@github.com:/learngit.git
$ git push -u origin master
```

把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。

由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

<img src="E:\MarkDown\FrontEnd\Git&amp;Github_Note_LiaoXuefeng_images\1570869991054.png" alt="1570869991054" style="zoom:50%;" />

<img src="E:\MarkDown\FrontEnd\Git&amp;Github_Note_LiaoXuefeng_images\1570870230439.png" alt="1570870230439" style="zoom:50%;" />

从现在起，只要本地作了提交，就可以通过命令：

```
$ git push origin master
```

### 从远程库克隆

```c
$ git clone git@github.com:cidycasey/gitskills.git
```

## 5.分支管理

分支在实际中有什么用呢？假设你准备开发一个新功能，但是需要两周才能完成，第一周你写了50%的代码，如果立刻提交，由于代码还没写完，不完整的代码库会导致别人不能干活了。如果等代码全部写完再一次提交，又存在丢失每天进度的巨大风险。

现在有了分支，就不用怕了。你创建了一个属于你自己的分支，别人看不到，还继续在原来的分支上正常工作，而你在自己的分支上干活，想提交就提交，直到开发完毕后，再一次性合并到原来的分支上，这样，既安全，又不影响别人工作。

## 命令汇总：

```
// 创建目录
$ mkdir learngit
$ cd learngit
$ pwd
// 把这个目录变成Git可以管理的仓库
$ git init
$ ls -ah
// 添加、提交文件
$ git add readme.txt   //git add <file>
$ git commit -m "wrote a readme.txt file"   //git commit -m <message>
// 查看状态
// 命令可以让我们时刻掌握仓库当前的状态，上面的命令输出告诉我们，readme.txt被修改过了，但还没有准备提交的修改。
$ git status
// 看看具体修改了什么内容(different);用git diff HEAD -- readme.txt命令可以查看工作区和版本库里面最新版本的区别。
$ git diff
// 版本控制系统肯定有某个命令可以告诉我们历史记录，在Git中，我们用git log命令查看：
$ git log
// 如果嫌输出信息太多，看得眼花缭乱的，可以试试加上--pretty=oneline参数：
$ git log --pretty=oneline
// 回退到上一个版本,命令既可以回退版本，也可以把暂存区的修改回退到工作区,当我们用HEAD时，表示最新的版本。
$ git reset --hard HEAD^
// 回退到未来的版本
$ git reset --hard 1094a
// 查看文件内容，看看readme.txt的内容
$ cat readme.txt
// 查看命令历史，以便确定要回到未来的哪个版本
$ git reflog
// 其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”
git checkout -- readme.txt
```

