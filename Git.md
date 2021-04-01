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



在使用git命令的时候，后面可能会带有参数，有些参数前面是单横杠有些是双横杠。

下面就简单介绍一下两种横杠的使用场景。

**一.单横杠短选项命令（UNIX风格）：**

（1）.一个短选项命令，由横杠（-）紧跟单个短选项字符。

（2）.多个短选项命令，由横杠（-）紧跟每个短选项字符。

（3）.命令和参数之间用空格分隔。

（4）.仅作为连字符。

代码如下：

```
[Shell] 纯文本查看 复制代码



$ git commit -m "蚂蚁部落第一次提交"
```

单横杠后面是一个字符m，与参数"蚂蚁部落第一次提交"用空格分隔。

```
[Shell] 纯文本查看 复制代码



$ rm -rf ant
```

上面代码是由2个短选项构成。

```
[Shell] 纯文本查看 复制代码



$ git show --name-only
```

上面的单个横杠（-）仅作为连字符使用。

**二.双横杠长选项命令（GNU风格）：**

（1）.长选项命令，有两个（--）紧跟长选项单词（单词不能简写）。

（2）.长选项后面跟参数，用空格或等号分隔。

代码如下：

```
[Shell] 纯文本查看 复制代码



$ git log --pretty=oneline
```

特别说明：双横杠（--）后面不一定都是参数，也许是路径，如果是路径则需要用空格分隔：

```
[Shell] 纯文本查看 复制代码



$ git checkout -- readme.txt
```







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





# 7.7 Git 工具 - 重置揭密

## 重置揭密

在继续了解更专业的工具前，我们先探讨一下 Git 的 `reset` 和 `checkout` 命令。 在初遇的 Git 命令中，这两个是最让人困惑的。 它们能做很多事情，所以看起来我们很难真正地理解并恰当地运用它们。 针对这一点，我们先来做一个简单的比喻。

### 三棵树

理解 `reset` 和 `checkout` 的最简方法，就是以 Git 的思维框架（将其作为内容管理器）来管理三棵不同的树。 “树” 在我们这里的实际意思是 “文件的集合”，而不是指特定的数据结构。 （在某些情况下索引看起来并不像一棵树，不过我们现在的目的是用简单的方式思考它。）

Git 作为一个系统，是以它的一般操作来管理并操纵这三棵树的：

| 树                | 用途                                 |
| :---------------- | :----------------------------------- |
| HEAD              | 上一次提交的快照，下一次提交的父结点 |
| Index             | 预期的下一次提交的快照               |
| Working Directory | 沙盒                                 |

#### HEAD

HEAD 是当前分支引用的指针，它总是指向该分支上的最后一次提交。 这表示 HEAD 将是下一次提交的父结点。 通常，理解 HEAD 的最简方式，就是将它看做 **该分支上的最后一次提交** 的快照。

其实，查看快照的样子很容易。 下例就显示了 HEAD 快照实际的目录列表，以及其中每个文件的 SHA-1 校验和：

```console
$ git cat-file -p HEAD
tree cfda3bf379e4f8dba8717dee55aab78aef7f4daf
author Scott Chacon  1301511835 -0700
committer Scott Chacon  1301511835 -0700

initial commit

$ git ls-tree -r HEAD
100644 blob a906cb2a4a904a152...   README
100644 blob 8f94139338f9404f2...   Rakefile
040000 tree 99f1a6d12cb4b6f19...   lib
```

Git 的 `cat-file` 和 `ls-tree` 是底层命令，它们一般用于底层工作，在日常工作中并不使用。 不过它们能帮助我们了解到底发生了什么。

#### 索引

索引是你的 **预期的下一次提交**。 我们也会将这个概念引用为 Git 的“暂存区”，这就是当你运行 `git commit` 时 Git 看起来的样子。

Git 将上一次检出到工作目录中的所有文件填充到索引区，它们看起来就像最初被检出时的样子。 之后你会将其中一些文件替换为新版本，接着通过 `git commit` 将它们转换为树来用作新的提交。

```console
$ git ls-files -s
100644 a906cb2a4a904a152e80877d4088654daad0c859 0	README
100644 8f94139338f9404f26296befa88755fc2598c289 0	Rakefile
100644 47c6340d6459e05787f644c2447d2595f5d3a54b 0	lib/simplegit.rb
```

再说一次，我们在这里又用到了 `git ls-files` 这个幕后的命令，它会显示出索引当前的样子。

确切来说，索引在技术上并非树结构，它其实是以扁平的清单实现的。不过对我们而言，把它当做树就够了。

#### 工作目录

最后，你就有了自己的 **工作目录**（通常也叫 **工作区**）。 另外两棵树以一种高效但并不直观的方式，将它们的内容存储在 `.git` 文件夹中。 工作目录会将它们解包为实际的文件以便编辑。 你可以把工作目录当做 **沙盒**。在你将修改提交到暂存区并记录到历史之前，可以随意更改。

```console
$ tree
.
├── README
├── Rakefile
└── lib
    └── simplegit.rb

1 directory, 3 files
```

### 工作流程

经典的 Git 工作流程是通过操纵这三个区域来以更加连续的状态记录项目快照的。

![reset workflow](https://git-scm.com/book/en/v2/images/reset-workflow.png)

让我们来可视化这个过程：假设我们进入到一个新目录，其中有一个文件。 我们称其为该文件的 **v1** 版本，将它标记为蓝色。 现在运行 `git init`，这会创建一个 Git 仓库，其中的 HEAD 引用指向未创建的 `master` 分支。

![reset ex1](https://git-scm.com/book/en/v2/images/reset-ex1.png)

此时，只有工作目录有内容。

现在我们想要提交这个文件，所以用 `git add` 来获取工作目录中的内容，并将其复制到索引中。

![reset ex2](https://git-scm.com/book/en/v2/images/reset-ex2.png)

接着运行 `git commit`，它会取得索引中的内容并将它保存为一个永久的快照， 然后创建一个指向该快照的提交对象，最后更新 `master` 来指向本次提交。

![reset ex3](https://git-scm.com/book/en/v2/images/reset-ex3.png)

此时如果我们运行 `git status`，会发现没有任何改动，因为现在三棵树完全相同。

现在我们想要对文件进行修改然后提交它。 我们将会经历同样的过程；首先在工作目录中修改文件。 我们称其为该文件的 **v2** 版本，并将它标记为红色。

![reset ex4](https://git-scm.com/book/en/v2/images/reset-ex4.png)

如果现在运行 `git status`，我们会看到文件显示在 “Changes not staged for commit” 下面并被标记为红色，因为该条目在索引与工作目录之间存在不同。 接着我们运行 `git add` 来将它暂存到索引中。

![reset ex5](https://git-scm.com/book/en/v2/images/reset-ex5.png)

此时，由于索引和 HEAD 不同，若运行 `git status` 的话就会看到 “Changes to be committed” 下的该文件变为绿色 ——也就是说，现在预期的下一次提交与上一次提交不同。 最后，我们运行 `git commit` 来完成提交。

![reset ex6](https://git-scm.com/book/en/v2/images/reset-ex6.png)

现在运行 `git status` 会没有输出，因为三棵树又变得相同了。

切换分支或克隆的过程也类似。 当检出一个分支时，它会修改 **HEAD** 指向新的分支引用，将 **索引** 填充为该次提交的快照， 然后将 **索引** 的内容复制到 **工作目录** 中。

### 重置的作用

在以下情景中观察 `reset` 命令会更有意义。

为了演示这些例子，假设我们再次修改了 `file.txt` 文件并第三次提交它。 现在的历史看起来是这样的：

![reset start](https://git-scm.com/book/en/v2/images/reset-start.png)

让我们跟着 `reset` 看看它都做了什么。 它以一种简单可预见的方式直接操纵这三棵树。 它做了三个基本操作。

#### 第 1 步：移动 HEAD

`reset` 做的第一件事是移动 HEAD 的指向。 这与改变 HEAD 自身不同（`checkout` 所做的）；`reset` 移动 HEAD 指向的分支。 这意味着如果 HEAD 设置为 `master` 分支（例如，你正在 `master` 分支上）， 运行 `git reset 9e5e6a4` 将会使 `master` 指向 `9e5e6a4`。

![reset soft](https://git-scm.com/book/en/v2/images/reset-soft.png)

无论你调用了何种形式的带有一个提交的 `reset`，它首先都会尝试这样做。 使用 `reset --soft`，它将仅仅停在那儿。

现在看一眼上图，理解一下发生的事情：它本质上是撤销了上一次 `git commit` 命令。 当你在运行 `git commit` 时，Git 会创建一个新的提交，并移动 HEAD 所指向的分支来使其指向该提交。 当你将它 `reset` 回 `HEAD~`（HEAD 的父结点）时，其实就是把该分支移动回原来的位置，而不会改变索引和工作目录。 现在你可以更新索引并再次运行 `git commit` 来完成 `git commit --amend` 所要做的事情了（见 [修改最后一次提交](https://git-scm.com/book/zh/v2/ch00/_git_amend)）。

#### 第 2 步：更新索引（--mixed）

注意，如果你现在运行 `git status` 的话，就会看到新的 HEAD 和以绿色标出的它和索引之间的区别。

接下来，`reset` 会用 HEAD 指向的当前快照的内容来更新索引。

![reset mixed](https://git-scm.com/book/en/v2/images/reset-mixed.png)

如果指定 `--mixed` 选项，`reset` 将会在这时停止。 这也是默认行为，所以如果没有指定任何选项（在本例中只是 `git reset HEAD~`），这就是命令将会停止的地方。

现在再看一眼上图，理解一下发生的事情：它依然会撤销一上次 `提交`，但还会 *取消暂存* 所有的东西。 于是，我们回滚到了所有 `git add` 和 `git commit` 的命令执行之前。

#### 第 3 步：更新工作目录（--hard）

`reset` 要做的的第三件事情就是让工作目录看起来像索引。 如果使用 `--hard` 选项，它将会继续这一步。

![reset hard](https://git-scm.com/book/en/v2/images/reset-hard.png)

现在让我们回想一下刚才发生的事情。 你撤销了最后的提交、`git add` 和 `git commit` 命令 **以及** 工作目录中的所有工作。

必须注意，`--hard` 标记是 `reset` 命令唯一的危险用法，它也是 Git 会真正地销毁数据的仅有的几个操作之一。 其他任何形式的 `reset` 调用都可以轻松撤消，但是 `--hard` 选项不能，因为它强制覆盖了工作目录中的文件。 在这种特殊情况下，我们的 Git 数据库中的一个提交内还留有该文件的 **v3** 版本， 我们可以通过 `reflog` 来找回它。但是若该文件还未提交，Git 仍会覆盖它从而导致无法恢复。

#### 回顾

`reset` 命令会以特定的顺序重写这三棵树，在你指定以下选项时停止：

1. 移动 HEAD 分支的指向 *（若指定了 `--soft`，则到此停止）*
2. 使索引看起来像 HEAD *（若未指定 `--hard`，则到此停止）*
3. 使工作目录看起来像索引

### 通过路径来重置

前面讲述了 `reset` 基本形式的行为，不过你还可以给它提供一个作用路径。 若指定了一个路径，`reset` 将会跳过第 1 步，并且将它的作用范围限定为指定的文件或文件集合。 这样做自然有它的道理，因为 HEAD 只是一个指针，你无法让它同时指向两个提交中各自的一部分。 不过索引和工作目录 *可以部分更新*，所以重置会继续进行第 2、3 步。

现在，假如我们运行 `git reset file.txt` （这其实是 `git reset --mixed HEAD file.txt` 的简写形式，因为你既没有指定一个提交的 SHA-1 或分支，也没有指定 `--soft` 或 `--hard`），它会：

1. 移动 HEAD 分支的指向 *（已跳过）*
2. 让索引看起来像 HEAD *（到此处停止）*

所以它本质上只是将 `file.txt` 从 HEAD 复制到索引中。

![reset path1](https://git-scm.com/book/en/v2/images/reset-path1.png)

它还有 *取消暂存文件* 的实际效果。 如果我们查看该命令的示意图，然后再想想 `git add` 所做的事，就会发现它们正好相反。

![reset path2](https://git-scm.com/book/en/v2/images/reset-path2.png)

这就是为什么 `git status` 命令的输出会建议运行此命令来取消暂存一个文件。 （查看 [取消暂存的文件](https://git-scm.com/book/zh/v2/ch00/_unstaging) 来了解更多。）

我们可以不让 Git 从 HEAD 拉取数据，而是通过具体指定一个提交来拉取该文件的对应版本。 我们只需运行类似于 `git reset eb43bf file.txt` 的命令即可。

![reset path3](https://git-scm.com/book/en/v2/images/reset-path3.png)

它其实做了同样的事情，也就是把工作目录中的文件恢复到 **v1** 版本，运行 `git add` 添加它， 然后再将它恢复到 **v3** 版本（只是不用真的过一遍这些步骤）。 如果我们现在运行 `git commit`，它就会记录一条“将该文件恢复到 **v1** 版本”的更改， 尽管我们并未在工作目录中真正地再次拥有它。

还有一点同 `git add` 一样，就是 `reset` 命令也可以接受一个 `--patch` 选项来一块一块地取消暂存的内容。 这样你就可以根据选择来取消暂存或恢复内容了。

### 压缩

我们来看看如何利用这种新的功能来做一些有趣的事情——压缩提交。

假设你的一系列提交信息中有 “oops.”“WIP” 和 “forgot this file”， 聪明的你就能使用 `reset` 来轻松快速地将它们压缩成单个提交，也显出你的聪明。 （[压缩提交](https://git-scm.com/book/zh/v2/ch00/_squashing) 展示了另一种方式，不过在本例中用 `reset` 更简单。）

假设你有一个项目，第一次提交中有一个文件，第二次提交增加了一个新的文件并修改了第一个文件，第三次提交再次修改了第一个文件。 由于第二次提交是一个未完成的工作，因此你想要压缩它。

![reset squash r1](https://git-scm.com/book/en/v2/images/reset-squash-r1.png)

那么可以运行 `git reset --soft HEAD~2` 来将 HEAD 分支移动到一个旧一点的提交上（即你想要保留的最近的提交）：

![reset squash r2](https://git-scm.com/book/en/v2/images/reset-squash-r2.png)

然后只需再次运行 `git commit`：

![reset squash r3](https://git-scm.com/book/en/v2/images/reset-squash-r3.png)

现在你可以查看可到达的历史，即将会推送的历史，现在看起来有个 v1 版 `file-a.txt` 的提交， 接着第二个提交将 `file-a.txt` 修改成了 v3 版并增加了 `file-b.txt`。 包含 v2 版本的文件已经不在历史中了。

### 检出

最后，你大概还想知道 `checkout` 和 `reset` 之间的区别。 和 `reset` 一样，`checkout` 也操纵三棵树，不过它有一点不同，这取决于你是否传给该命令一个文件路径。

#### 不带路径

运行 `git checkout [branch]` 与运行 `git reset --hard [branch]` 非常相似，它会更新所有三棵树使其看起来像 `[branch]`，不过有两点重要的区别。

首先不同于 `reset --hard`，`checkout` 对工作目录是安全的，它会通过检查来确保不会将已更改的文件弄丢。 其实它还更聪明一些。它会在工作目录中先试着简单合并一下，这样所有 *还未修改过的* 文件都会被更新。 而 `reset --hard` 则会不做检查就全面地替换所有东西。

第二个重要的区别是 `checkout` 如何更新 HEAD。 `reset` 会移动 HEAD 分支的指向，而 `checkout` 只会移动 HEAD 自身来指向另一个分支。

例如，假设我们有 `master` 和 `develop` 分支，它们分别指向不同的提交；我们现在在 `develop` 上（所以 HEAD 指向它）。 如果我们运行 `git reset master`，那么 `develop` 自身现在会和 `master` 指向同一个提交。 而如果我们运行 `git checkout master` 的话，`develop` 不会移动，HEAD 自身会移动。 现在 HEAD 将会指向 `master`。

所以，虽然在这两种情况下我们都移动 HEAD 使其指向了提交 A，但 *做法* 是非常不同的。 `reset` 会移动 HEAD 分支的指向，而 `checkout` 则移动 HEAD 自身。

![reset checkout](https://git-scm.com/book/en/v2/images/reset-checkout.png)

#### 带路径

运行 `checkout` 的另一种方式就是指定一个文件路径，这会像 `reset` 一样不会移动 HEAD。 它就像 `git reset [branch] file` 那样用该次提交中的那个文件来更新索引，但是它也会覆盖工作目录中对应的文件。 它就像是 `git reset --hard [branch] file`（如果 `reset` 允许你这样运行的话）， 这样对工作目录并不安全，它也不会移动 HEAD。

此外，同 `git reset` 和 `git add` 一样，`checkout` 也接受一个 `--patch` 选项，允许你根据选择一块一块地恢复文件内容。

### 总结

希望你现在熟悉并理解了 `reset` 命令，不过关于它和 `checkout` 之间的区别，你可能还是会有点困惑，毕竟不太可能记住不同调用的所有规则。

下面的速查表列出了命令对树的影响。 “HEAD” 一列中的 “REF” 表示该命令移动了 HEAD 指向的分支引用，而 “HEAD” 则表示只移动了 HEAD 自身。 特别注意 *WD Safe?* 一列——如果它标记为 **NO**，那么运行该命令之前请考虑一下。

|                             | HEAD | Index | Workdir | WD Safe? |
| :-------------------------- | :--- | :---- | :------ | :------- |
| **Commit Level**            |      |       |         |          |
| `reset --soft [commit]`     | REF  | NO    | NO      | YES      |
| `reset [commit]`            | REF  | YES   | NO      | YES      |
| `reset --hard [commit]`     | REF  | YES   | YES     | **NO**   |
| `checkout <commit>`         | HEAD | YES   | YES     | YES      |
| **File Level**              |      |       |         |          |
| `reset [commit] <paths>`    | NO   | YES   | NO      | YES      |
| `checkout [commit] <paths>` | NO   | YES   | YES     | **NO**   |







有时候，我们用Git的时候有可能commit提交代码后，发现这一次commit的内容是有错误的，那么有两种处理方法：
 1、修改错误内容，再次commit一次 2、使用**git reset** 命令撤销这一次错误的commit
 第一种方法比较直接，但会多次一次commit记录。
 而我个人更倾向第二种方法，错误的commit没必要保留下来。
 那么今天来说一下**git reset**。它的一句话概括



```undefined
git-reset - Reset current HEAD to the specified state
```

意思就是可以让HEAD这个指针指向其他的地方。例如我们有一次commit不是不是很满意，需要回到上一次的Commit里面。那么这个时候就需要通过reset，把HEAD指针指向上一次的commit的点。
 它有三种模式，soft,mixed,hard，具体的使用方法下面这张图，展示的很全面了。

![img](https:////upload-images.jianshu.io/upload_images/4428238-fcad08ebe26933a6.png?imageMogr2/auto-orient/strip|imageView2/2/w/638/format/webp)

git各个区域和命令关系



这三个模式理解了，对于使用这个命令很有帮助。在理解这三个模式之前，需要略微知道一点Git的基本流程。正如上图，Git会有三个区域：

- **Working Tree** 当前的工作区域
- **Index/Stage** 暂存区域，和git stash命令暂存的地方不一样。使用git add xx，就可以将xx添加近Stage里面
- **Repository** 提交的历史，即使用git commit提交后的结果

![img](https:////upload-images.jianshu.io/upload_images/4428238-5b04868bd09cb72a.png?imageMogr2/auto-orient/strip|imageView2/2/w/546/format/webp)

文件存入Repository流程

以下简单敘述一下把文件存入Repository流程：

1. 刚开始 working tree 、 index 与 repository(HEAD)里面的內容都是一致的

   ![img](https:////upload-images.jianshu.io/upload_images/4428238-fb3e9ca4dce0328c.png?imageMogr2/auto-orient/strip|imageView2/2/w/469/format/webp)

   阶段1

2. 当git管理的文件夹里面的内容出现改变后，此時 working tree 的內容就会跟 index 及 repository(HEAD)的不一致，而Git知道是哪些文件(Tracked File)被改动过，直接将文件状态设置为 modified (Unstaged files)。

   ![img](https:////upload-images.jianshu.io/upload_images/4428238-e92ef69bc699fad5.png?imageMogr2/auto-orient/strip|imageView2/2/w/465/format/webp)

   阶段2

3. 当我們执行 git add 后，会将这些改变的文件內容加入 index 中 (Staged files)，所以此时working tree跟index的內容是一致的，但他们与repository(HEAD)內容不一致。

   ![img](https:////upload-images.jianshu.io/upload_images/4428238-0b04f397c336d245.png?imageMogr2/auto-orient/strip|imageView2/2/w/466/format/webp)

   阶段3

4. 接着执行 git commit 後，將Git索引中所有改变的文件內容提交至 Repository 中，建立出新的 commit 节点(HEAD)后， working tree 、 index 與与repository(HEAD)区域的内容 又会保持一致。

   ![img](https:////upload-images.jianshu.io/upload_images/4428238-75a651c0a39381a0.png?imageMogr2/auto-orient/strip|imageView2/2/w/473/format/webp)

   阶段4

### 实战演示

### reset --hard：重置stage区和工作目录:

**reset --hard** 会在重置 **HEAD** 和**branch**的同时，重置stage区和工作目录里的内容。当你在 **reset** 后面加了 **--hard** 参数时，你的stage区和工作目录里的内容会被完全重置为和**HEAD**的新位置相同的内容。换句话说，就是你的没有**commit**的修改会被全部擦掉。

例如你在上次 **commit** 之后又对文件做了一些改动：把修改后的**ganmes.txt**文件**add**到**stage区**，修改后的**shopping list.txt**保留在**工作目录**



```shell
git status
```



![img](https:////upload-images.jianshu.io/upload_images/4428238-1c22b16e14586320.png?imageMogr2/auto-orient/strip|imageView2/2/w/621/format/webp)

最初状态


 然后，你执行了**reset**并附上了**--hard**参数：





```shell
git reset --hard HEAD^
```

你的 **HEAD \**和当前\** branch** 切到上一条**commit** 的同时，你工作目录里的新改动和已经add到stage区的新改动也一起全都消失了：



```shell
git status
```



![img](https:////upload-images.jianshu.io/upload_images/4428238-3e57cea5e5400c94.png?imageMogr2/auto-orient/strip|imageView2/2/w/355/format/webp)

reset --hard head^之后


 可以看到，在 **reset --hard** 后，所有的改动都被擦掉了。



### reset --soft：保留工作目录，并把重置 HEAD 所带来的新的差异放进暂存区

**reset --soft** 会在重置 **HEAD** 和 **branch** 时，保留工作目录和暂存区中的内容，并把重置 **HEAD** 所带来的新的差异放进暂存区。

什么是「重置 **HEAD** 所带来的新的差异」？就是这里：

![img](https:////upload-images.jianshu.io/upload_images/4428238-75ef41dc9eec6f8e?imageMogr2/auto-orient/strip|imageView2/2/w/478/format/webp)



由于 **HEAD** 从 4 移动到了 3，而且在 reset 的过程中工作目录和暂存区的内容没有被清理掉，所以 4 中的改动在 **reset** 后就也成了工作目录新增的「工作目录和 **HEAD** 的差异」。这就是上面一段中所说的「重置 **HEAD** 所带来的差异」。

此模式下会保留 **working tree工作目录**的內容，不会改变到目前所有的git管理的文件夹的內容；也会
 保留 **index暂存区**的內容，让 **index 暂存区**与 **working tree** 工作目录的內容是一致的。就只有 **repository** 中的內容的更变需要与 **reset** 目标节点一致，因此原始节点与**reset**节点之间的差异变更集合会存在与index暂存区中(**Staged files**)，所以我们可以直接执行 **git commit** 將 **index暂存区**中的內容提交至 **repository** 中。当我们想合并「当前节点」与「reset目标节点」之间不具太大意义的 **commit** 记录(可能是阶段性地频繁提交)時，可以考虑使用 **Soft Reset** 来让 **commit** 演进线图较为清晰点。

![img](https:////upload-images.jianshu.io/upload_images/4428238-2fcf8c37b866d331.png?imageMogr2/auto-orient/strip|imageView2/2/w/422/format/webp)



所以在同样的情况下，还是老样子：把修改后的**ganmes.txt**文件**add**到**stage区**，修改后的**shopping list.txt**保留在**工作目录**



```shell
git status
```



![img](https:////upload-images.jianshu.io/upload_images/4428238-0e5563f9d547f694.png?imageMogr2/auto-orient/strip|imageView2/2/w/621/format/webp)

最初状态


 假设此时当前 **commit** 的改动内容是新增了 **laughters.txt** 文件：





```shell
git show --stat
```

![img](https:////upload-images.jianshu.io/upload_images/4428238-1ac751cca1b142da.png?imageMogr2/auto-orient/strip|imageView2/2/w/533/format/webp)

git show --stat

如果这时你执行：



```shell
git reset --soft HEAD^
```

那么除了 **HEAD** 和它所指向的 **branch1** 被移动到 **HEAD^** 之外，原先 **HEAD** 处 **commit** 的改动（也就是那个 **laughters.txt** 文件）也会被放进暂存区：



```shell
git status
```



![img](https:////upload-images.jianshu.io/upload_images/4428238-65f172789e8446da.png?imageMogr2/auto-orient/strip|imageView2/2/w/626/format/webp)

使用git reset --soft HEAD^后


 这就是**--soft** 和 **--hard** 的区别：**--hard** 会清空工作目录和暂存区的改动,*而 **--soft则会保留工作目录的内容，并把因为保留工作目录内容所带来的新的文件差异放进暂存区**。



### reset 不加参数(mixed)：保留工作目录，并清空暂存区

**reset** 如果不加参数，那么默认使用 **--mixed** 参数。它的行为是：保留工作目录，并且清空暂存区。也就是说，工作目录的修改、暂存区的内容以及由 **reset** 所导致的新的文件差异，都会被放进工作目录。简而言之，就是「把所有差异都混合（mixed）放在工作目录中」。

还以同样的情况为例：



```shell
git status
```

![img](https:////upload-images.jianshu.io/upload_images/4428238-5e1d7adf0b472564.png?imageMogr2/auto-orient/strip|imageView2/2/w/621/format/webp)

最初状态

**修改了 的games.txt 和 shopping list.txt，并把 games.txt 放进了暂存区。**



```shell
git show --stat
```

![img](https:////upload-images.jianshu.io/upload_images/4428238-5ed83eef2b0c00e7.png?imageMogr2/auto-orient/strip|imageView2/2/w/533/format/webp)

git show --stat

**最新的 commit 中新增了 laughters.txt 文件。**

这时如果你执行**无参数**的**reset**或者带**--mixed**参数：



```shell
git reset HEAD^
git reset --mixed HEAD^
```

工作目录的内容和 **--soft** 一样会被保留，但和 **--soft** 的区别在于，它会把暂存区清空,并把原节点和**reset**节点的差异的文件放在工作目录，总而言之就是，工作目录的修改、暂存区的内容以及由 **reset** 所导致的新的文件差异，都会被放进工作目录



```shell
git status
```

![img](https:////upload-images.jianshu.io/upload_images/4428238-64770040c28c4c97.png?imageMogr2/auto-orient/strip|imageView2/2/w/625/format/webp)

git reset HEAD^之后

# 总结

### reset 的本质：移动 HEAD 以及它所指向的 branch

实质上，**reset** 这个指令虽然可以用来撤销 **commit** ，但它的实质行为并不是撤销，而是移动 **HEAD** ，并且「捎带」上 **HEAD** 所指向的 **branch**（如果有的话）。也就是说，**reset** 这个指令的行为其实和它的字面意思 "**reset**"（重置）十分相符：它是用来重置 **HEAD** 以及它所指向的 **branch** 的位置的。

而 **reset --hard HEAD^** 之所以起到了撤销 **commit** 的效果，是因为它把 **HEAD** 和它所指向的 branch 一起移动到了当前 **commit** 的父 **commit** 上，从而起到了「撤销」的效果：

![img](https:////upload-images.jianshu.io/upload_images/4428238-6dbab74ae9ad2e1f?imageMogr2/auto-orient/strip|imageView2/2/w/466/format/webp)

git reset

Git 的历史只能往回看，不能向未来看，所以把 **HEAD** 和 **branch** 往回移动，就能起到撤回 **commit** 的效果。

所以同理，**reset --hard** 不仅可以撤销提交，还可以用来把 **HEAD** 和 **branch** 移动到其他的任何地方。



```shell
git reset --hard branch2
```

![img](https:////upload-images.jianshu.io/upload_images/4428238-71f7141a3878da7e?imageMogr2/auto-orient/strip|imageView2/2/w/434/format/webp)

git reset --hard branch2

### reset三种模式区别和使用场景

#### 区别：

1. **--hard**：重置位置的同时，直接将 **working Tree工作目录**、 **index 暂存区**及 **repository** 都重置成目标**Reset**节点的內容,所以效果看起来等同于清空暂存区和工作区。
2. **--soft**：重置位置的同时，保留**working Tree工作目录**和**index暂存区**的内容，只让**repository**中的内容和 **reset** 目标节点保持一致，因此原节点和**reset**节点之间的【差异变更集】会放入**index暂存区**中(**Staged files**)。所以效果看起来就是工作目录的内容不变，暂存区原有的内容也不变，只是原节点和**Reset**节点之间的所有差异都会放到暂存区中。
3. **--mixed（默认）**：重置位置的同时，只保留**Working Tree工作目录**的內容，但会将 **Index暂存区** 和 **Repository** 中的內容更改和reset目标节点一致，因此原节点和**Reset**节点之间的【差异变更集】会放入**Working Tree工作目录**中。所以效果看起来就是原节点和**Reset**节点之间的所有差异都会放到工作目录中。

#### 使用场景:

1. **--hard**：(1) **要放弃目前本地的所有改变時**，即去掉所有add到暂存区的文件和工作区的文件，可以执行 **git reset -hard HEAD** 来强制恢复git管理的文件夹的內容及状态；(2) **真的想抛弃目标节点后的所有commit**（可能觉得目标节点到原节点之间的commit提交都是错了，之前所有的commit有问题）。
2. **--soft**：原节点和**reset**节点之间的【差异变更集】会放入**index暂存区**中(**Staged files**)，所以假如我们之前工作目录没有改过任何文件，也没add到暂存区，那么使用**reset  --soft**后，我们可以直接执行 **git commit** 將 index暂存区中的內容提交至 **repository** 中。为什么要这样呢？这样做的使用场景是：假如我们想合并「当前节点」与「**reset**目标节点」之间不具太大意义的 **commit** 记录(可能是阶段性地频繁提交,就是开发一个功能的时候，改或者增加一个文件的时候就**commit**，这样做导致一个完整的功能可能会好多个**commit**点，这时假如你需要把这些**commit**整合成一个**commit**的时候)時，可以考虑使用**reset  --soft**来让 **commit** 演进线图较为清晰。总而言之，**可以使用--soft合并commit节点**。
3. **--mixed（默认）**：(1)使用完**reset --mixed**后，我們可以直接执行 **git add** 将這些改变果的文件內容加入 **index 暂存区**中，再执行 **git commit** 将 **Index暂存区** 中的內容提交至**Repository**中，这样一样可以达到合并**commit**节点的效果（与上面--soft合并commit节点差不多，只是多了git add添加到暂存区的操作）；(2)移除所有Index暂存区中准备要提交的文件(Staged files)，我们可以执行 **git reset HEAD** 来 **Unstage** 所有已列入 **Index暂存区** 的待提交的文件。(有时候发现add错文件到暂存区，就可以使用命令)。(3)**commit**提交某些错误代码，或者没有必要的文件也被**commit**上去，不想再修改错误再**commit**（因为会留下一个错误**commit**点），可以回退到正确的**commit**点上，然后所有原节点和**reset**节点之间差异会返回工作目录，假如有个没必要的文件的话就可以直接删除了，再**commit**上去就OK了。

#### 假如手贱，又想回退撤销的版本呢?

请看另外一篇文章:TODO

##### 参考文章：

https://dotblogs.com.tw/wasichris/2016/04/29/225157
 https://www.domon.cn/2018/09/06/Git-reset-used-in-coding/
 https://juejin.im/book/5a124b29f265da431d3c472e/section/5a14529bf265da43310d7351(掘金小册)



作者：carway
链接：https://www.jianshu.com/p/c2ec5f06cf1a
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。





最近发现git在修改完文件后，提示恢复修改的命令是restore，如下所示，印象中应该是checkout，所以就研究了下，总结一下分享给大家

```bash
$ git status

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   _posts/git/2020-8-24-how-to-transfer-a-git-repo.md
```

原来是git中的checkout命令承载了分支操作和文件恢复的部分功能，有点复杂，并且难以使用和学习，所以社区解决将这两部分功能拆分开，在git 2.23.0中引入了两个新的命令switch和restore用来取代checkout

下面分别来说说分支操作和文件恢复，如果你对git还不太熟悉，可以先阅读我的git入门文章

- [起底Git](https://link.zhihu.com/?target=https%3A//yanhaijing.com/git/2017/01/19/deep-git-0/)
- [我的git笔记](https://link.zhihu.com/?target=https%3A//yanhaijing.com/git/2014/11/01/my-git-note/)
- [图解4种git合并分支方法](https://link.zhihu.com/?target=https%3A//yanhaijing.com/git/2017/07/14/four-method-for-git-merge/)

## 分支操作

原来git有两个命令用来操作分支，分别是branch和checkout

其中branch用来管理分支

```bash
$ git branch ## 查看当前所在分支 
$ git branch aaa # 新建分支aaa $ git branch -d aaa # 删除分支aaa
```

checkout用来切换分支，切换分支时，也可以新建分支

由于git中分支仅仅是一个commit id的别名，所以checkout也可以切换到一个commit id

```bash
$ git checkout aaa # 切换到 aaa分支 
$ git checkout -b aaa # 创建aaa，然后切换到 aaa分支 
$ git checkout commitid # 切换到某个commit id
```

checkout命令会用仓库中的文件，覆盖索引区(staged or index)和工作目录(work tree)

新的switch命令用来接替checkout的功能，但switch不能切换到commit id

```bash
$ git switch aaa # 切换到 aaa分支 
$ git switch -c aaa # 创建aaa，然后切换到 aaa分支
```

下面来总结对比下

![img](https://pic2.zhimg.com/80/v2-6604a9bcd7f748b9364a9919b07dceb1_1440w.jpg)

## 文件恢复

原来git中文件恢复涉及到两个命令，一个是checkout，一个是reset，reset除了重置分支之外，还提供了恢复文件的能力

```bash
$ git checkout -- aaa # 从staged中恢复aaa到worktree 
$ git reset -- aaa # 从repo中恢复aaa到staged 
$ git checkout -- HEAD aaa # 从repo中恢复aaa到staged和worktree 
$ git reset --hard -- aaa # 同上
```

一图胜千言系列



![img](https://pic2.zhimg.com/80/v2-482642d1fdbc31260e371779659de165_1440w.jpg)

新的restore命令专门用来恢复staged和worktree的文件

```bash
$ git restore [--worktree] aaa # 从staged中恢复aaa到worktree 
$ git restore --staged aaa # 从repo中恢复aaa到staged 
$ git restore --staged --worktree aaa # 从repo中恢复aaa到staged和worktree 
$ git restore --source dev aaa # 从指定commit中恢复aaa到worktree
```

一图胜千言系列



![img](https://pic4.zhimg.com/80/v2-16403835357dcc65a21cf63406bae907_1440w.jpg)



可以看到restore提供checkout和reset两个命令才能提供的文件恢复能力，也提供了更好的语义

## 总结

大家可以继续使用自己熟悉的命令，毕竟熟悉的才好用，本文希望能够帮助大家理清这个问题，满足大家的好奇心

git这样一个成熟的项目，一直处在不停的演进进化中，我们作为开发者也要保持持续学习演进的能力，共勉









```
git checkout readme.txt
```

以上代码可以将暂存区中的readme.txt文件还原到工作区，如果要还原多个文件，那么使用空格分隔：如果要还原所有文件，checkout后面跟点即可（.）：

特别说明：如果checkout后面是文件名称，以下写法更为稳妥：

```
 
```

[Shell] *纯文本查看* *复制代码*

| 1    | `$ git checkout -- readme.txt` |
| ---- | ------------------------------ |
|      |                                |

文件名称前面有两个横杠，并且中间采用空格分隔（否则报错）。此种方式可以防止Git出现误判，加入暂存区有一个文件名为ant（没有后缀名），恰好当前项目也有有个名为ant的分支，这个时候Git会优先将ant当做分支处理，于是就有可能导致错误。





我们也可以将其他分支的指定提交内容还原到当前分支工作区，首先做一下说明：checkout后面仅仅跟着分支名称，那么它的功能是切换分支。

```
 
```

[Shell] *纯文本查看* *复制代码*

| 1    | `$ git checkout Develop -- readme.txt` |
| ---- | -------------------------------------- |
|      |                                        |

如果分支后面跟着文件路径，那么就是将对应分支中的文件还原到当前分支的工作区。





用指定commit提交的内容覆盖工作区:

git checkout 6c89271 -- readme.txt





现在我们用Develop分支的指定commit提交的文件来覆盖master分支工作区：

```
 
```

[Shell] *纯文本查看* *复制代码*

| 1    | `$ git checkout Develop -- readme.txt` |
| ---- | -------------------------------------- |
|      |                                        |















## Level 3

