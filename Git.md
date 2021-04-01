# Git

所有的版本控制系统，其实只能跟踪文本文件的改动，比如TXT文件，网页，所有的程序代码等等，Git也不例外。版本控制系统可以告诉你每次的改动，比如在第5行加了一个单词“Linux”，在第8行删了一个单词“Windows”。而图片、视频这些二进制文件，虽然也能由版本控制系统管理，但没法跟踪文件的变化，只能把二进制文件每次改动串起来，也就是只知道图片从100KB改成了120KB，但到底改了啥，版本控制系统不知道，也没法知道。

![img](https://www.runoob.com/wp-content/uploads/2015/02/1352126739_7909.jpg)

![img](https://www.runoob.com/wp-content/uploads/2015/02/git-command.jpg)

**工作区：**就是你在电脑里能看到的目录。

**暂存区：**英文叫 stage 或 index。一般存放在 **.git** 目录下的 index 文件（.git/index）中，所以我们把暂存区有时也叫作索引（index）。

**版本库：**工作区有一个隐藏目录 **.git**，这个不算工作区，而是 Git 的版本库。commit的目标地。

- 当对工作区修改（或新增）的文件执行 **git add** 命令时，暂存区的目录树被更新，同时工作区修改（或新增）的文件内容被写入到对象库中的一个新的对象中，而该对象的ID被记录在暂存区的文件索引中。
- 当执行提交操作（**git commit**）时，暂存区的目录树写到版本库（对象库）中，master 分支会做相应的更新。即 master 指向的目录树就是提交时暂存区的目录树。
- 当执行 **git reset HEAD** 命令时，暂存区的目录树会被重写，被 master 分支指向的目录树所替换，但是工作区不受影响。
- 当执行 **git rm --cached <file>** 命令时，会直接从暂存区删除文件，工作区则不做出改变。
- 当执行 **git checkout .** 或者 **git checkout -- <file>** 命令时，会用暂存区全部或指定的文件替换工作区的文件。这个操作很危险，会清除工作区中未添加到暂存区的改动。
- 当执行 **git checkout HEAD .** 或者 **git checkout HEAD <file>** 命令时，会用 HEAD 指向的 master 分支中的全部或者部分文件替换暂存区和以及工作区中的文件。这个命令也是极具危险性的，因为不但会清除工作区中未提交的改动，也会清除暂存区中未提交的改动。

| `git add`    | 添加文件到仓库                           |
| ------------ | ---------------------------------------- |
| `git status` | 查看仓库当前的状态，显示有变更的文件。   |
| `git diff`   | 比较文件的不同，即暂存区和工作区的差异。 |
| `git commit` | 提交暂存区到本地仓库。                   |
| `git reset`  | 回退版本。                               |
| `git rm`     | 删除工作区文件。                         |
| `git mv`     | 移动或重命名工作区文件。                 |

| `git log`          | 查看历史提交记录                     |
| ------------------ | ------------------------------------ |
| `git blame <file>` | 以列表形式查看指定文件的历史修改记录 |

| `git remote` | 远程仓库操作       |
| ------------ | ------------------ |
| `git fetch`  | 从远程获取代码库   |
| `git pull`   | 下载远程代码并合并 |
| `git push`   | 上传远程代码并合并 |

++++++++++++

## level 1

### 本地

```text
git init        初始化
```

#### git config

Git 提供了一个叫做 git config 的工具，专门用来配置或读取相应的工作环境变量。这些环境变量，决定了 Git 在各个环节的具体工作方式和行为。这些变量可以存放在以下三个不同的地方：

- **`/etc/gitconfig`** 文件：系统中对所有用户都普遍适用的配置。若使用 `git config` 时用 `--system` 选项，读写的就是这个文件。

- **`~/.git/config`** 文件：用户目录下的配置文件只适用于该用户。若使用 `git config` 时用 `--global` 选项，读写的就是这个文件。

- 当前项目的 Git 目录中的配置文件（也就是工作目录中的 **`.git/config`** 文件）：这里的配置仅仅针对当前项目有效。

  每一个级别的配置都会覆盖上层的相同配置，所以 `.git/config` 里的配置会覆盖 `/etc/gitconfig` 中的同名变量。

```text
git config -e    # 针对当前仓库 
git config -e --global   # 针对系统上所有仓库
git config --list
git config --global user.name "github_name"
git config --global user.email "github_email"
git config --global core.ignorecase false    #设置大小写敏感
git config --global core.autocrlf false      #关闭自动换行
git config --global core.safecrlf true       #保证文件的换行符是安全的方法，避免windows与unix换行符混用
```

#### git add

```
git add . ：将文件的修改，文件的新建，添加到暂存区。 无删除,默认
git add -u：将文件的修改、文件的删除，添加到暂存区。 无新建
git add -A：将文件的修改，文件的删除，文件的新建，添加到暂存区。
```

#### git rm

```text
git rm file1              删除文件跟踪并且删除文件系统中的文件file1
git rm --cached file1     删除文件跟踪但不删除文件系统中的文件
```

#### git commit

git commit 主要是将暂存区里的改动给提交到本地的版本库。每次使用git commit 命令我们都会在本地版本库生成一个40位的哈希值，这个哈希值也叫commit-id，commit-id 在版本回退的时候是非常有用的，它相当于一个快照,可以在未来的任何时候通过与git reset的组合命令回到这里.

```
git commit -am ‘message’ -am等同于-a -m
-m 参数表示可以直接输入后面的“message”，如果不加 -m参数，那么是不能直接输入message的，而是会调用一个编辑器一般是vim来让你输入这个message，
-a参数可以将所有已跟踪文件中的执行修改或删除操作的文件都提交到本地仓库，即使它们没有经过git add添加到暂存区，
*注意:* 新加的文件（即没有被git系统管理的文件）是不能被提交到本地仓库的。
```

#### git tag

```
git tag  # 列出当前仓库的所有标签
git tag <tagname>     #用于新建一个标签，默认为`HEAD`，也可以指定一个commit id；
git tag -l 'v0.1.*'  # 搜索符合当前模式的标签
git tag v0.2.1-light  # 创建轻量标签
git tag -a v0.2.1 -m '0.2.1版本'  # 创建附注标签
git checkout [tagname]  # 切换到标签
git show v0.2.1  # 查看标签版本信息
git tag -d v0.2.1  # 删除标签
git tag -a v0.2.1 9fbc3d0  # 补打标签
```

#### 分支

````
git branch									查看本地所有分支
git branch 									-r查看远程所有分支
git branch 									-a查看本地和远程所有分支
git branch <name>                                创建分支
git checkout <name>或者git switch <name>          切换分支
git checkout -b <name>或者git switch -c <name>    创建+切换分支
git checkout -B         
git merge <name>                                 合并某分支到当前分支
git branch -d <name>														 删除分支
git branch -D <name>                             要丢弃一个没有被合并过的分支，强行删除
git push origin <local_branch_name>:<remote_branch_name>    推送分支
git branch -d -r <branchname>删除远程分支，其中<branchname>为本地分支名

删除后，还要推送到服务器上才行，即git push origin :<branchname>

作者：C1R2
链接：https://www.jianshu.com/p/aea408814ebe
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
重命名分支 git branch (-m | -M) <oldbranch> <newbranch>
git push origin :<remote_branch_name>						 删除远端分支，其实就是推送了一个空的分支到远端覆盖了原来的远端分支
git checkout -b <local_branch_name> origin/<remote_branch_name> 或
git branch —track <local_branch_name> origin/<remote_branch_name>				
git branch —set-upstream <local_branch_name> origin/<remote_branch_name>    本地已经存在的分支和远端分支建立对应关系

````

#### git log

```text
git show 4ebd4bbc3ed321      查看某一历史版本的提交内容，这里能看到版本的详细修改代码。
git diff c0f28a2ec490 2e476412c34     对比不同版本
git log -p 每一次提交具体差异
git log —stat 显示文件修改差异，没显示具体修改
git log —graph 树形状提交记录，可查看分支合并信息
```



### 远程

#### ssh key

ssh-keygen -t rsa -C "saligia.eden@icloud.com",生成秘钥文件，回车留空，将生成的~/.ssh/中的id_rsa.pub中的秘钥copy到github后台的ssh添加中。

#### git clone

git clone只能clone远程库的master分支,无法clone所有分支,`git clone` 默认是克隆`Head`指向的`master`分支，如果是多分支，我们可以单个克隆分支项目。

1.只克隆单分支（非master）： git clone -b 分支名 https://xxx.git

2.克隆所有分支（多分支）

```
git branch -a
git checkout -b template origin/template   // 作用是checkout远程仓库origin的分支template，在本地起名为template分支，并切换到本地的template分支 
```

#### git remote 

```
git remote -v
git remote add   <name>  link
git remote remove
git remote set-url origin git@192.168.6.70:res_dev_group/test.git 

```



#### git pull

**git pull** 其实就是 **git fetch** 和 **git merge FETCH_HEAD** 的简写。 命令格式如下：

```
$ git pull
$ git pull origin
将远程主机 origin 的 master 分支拉取过来，与本地的 brantest 分支合并。

git pull origin master:brantest
如果远程分支是与当前分支合并，则冒号后面的部分可以省略。

git pull origin master
git pull <远程主机名> <远程分支名>:<本地分支名>
git fetch --all								 所有分支
git pull  --all								 所有分支
```

#### git fetch

假如你想丢弃你在本地的所有改动与提交，可以到服务器上获取最新的版本历史，并将你本地主分支指向它：
`git fetch origin`
`git reset --hard origin/master`

强制远端覆盖本地

`git fetch --all`
`git reset --hard origin/<remote_branch_name>`		提交日志查看方式



#### git push

```
git push <远程主机名> <本地分支名>:<远程分支名>
如果本地分支名与远程分支名相同，则可以省略冒号：
git push <远程主机名> <本地分支名>
如果本地版本与远程版本有差异，但又要强制推送可以使用 --force 参数：
git push --force origin master
git push REMOTE '*:*'          推送所有分支
git push REMOTE --all					 推送所有分支
git push --all origin					 推送所有分支
git push origin master         如果该远程分支不存在，则会被新建
git push -u origin master      加了参数-u后，以后即可直接用git push 代替git push origin master
git push origin ：refs/for/master    如果省略本地分支名，则表示删除指定的远程分支，等同于 git push origin –delete master
git push origin    如果当前分支与远程分支存在追踪关系，则本地分支和远程分支都可以省略，将当前分支推送到origin主机的对应分支
git push           如果当前分支只有一个远程分支，那么主机名都可以省略，形如 git push，可以使用git branch -r ，查看远程的分支名


git push origin v0.1.2  # 将v0.1.2标签提交到git服务器
git push origin --tags  # 将本地所有标签一次性提交到git服务器
git push origin :refs/tags/<tagname>    #可以删除一个远程标签。
```

在git push后出现错误可能是因为其他人提交了代码，而使你的本地代码库版本不是最新。这时你需要先git pull代码后，检查是否有文件冲突。没有文件冲突的话需要重新走一遍代码提交流程add —> commit —> push。

关于 refs/for：
 refs/for 的意义在于我们提交代码到服务器之后是需要经过code review 之后才能进行merge的，而refs/heads 不需要



++++++++++++++

## level 2



章目录
前言
代码回退
Git管理下的各种文件状态
Git回退命令
git checkout
git revert
git restore
git reset
git rm
具体回退操作
初始状态
还原00：工作区中未加到暂存区和版本库的文件，还原今天所做的修改
还原01：工作区中未加到暂存区和版本库的文件，执行了 `git add` 操作
git rm
git restore
git reset
还原02：版本库中的文件，修改或删除后未执行 `git add` 操作
git restore
git checkout
git reset
还原03：版本库中的文件，修改或删除后执行了 `git add` 操作
git restore
git reset
还原04：版本库中的文件，修改或删除后执行了 `git add`、`git commit` 操作
还原05：版本库中的文件，修改或删除后执行了 `git add`、`git commit`、`git push` 操作
还原06：两次`git commit` 之后产生两条日志，只还原第一次提交
常用集合
总结
在 Git 中没有真正的方法来做任何事情，这就是它的妙处！

前言
经常会听到别人说，如果时光可以倒流，我将会如何如何，可是现阶段的科技还达不到时光倒流的目的，或许在《三体》世界的四维裂缝里可以试一下。现实的世界中找不到后悔药，但是在代码的世界里却可以轻松实现，错误的BUG修改、砍掉的做了一半的功能都可以轻松回退，不留一丝痕迹，回滚之后一切又可以重新开始了。

代码回退
大型编程项目的开发往往伴随着版本工具的使用，其实引入代码版本控制工具，有一部分原因也是为了方便回退，回退操作每天都发生的，只是有时是我们显式的操作，有时却自然而然的进行着，我们切换着分支很可能就是从开发版本回退到一个稳定版本，我们查询日志，实际上是在记忆上回退我们整个的开发过程，找寻其中的问题和修改的内容。

Git管理下的各种文件状态
Git的使用中，由于一个文件存在好几种状态的变化，所以处理起回退要分情况进行，有些各式各样的命令最终分析起来其实作用是一样的。

说起Git常常会提到工作区、暂存区、版本库的概念，这是很通用的说法，其实工作区一般就是指我们能看到的文件、本地操作文件所在的目录，我们正常编写的代码文件、管理的资源文件都是在工作区里操作，这里的文件也不是全都平等的，又细分为受版本控制的文件和不受版本控制的文件。

提到暂存区就和index文件建立起了联系，工作区的新文件和已经修改的受版本控制的文件，使用 git add file_name 就可以加到暂存区，相当于登记报个名，以后提交到版本库的时候会把这些登记的文件都带上，实际上执行了 git add 命令的文件都生成了对应的 object 对象，放在.git/objects目录下，状态变成了 staged， 当提交到版本库时，分支会引用这些对象。

版本库就是文件修改的目的地了，最终的修改会提交到版本库，这时提交的文件状态变成 committed，其实也是一种 unmodified 状态，一路走来，版本库中记录了你的每一次提交，可以追溯你每一次修改的内容。

其实还有一个远程仓库的概念，一般确定本地仓库的修改没有问题了，或者要将本地代码远程备份时，可以将自己修改的分支推送到远程仓库，因为有时候我们也想回退已经推送到远程仓库的修改，所以这里先提一下远程仓库。

总结起来一个文件的状态通常可以分为：

不受版本控制的 untracked 状态
受版本控制并且已修改的 modified 状态
受版本控制已修改并提交到暂存区的 staged 状态
从暂存区已经提交到本地仓库的 committed 状态
提交到本地仓库未修改或者从远程仓库克隆下来的 unmodified 状态
Git回退命令
上面提到了在 Git 这个版本控制工具下文件的各种状态，其实回退操作就是通过命令实现这些文件状态的“倒退”，进而达到回退操作的目的，下面一起先来了解下这些可以实现回退的命令。

git checkout
这个命令又出现了，上次是总结 git branch 分支操作的时候，git checkout 可以用来新建或者切换分支，这次总结回退版本的命令，git checkout 也可以用来回退文件版本，很神奇吧。

其实这个命令的作用就是它单词的本义——检出，他的常用操作也取自这个意思，比如 git checkout branch_name 切换分支操作，实际上就是把指定分支在仓库中对应的所有文件检出来覆盖当前工作区，最终表现就是切换了分支。

而针对于文件的检出可以使用 git checkout -- file_name，当不指定 commit id 就是将暂存区的内容恢复到工作区，也就可以达到回退本地修改的作用。

不过，这个身兼数职的 git checkout 命令现在可以轻松一些了，从 Git 2.23 版本开始引入了两个新的命令： git switch 用来切换分支，git restore用来还原工作区的文件，这个后面还会提到。

git revert
revert 这个词的意思是：归还，复原，回退，它和后面即将提到的 restore 在意思上简直无法区分，为了区别他们两个这里可以把 git revert 看成归还的意思，对某次提交执行 git revert 命令就是对这次修改执行一个归还操作，其实就是反向再修改一次。

要理解 git revert 就要从反向修改的含义来看，当我们再一个文件中添加一行内容，并提交到版本库后，产生一个提交id——commit-id-a，如果这时使用 git revert commit-id-a 命令，就相当于在工作区中的那个文件将刚在新加的一行内容删除掉，然后再进行一个提交。

注意，这个操作是会改变分支记录的，因为产生了新的提交。

git restore
这个命令是 Git 2.23 版本之后新加的，用来分担之前 git checkout 命令的功能，作用就是用暂存区或者版本库中的文件覆盖本地文件的修改可以达到回退修改的目的，同时也可以使用版本库中的文件覆盖暂存区的文件，达到回退git add 命令的目的。

注意，这个操作是不会影响分支记录的，就是相当于之前的 git checkout 命令重新检出一份文件来覆盖本地的修改。

git reset
reset 重新设置的意思，其实就是用来设置分支的头部指向，当进行了一系列的提交之后，忽然发现最近的几次提交有问题，想从提交记录中删除，这是就会用到 git reset 命令，这个命令后面跟 commit id，表示当前分支回退到这个 commit id 对应的状态，之后的日志记录被删除，工作区中的文件状态根据参数的不同会恢复到不同的状态。

--soft: 被回退的那些版本的修改会被放在暂存区，可以再次提交。

--mixed: 默认选项，被回退的那些版本的修改会放在工作目录，可以先加到暂存区，然后再提交。

--hard: 被回退的那些版本的修改会直接舍弃，好像它们没有来过一样。

这样来看，git set 命令好像是用来回退版本的，但是如果使用 git rest HEAD file_name 命令就可以将一个文件回退到 HEAD 指向版本所对应的状态，其实就是当前版本库中的状态，也就相当于还原了本地的修改。

git rm
临时插播的命令，本来删除不能算是回退，但是如果它和某些命令反着来就是一种回退，比如对一个新文件使用 git add newfile_name 命令，然后再使用 git rm --cached newfile_name 就可以将这个文件从暂存区移除掉，但是在工作区里没有消失，如果不加 --cached 参数，就会从工作区和版本库暂存区同时删除，相当于执行了 rm newfile_name 和 git add new_file 两条命令。

具体回退操作
说了这么多肯定有点懵，特别是一个相同的需求可以使用很多命令来实现的时候，接下来看一些具体需求，整个测试过程用上一篇总结《git branch常用分支操作》使用的 git 仓库来进行，远程地址是 git@gitee.com:myname/gitstart.git，下面测试开始，我们看一下这些情况怎么进行还原：

初始状态
albert@homepc MINGW64 /d/gitstart (dev)
$ ls
README.md

albert@homepc MINGW64 /d/gitstart (dev)
$ git status
On branch dev
Your branch is up to date with 'origin/dev'.

nothing to commit, working tree clean

albert@homepc MINGW64 /d/gitstart (dev)
$ git branch -a
* dev
  master
  remotes/origin/dev
  remotes/origin/master

albert@homepc MINGW64 /d/gitstart (dev)
$ git branch -vv
* dev     3226b63 [origin/dev] add readme file
  master  3226b63 [origin/master] add readme file
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
还原00：工作区中未加到暂存区和版本库的文件，还原今天所做的修改
实话实说，办不到，没有加到过暂存区就没有被追踪，它的任何修改是没有办法回退的，可是使用 Ctrl+Z 碰碰运气，没准就退回到了你想要的状态。

还原01：工作区中未加到暂存区和版本库的文件，执行了 git add 操作
这种情况可以使用git rm --cached newfile、git restore --staged newfile 或者 git reset HEAD newfile 命令，使用后两个命令的时候不能是版本库的第一个文件。

git rm
albert@homepc MINGW64 /d/gitstart (dev)
$ echo "test data">new.txt

albert@homepc MINGW64 /d/gitstart (dev)
$ git add new.txt
warning: LF will be replaced by CRLF in new.txt.
The file will have its original line endings in your working directory

albert@homepc MINGW64 /d/gitstart (dev)
$ git status
On branch dev
Your branch is up to date with 'origin/dev'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   new.txt


albert@homepc MINGW64 /d/gitstart (dev)
$ git rm --cached new.txt
rm 'new.txt'

albert@homepc MINGW64 /d/gitstart (dev)
$ git status
On branch dev
Your branch is up to date with 'origin/dev'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        new.txt

nothing added to commit but untracked files present (use "git add" to track)
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
git restore
albert@homepc MINGW64 /d/gitstart (dev)
$ git add new.txt
warning: LF will be replaced by CRLF in new.txt.
The file will have its original line endings in your working directory

albert@homepc MINGW64 /d/gitstart (dev)
$ git status
On branch dev
Your branch is up to date with 'origin/dev'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   new.txt


albert@homepc MINGW64 /d/gitstart (dev)
$ git restore --staged new.txt

albert@homepc MINGW64 /d/gitstart (dev)
$ git status
On branch dev
Your branch is up to date with 'origin/dev'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        new.txt

nothing added to commit but untracked files present (use "git add" to track)
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
git reset
albert@homepc MINGW64 /d/gitstart (dev)
$ git add new.txt
warning: LF will be replaced by CRLF in new.txt.
The file will have its original line endings in your working directory

albert@homepc MINGW64 /d/gitstart (dev)
$ git status
On branch dev
Your branch is up to date with 'origin/dev'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   new.txt


albert@homepc MINGW64 /d/gitstart (dev)
$ git reset HEAD new.txt

albert@homepc MINGW64 /d/gitstart (dev)
$ git status
On branch dev
Your branch is up to date with 'origin/dev'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        new.txt

nothing added to commit but untracked files present (use "git add" to track)
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
还原02：版本库中的文件，修改或删除后未执行 git add 操作
我们直接修改 README.md 文件吧，删除刚才添加的未受版本管理的 new.txt，在 README.md 文件中添加内容，然后试着还原，这种情况常常出现在修改一个功能还未提交，但是先不要求修改了，可以直接还原。

这种情况可以使用git restore file_name、git checkout -- file_name 或者 git reset --hard HEAD 命令，最后的git reset 命令带有 --hard 参数不能再加文件目录，只能将工作区全还原。

git restore
albert@homepc MINGW64 /d/gitstart (dev)
$ echo "new line">>README.md

albert@homepc MINGW64 /d/gitstart (dev)
$ git status
On branch dev
Your branch is up to date with 'origin/dev'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")

albert@homepc MINGW64 /d/gitstart (dev)
$ git restore README.md

albert@homepc MINGW64 /d/gitstart (dev)
$ git status
On branch dev
Your branch is up to date with 'origin/dev'.

nothing to commit, working tree clean
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
git checkout
albert@homepc MINGW64 /d/gitstart (dev)
$ echo "new line">>README.md

albert@homepc MINGW64 /d/gitstart (dev)
$ git status
On branch dev
Your branch is up to date with 'origin/dev'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")

albert@homepc MINGW64 /d/gitstart (dev)
$ git checkout -- README.md

albert@homepc MINGW64 /d/gitstart (dev)
$ git status
On branch dev
Your branch is up to date with 'origin/dev'.

nothing to commit, working tree clean
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
git reset
albert@homepc MINGW64 /d/gitstart (dev)
$ echo "new line">>README.md

albert@homepc MINGW64 /d/gitstart (dev)
$ git status
On branch dev
Your branch is up to date with 'origin/dev'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")

albert@homepc MINGW64 /d/gitstart (dev)
$ git reset --hard HEAD README.md
fatal: Cannot do hard reset with paths.

albert@homepc MINGW64 /d/gitstart (dev)
$ git reset --hard HEAD
HEAD is now at 3226b63 add readme file

albert@homepc MINGW64 /d/gitstart (dev)
$ git status
On branch dev
Your branch is up to date with 'origin/dev'.

nothing to commit, working tree clean

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
还原03：版本库中的文件，修改或删除后执行了 git add 操作
使用了 git add 命令之后，文件的改变就放到了暂存区，这种情况可以使用git restore --staged file_name 或者 git reset HEAD file_name 命令。

git restore
执行 git restore --staged file_name 实际上是使用版本库中的文件覆盖暂存区中的数据，执行结束后文件状态变成了 <还原02> 中的情况。

albert@homepc MINGW64 /d/gitstart (dev)
$ git status
On branch dev
Your branch is up to date with 'origin/dev'.

nothing to commit, working tree clean

albert@homepc MINGW64 /d/gitstart (dev)
$ echo "test add">>README.md

albert@homepc MINGW64 /d/gitstart (dev)
$ git status
On branch dev
Your branch is up to date with 'origin/dev'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")

albert@homepc MINGW64 /d/gitstart (dev)
$ git add README.md
warning: LF will be replaced by CRLF in README.md.
The file will have its original line endings in your working directory

albert@homepc MINGW64 /d/gitstart (dev)
$ git status
On branch dev
Your branch is up to date with 'origin/dev'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   README.md


albert@homepc MINGW64 /d/gitstart (dev)
$ git restore --staged README.md

albert@homepc MINGW64 /d/gitstart (dev)
$ git status
On branch dev
Your branch is up to date with 'origin/dev'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49
50
51
git reset
git reset 命令如果加上 --hard 参数不能再加文件目录，只能将工作区全还原，如果不加默认参数为 --mixed，执行之后修改的文件状态变成了 <还原02> 中的情况。

albert@homepc MINGW64 /d/gitstart (dev)
$ git status
On branch dev
Your branch is up to date with 'origin/dev'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")

albert@homepc MINGW64 /d/gitstart (dev)
$ git add README.md
warning: LF will be replaced by CRLF in README.md.
The file will have its original line endings in your working directory

albert@homepc MINGW64 /d/gitstart (dev)
$ git status
On branch dev
Your branch is up to date with 'origin/dev'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   README.md


albert@homepc MINGW64 /d/gitstart (dev)
$ git reset HEAD README.md
Unstaged changes after reset:
M       README.md

albert@homepc MINGW64 /d/gitstart (dev)
$ git status
On branch dev
Your branch is up to date with 'origin/dev'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
还原04：版本库中的文件，修改或删除后执行了 git add、git commit 操作
git commit 命令一旦执行了之后就形成了“历史”，我们叫做提交日志，要想回退就得有篡改历史的能力，很幸运 Git 给了我们这种能力，其实提交之后我们可以把本地文件反向修改，然后再提交一次，但是我们说的还原，一般都是只倒退，既然是错误的提交，我们就像把这段“历史”抹去，这时就要用到 git reset HEAD^ 命令。

执行这个命令之后，刚刚的提交记录就被抹掉了，文件状态就回到了 <还原02> 的情况，如果加上参数 --soft 就会回到 <还原03> 的情况，如果加上参数 --hard ，就不能添加 file_name 这个文件名，然后整个工作区倒退到上一次修改之前，其他两种参数 --mixed 和 --soft 就可以指定添加名字。

这里的 HEAD^ 表示最新版本的前一版，也就是倒数第二版本，可以类推，HEAD^^ 表示倒数第三版本，HEAD^^^ 表示倒数第四版本。

另外还有另一种写法 HEAD~1 表示最新版本的前一版，也就是倒数第二版本，HEAD~2 表示倒数第三版本，HEAD~3 表示倒数第四版本。

其中 ^ 和 ~ 的含义并不相同，涉及到合并分支的概念，有兴趣的话可以多了解下，这里就不展开了，继续还原当前这种情况，我们选择 git reset HEAD^ 命令，先提交看下：

albert@homepc MINGW64 /d/gitstart (dev)
$ git status
On branch dev
Your branch is up to date with 'origin/dev'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")

albert@homepc MINGW64 /d/gitstart (dev)
$ git add README.md
warning: LF will be replaced by CRLF in README.md.
The file will have its original line endings in your working directory

albert@homepc MINGW64 /d/gitstart (dev)
$ git commit -m"modify readme 1"
[dev 8a40f22] modify readme 1
 1 file changed, 1 insertion(+)

albert@homepc MINGW64 /d/gitstart (dev)
$ git status
On branch dev
Your branch is ahead of 'origin/dev' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean

albert@homepc MINGW64 /d/gitstart (dev)
$ git log -2
commit 8a40f229881da037ff99070fa205d7819ba9f51b (HEAD -> dev)
Author: albert <qianxuan101@163.com>
Date:   Sat Mar 7 15:46:32 2020 +0800

    modify readme 1

commit 3226b63185a16398a02d5eaea47c95309ba49588 (origin/master, origin/dev, release, master)
Author: albert <qianxuan101@163.com>
Date:   Wed Feb 26 00:36:35 2020 +0800

    add readme file
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
然后再还原试试：

albert@homepc MINGW64 /d/gitstart (dev)
$ git reset HEAD^
Unstaged changes after reset:
M       README.md

albert@homepc MINGW64 /d/gitstart (dev)
$ git status
On branch dev
Your branch is up to date with 'origin/dev'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")

albert@homepc MINGW64 /d/gitstart (dev)
$ git log -2
commit 3226b63185a16398a02d5eaea47c95309ba49588 (HEAD -> dev, origin/master, origin/dev, release, master)
Author: albert <qianxuan101@163.com>
Date:   Wed Feb 26 00:36:35 2020 +0800

    add readme file
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
怎么样，历史被我们抹除了，需要注意的是，如果想还原“历史”，那么 git set 命令后面不能跟文件名，也就是说必须整个还原到上一版本，否则就相当于将单个文件简单反向修改添加到暂存区，而之前对文件的修改保留在本地，文件的日志并没有回退，具体的文件状态还得你自己操作感受一下。

还原05：版本库中的文件，修改或删除后执行了 git add、git commit、git push 操作
这种情况就是还原远程仓库的日志记录了，实际上操作步骤先按照 <还原04> 来处理，然后将本地分支情况推送到远程分支即可。

我们先把刚才的修改提交，然后推送到远程分支，使用 git status 可以看到本地分支已经领先远程分支了(Your branch is ahead of ‘origin/dev’ by 1 commit.)， git push 操作之后两个分支同步了。

albert@homepc MINGW64 /d/gitstart (dev)
$ git status
On branch dev
Your branch is ahead of 'origin/dev' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean

albert@homepc MINGW64 /d/gitstart (dev)
$ git log
commit a5b6c18db71a0487f6316f5db4304a99984f2ab3 (HEAD -> dev)
Author: albert <qianxuan101@163.com>
Date:   Sat Mar 7 15:51:56 2020 +0800

    modify readme 1

commit 3226b63185a16398a02d5eaea47c95309ba49588 (origin/master, origin/dev, release, master)
Author: albert <qianxuan101@163.com>
Date:   Wed Feb 26 00:36:35 2020 +0800

    add readme file

albert@homepc MINGW64 /d/gitstart (dev)
$ git push
Warning: Permanently added the ECDSA host key for IP address '180.97.125.228' to the list of known hosts.
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Writing objects: 100% (3/3), 286 bytes | 286.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote: Powered by GITEE.COM [GNK-3.8]
To gitee.com:myname/gitstart.git
   3226b63..a5b6c18  dev -> dev
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
这时通过远程仓库的管理软件，你可以看到远程分支已经有了最新的提交，然后我们可以参考 <还原04> 的情况，先将本地日志还原，再推送到远程仓库。

albert@homepc MINGW64 /d/gitstart (dev)
$ git reset HEAD^
Unstaged changes after reset:
M       README.md

albert@homepc MINGW64 /d/gitstart (dev)
$ git status
On branch dev
Your branch is behind 'origin/dev' by 1 commit, and can be fast-forwarded.
  (use "git pull" to update your local branch)

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")

albert@homepc MINGW64 /d/gitstart (dev)
$ git log
commit 3226b63185a16398a02d5eaea47c95309ba49588 (HEAD -> dev, origin/master, release, master)
Author: albert <qianxuan101@163.com>
Date:   Wed Feb 26 00:36:35 2020 +0800

    add readme file

albert@homepc MINGW64 /d/gitstart (dev)
$ git push
To gitee.com:myname/gitstart.git
 ! [rejected]        dev -> dev (non-fast-forward)
error: failed to push some refs to 'git@gitee.com:myname/gitstart.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
和想象的不太一样的，这种情况是远程仓库的记录领先，无法直接推送，此时可以添加 -f 参数，用本地提交记录覆盖远程分支记录：

albert@homepc MINGW64 /d/gitstart (dev)
$ git push -f
Total 0 (delta 0), reused 0 (delta 0)
remote: Powered by GITEE.COM [GNK-3.8]
To gitee.com:myname/gitstart.git
 + a5b6c18...3226b63 dev -> dev (forced update)
1
2
3
4
5
6
这次再查询远程分支记录，发现也被回退了，目的达成。

还原06：两次git commit 之后产生两条日志，只还原第一次提交
这种情况其实发生了两次修改和两次提交，和 <还原05> 情况不同的是要还原的提交不是最后一次，如果使用 git reset 命令必然将最后一次修改也还原了，虽然不能直接完成，但是给我们提供了解决问题的思路：

第一种方法：直接使用 git reset HEAD^^ 命令还原两次提交，然后在工作区将文件按第二次修改再改一次进行提交，这种方法适用于想要抹除第一次提交历史的情况。

第二种方法：如果你不在意提交历史，只是想还原第一次修改，那么可以使用 git revert HEAD^ 命令来反向修改那一次变化，修改之后会自动添加到暂存区，等待提交。

先来修改提交两次，产生两次记录：

albert@homepc MINGW64 /d/gitstart (dev)
$ git status
On branch dev
Your branch is up to date with 'origin/dev'.

nothing to commit, working tree clean

albert@homepc MINGW64 /d/gitstart (dev)
$ echo "m1">>README.md

albert@homepc MINGW64 /d/gitstart (dev)
$ git add README.md
warning: LF will be replaced by CRLF in README.md.
The file will have its original line endings in your working directory

albert@homepc MINGW64 /d/gitstart (dev)
$ git commit -m"modify README 1"
[dev e570df1] modify README 1
 1 file changed, 1 insertion(+)

albert@homepc MINGW64 /d/gitstart (dev)
$ echo "m2">>README.md

albert@homepc MINGW64 /d/gitstart (dev)
$ git add README.md
warning: LF will be replaced by CRLF in README.md.
The file will have its original line endings in your working directory

albert@homepc MINGW64 /d/gitstart (dev)
$ git commit -m"modify README 2"
[dev 140547f] modify README 2
 1 file changed, 1 insertion(+)
gi
albert@homepc MINGW64 /d/gitstart (dev)
$ git log
commit 140547f8d0b10d9a388beaf2ce522c38c878a839 (HEAD -> dev)
Author: albert <qianxuan101@163.com>
Date:   Sat Mar 7 16:26:17 2020 +0800

    modify README 2

commit e570df134b39ee7424bc8c48c1067e72c3fb9637
Author: albert <qianxuan101@163.com>
Date:   Sat Mar 7 16:26:07 2020 +0800

    modify README 1

commit 3226b63185a16398a02d5eaea47c95309ba49588 (origin/master, origin/dev, release, master)
Author: albert <qianxuan101@163.com>
Date:   Wed Feb 26 00:36:35 2020 +0800

    add readme file

albert@homepc MINGW64 /d/gitstart (dev)
$ cat README.md
learn git branch command
m1
m2
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49
50
51
52
53
54
55
56
57
58
然后使用 git revert HEAD^ 还原第一次修改记录：

albert@homepc MINGW64 /d/gitstart (dev)
$ git revert HEAD^
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
error: could not revert e570df1... modify README 1
hint: after resolving the conflicts, mark the corrected paths
hint: with 'git add <paths>' or 'git rm <paths>'
hint: and commit the result with 'git commit'

albert@homepc MINGW64 /d/gitstart (dev|REVERTING)
$ vi README.md

albert@homepc MINGW64 /d/gitstart (dev|REVERTING)
$ git add README.md

albert@homepc MINGW64 /d/gitstart (dev|REVERTING)
$ git commit
[dev 6ae97d0] Revert "modify README 1"
 1 file changed, 1 deletion(-)

albert@homepc MINGW64 /d/gitstart (dev)
$ git log
commit 6ae97d0e136abc1ed241854298037ca9d1c4460c (HEAD -> dev)
Author: albert <qianxuan101@163.com>
Date:   Sat Mar 7 16:31:50 2020 +0800

    Revert "modify README 1"
    
    This reverts commit e570df134b39ee7424bc8c48c1067e72c3fb9637.

commit 140547f8d0b10d9a388beaf2ce522c38c878a839
Author: albert <qianxuan101@163.com>
Date:   Sat Mar 7 16:26:17 2020 +0800

    modify README 2

commit e570df134b39ee7424bc8c48c1067e72c3fb9637
Author: albert <qianxuan101@163.com>
Date:   Sat Mar 7 16:26:07 2020 +0800

    modify README 1

commit 3226b63185a16398a02d5eaea47c95309ba49588 (origin/master, origin/dev, release, master)
Author: albert <qianxuan101@163.com>
Date:   Wed Feb 26 00:36:35 2020 +0800

    add readme file
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
因为修改了同一个文件，还原的时候还产生了冲突，解决冲突之后才提交，看日志发现这是一条新的记录，在实际操作的过程中可能会发生比这还要麻烦的场景，多练就好了。

常用集合
使用 Git 进行版本管理时，遇到的回退情况远不止这么多，这只是我目前常见的，之后遇到还会补充，每种情况我们其实不止有一种解决方式，接下来对于每种情况给一个我个人常用的处理方式，因为 git checkout 的作用被逐渐拆分成更具体的 git switch 和 git restore，我们尽量选择功能明确的命令：

还原00：工作区中未加到暂存区和版本库的文件，还原今天所做的修改
尝试下Ctrl+z吧，不行就找找自动保存的缓存文件，看看能不能找到之前版本
还原01：工作区中未加到暂存区和版本库的文件，执行了 git add 操作
直接使用 git restore --staged file_name 命令，如果版本不支持则使用 git rm --cached file_name
还原02：版本库中的文件，修改或删除后未执行 git add 操作
直接使用 git restore file_name 命令，如果版本不支持则使用 git checkout -- file_name
还原03：版本库中的文件，修改或删除后执行了 git add 操作
直接使用 git restore --staged file_name 命令，按 <还原02> 情况处理
还原04：版本库中的文件，修改或删除后执行了 git add、git commit 操作
直接使用 git reset HEAD^ 命令，按 <还原02> 情况处理，或者使用 git reset --soft HEAD^ 命令，按 <还原03> 情况处理
还原05：版本库中的文件，修改或删除后执行了 git add、git commit、git push 操作
先按照 <还原04> 情况处理，然后使用 git push -f 命令
还原06：两次git commit 之后产生两条日志，只还原第一次提交
使用 git revert HEAD^ 命令，解决冲突后提交，revert 后面跟具体的 commit id 也可以。
总结
参考这些具体的例子你会发现，很多操作选择在使用 git status 之后都有列举
所以说 git status 是一个可以提示你做选择的强大帮手，不知所措时可以试试它
Git 2.23版本之后学会用 git switch 和 git restore 命令，因为之前 git checkout 背负了太多了
最后放一幅图吧，只画了主要的，没有画出全部情况，否则会很乱，可以对照着练习一下

#### 找回删除文件

![gitfilestate](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9jZG4uanNkZWxpdnIubmV0L2doL2FsYmVydGdpdGh1YmhvbWUvY2RuL2ltZy9naXRmaWxlc3RhdGUucG5n?x-oss-process=image/format,png,image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9hbGJlcnRnaXRodWJob21lLmdpdGh1Yi5pby9ibG9nL2Fib3V0,size_18,color_FFFFFF,t_70#pic_center)

1.git checkout – <file_name>是将暂存区的修改重新放回工作区，但只能操作文件内容，不能添加、删除文件；
2.git restore --staged <file_name>相当于撤销git add 命令，git restore <file_name> 是放弃对工作区的修改，对文件的操作（添加、删除）和文件内容的操作都能使用此命令；
3.而git reset HEAD <file_name>与 git restore --staged <file_name>的作用相同。

将简单的事情弄复杂也是人才啊！原来git并不是用restore的，工作区的用checkout 放弃修改，添加后的用reset head 。这样做指令风险太大。reset的模式就很多。加入restore 就改变了很多，但离完美还差的远。 restore 是对指定区域撤销一步。 工作区域 撤销 git restore 文件名， 执行后 指针指向 Stage Head[0] || Repository Head[0] 短路或的作用，如果暂存区有新版本就直接用暂存区的版本，没有最新就用 仓库最新版本。实际上暂存区的 Head[0] 在没有新加文件都是指向仓库区的。 git restore --staged <file_name> 这个只是针对暂存区撤销一步，指针指向 Repository Head[0]，所以你无论添加多少次操作都是恢复到仓库最后一次提交的指针。

1. commit并push了：
   1. 切换到删除文件的分支：git checkout 分支
   2. 查看提交记录：git log
   3. 复制一个删除前时间的提交码，新建一个分支：git checkout –b newdev 3a839a216a9091ad40b
   4. 将找回的文件复制出来
   5. 切换回原分支：git checkout dev, 复制找回文件进去
   6. 删除新建的分支：git branch -d newdev
   
2. add但未commit      

   git checkout --  file

   `git reset -- *files*` 用来撤销最后一次`git add *files*`，你也可以用`git reset` 撤销所有暂存区域文件。

   git restore --staged   <file>  撤销至已修改但未add的状态

3. 未add      文件系统管理

#### 版本回退

git reset HEAD <file_name> 丢弃暂存区的修改，重新放回工作区，会将暂存区的内容和本地已提交的内容全部恢复到未暂存的状态，不影响原来本地文件(相当于撤销git add 操作，不影响上一次commit后对本地文件的修改) （包括对文件的操作，如添加文件、删除文件）

git reset –hard HEAD 清空暂存区，将已提交的内容的版本恢复到本地，本地的文件也将被恢复的版本替换（恢复到上一次commit后的状态，上一次commit后的修改也丢弃）

```text
git reset --hard 248cba8e77231601d1189e3576dc096c8986ae51 
git reset --hard/soft <commit_id> // 回滚到某一个版本
git reset --hard/soft HEAD~<num> // 回滚num个提交
git revert <merge_commit_id> -m number // 撤销某一次merge
```

- HEAD 表示当前版本

- HEAD^ 上一个版本

- HEAD^^ 上上一个版本

- HEAD^^^ 上上上一个版本

  

- 以此类推...

  

可以使用 ～数字表示

- HEAD~0 表示当前版本
- HEAD~1 上一个版本
- HEAD^2 上上一个版本
- HEAD^3 上上上一个版本

回退的是所有文件，如果后悔回退可以git pull就可以了。

1. 删除本地文件，但是未添加到暂存区；  已add，再删除，删除东躲没有添加到index区，找回：git checkout -- file.

2. 删除本地文件，并且把删除操作添加到了暂存区；git reset HEAD file,回退到1描述的状态，再使用1的找回。

3. 把暂存区的操作提交到了本地git库；

   1. git log --pretty=oneline，此时我们进行版本回滚操作
      选择ID的前几位字符串就足够表示版本的唯一性，输入命令 git reset --hard 6aef62

4. 把本地git库的删除记录推送到了远程服务器github。

   1. 可以看到文件被从服务器中删除了，如果此时进行文件恢复操作就需要像上面第三步那样进行版本回滚操作
      当然了，这个版本回滚是在本地进行的，然后把回滚后的版本提交。

   2. 把回滚后的版本提交到远程服务器 git push，可以看到提交出错，因为git默认是高版本覆盖低版本，但不能反过来操作，因为回滚的版本比服务器此时的版本低，所以此时常规手段是无效的，但可以进行版本的暴力提交——force，通过git的提示 git push -h 可以查看到 force的说明。进行暴力提交 git push -f

   3. 如果要在服务器中回到删除文件的高级git版本版本该怎么操作呢？当然了，最简单的办法就是把费了老劲恢复的文件再次删除就可以，这样效果上是可以的，但是之前的那个git版本号其实是丢失了。

      git中有一个命令 reflog 可以查看每次提交的信息，包括 commit ID和名称，可以通过提交信息找到之前的版本好，然后重新回滚就可以了。

      然后再进行暴力提交



当执行提交操作（git commit）时，暂存区的目录树写到版本库（对象库）中，master 分支会做相应的更新。即 master 指向的目录树就是提交时暂存区的目录树。当执行 git reset HEAD 命令时，暂存区的目录树会被重写，被 master 分支指向的目录树所替换，但是工作区不受影响。当执行 git rm --cached <file> 命令时，会直接从暂存区删除文件，工作区则不做出改变。当执行 git checkout 或者git checkout -- <file>命令时，会用暂存区全部或指定的文件替换工作区的文件。这个操作很危险，会清除工作区中未添加到暂存区的改动。当执行 git checkout HEAD或者 git checkout HEAD <file>命令时，会用 HEAD 指向的 master 分支中的全部或者部分文件替换暂存区和以及工作区中的文件。这个命令也是极具危险性的，因为不但会清除工作区中未提交的改动，也会清除暂存区中未提交的改 动。





如果既没有指定文件名，也没有指定分支名，而是一个标签、远程分支、SHA-1值或者是像*main~3*类似的东西，就得到一个匿名分支，称作*detached HEAD*（被分离的*HEAD*标识）。这样可以很方便地在历史版本之间互相切换。比如说你想要编译1.6.6.1版本的git，你可以运行`git checkout v1.6.6.1`（这是一个标签，而非分支名），编译，安装，然后切换回另一个分支，比如说`git checkout main`。



#### 撤销修改

**git restore --staged** <file_name> 将暂存区的修改重新放回工作区（**包括对文件自身的操作，如添加文件、删除文件**）

**git restore** <file_name> 丢弃工作区的修改（**包括对文件自身的操作，如添加文件、删除文件**）

1. git restore --staged   <file>  撤销至已修改但未add的状态

2. 已commit的撤销

   ```
   git restore -s HEAD~1 READEME.md  // 该命名表示将版本回退到当前快照的前一个版本
   git restore -s 91410eb9  READEME.md  // 改命令指定明确的 commit id ，回退到指定的快照中
   git reset --soft HEAD^  // 该命令表示撤销 commit 至上一次 commit 的版本
   ```

   `restore` 命令，默认是带着 `--worktree` 参数的

|                命令                |                        作用                         |     备注      |
| :--------------------------------: | :-------------------------------------------------: | :-----------: |
| `git restore --worktree README.md` |        表示撤销 README.md 文件工作区的的修改        | 参数等同于 -W |
|  `git restore --staged README.md`  | 表示撤销暂存区的修改，将文件状态恢复到未 `add` 之前 | 参数等同于 -S |
| `git restore -s HEAD~1 README.md`  |       表示将当前工作区切换到上个 commit 版本        |               |
| `git restore -s dbv213 README.md`  |     表示将当前工作区切换到指定 commit id 的版本     |               |

要注意git restore命令在工作区是不会其作用的，也就是一个文件在工作区，使用git restore是不起作用的。需要使用git add后才能对该文件使用git restore命令。也就是在把文件加到暂存区后对文件进行修改，再执行git restore <file>会撤销文件的修改，撤销到最近一次执行git add的内容。

1、文件在暂存区且未作修改的情况

​	使用git restore --staged <file> 把文件从暂存区移动到工作区，即文件不被追踪；

2、文件在暂存区且已经修改的情况

​	使用git restore --staged <file> 把文件从暂存区移动到工作区，且不会撤销修改的内容；

​	使用git restore <file> 文件仍在暂存区且会撤销文件修改的内容；

3、文件在本地代码库已经修改的情况

​	使用git add <file> 把文件重新放到暂存区，且保留文件的修改；

​	使用git restore <file> 文件仍在本地代码库且会撤销文件的修改；

​     **对于git restore <file>命令，会撤销文件的修改，使文件恢复到暂存区或本地代码库（取决于文件在修改前的状态）；**

​	**对于git restore --staged <file>命令，把文件从暂存区撤回到工作区，保留文件最后一次修改的内容；**





## Level 3

