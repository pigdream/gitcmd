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

一个文件的状态通常可以分为：

- 不受版本控制的 untracked 状态

- 受版本控制并且已修改的 modified 状态

- 受版本控制已修改并提交到暂存区的 staged 状态

- 从暂存区已经提交到本地仓库的 committed 状态

- 提交到本地仓库未修改或者从远程仓库克隆下来的 unmodified 状态

  

### **Git回退命令**

#### **git checkout**

这个命令又出现了，上次是总结 git branch 分支操作的时候，git checkout 可以用来新建或者切换分支，这次总结回退版本的命令，git checkout 也可以用来回退文件版本，很神奇吧。

其实这个命令的作用就是它单词的本义——检出，他的常用操作也取自这个意思，比如 git checkout branch_name 切换分支操作，实际上就是把指定分支在仓库中对应的所有文件检出来覆盖当前工作区，最终表现就是切换了分支。

而针对于文件的检出可以使用 git checkout -- file_name，当不指定 commit id 就是将暂存区的内容恢复到工作区，也就可以达到回退本地修改的作用。

不过，这个身兼数职的 git checkout 命令现在可以轻松一些了，从 Git 2.23 版本开始引入了两个新的命令： git switch 用来切换分支，git restore用来还原工作区的文件，这个后面还会提到。

#### **git revert**

revert 这个词的意思是：归还，复原，回退，它和后面即将提到的 restore 在意思上简直无法区分，为了区别他们两个这里可以把 git revert 看成归还的意思，对某次提交执行 git revert 命令就是对这次修改执行一个归还操作，其实就是反向再修改一次。

要理解 git revert 就要从反向修改的含义来看，当我们再一个文件中添加一行内容，并提交到版本库后，产生一个提交id——commit-id-a，如果这时使用 git revert commit-id-a 命令，就相当于在工作区中的那个文件将刚在新加的一行内容删除掉，然后再进行一个提交。

**注意，这个操作是会改变分支记录的，因为产生了新的提交。**

#### git restore

这个命令是 Git 2.23 版本之后新加的，用来分担之前 git checkout 命令的功能，作用就是用暂存区或者版本库中的文件覆盖本地文件的修改可以达到回退修改的目的，同时也可以使用版本库中的文件覆盖暂存区的文件，达到回退git add 命令的目的。

注意，这个操作是不会影响分支记录的，就是相当于之前的 git checkout 命令重新检出一份文件来覆盖本地的修改。

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

#### git reset

reset 重新设置的意思，其实就是用来设置分支的头部指向，当进行了一系列的提交之后，忽然发现最近的几次提交有问题，想从提交记录中删除，这是就会用到 git reset 命令，这个命令后面跟 commit id，表示当前分支回退到这个 commit id 对应的状态，之后的日志记录被删除，工作区中的文件状态根据参数的不同会恢复到不同的状态。

--soft: 被回退的那些版本的修改会被放在暂存区，可以再次提交。

--mixed: 默认选项，被回退的那些版本的修改会放在工作目录，可以先加到暂存区，然后再提交。

--hard: 被回退的那些版本的修改会直接舍弃，好像它们没有来过一样。

这样来看，git reset 命令好像是用来回退版本的，但是如果使用 git reset HEAD file_name 命令就可以将一个文件回退到 HEAD 指向版本所对应的状态，其实就是当前版本库中的状态，也就相当于还原了本地的修改。

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

其中 ^ 和 ~ 的含义并不相同，涉及到合并分支的概念

#### git rm

临时插播的命令，本来删除不能算是回退，但是如果它和某些命令反着来就是一种回退，比如对一个新文件使用 git add newfile_name 命令，然后再使用 git rm --cached newfile_name 就可以将这个文件从暂存区移除掉，但是在工作区里没有消失，如果不加 --cached 参数，就会从工作区和版本库暂存区同时删除，相当于执行了 rm newfile_name 和 git add new_file 两条命令。









#### 还原00：工作区中未加到暂存区和版本库的文件，还原今天所做的修改

实话实说，办不到，没有加到过暂存区就没有被追踪，它的任何修改是没有办法回退的，可是使用 Ctrl+Z 碰碰运气，没准就退回到了你想要的状态。

#### 还原01：工作区中未加到暂存区和版本库的文件，执行了 git add 操作

这种情况可以使用git rm --cached newfile、git restore --staged newfile 或者 git reset HEAD newfile 命令，使用后两个命令的时候不能是版本库的第一个文件。

#### 还原02：版本库中的文件，修改或删除后未执行 git add 操作

这种情况可以使用git restore file_name、git checkout -- file_name 或者 git reset --hard HEAD 命令，最后的git reset 命令带有 --hard 参数不能再加文件目录，只能将工作区全还原。

#### 还原03：版本库中的文件，修改或删除后执行了 git add 操作

使用了 git add 命令之后，文件的改变就放到了暂存区，这种情况可以使用git restore --staged file_name 或者 git reset HEAD file_name 命令。执行 git restore --staged file_name 实际上是使用版本库中的文件覆盖暂存区中的数据，执行结束后文件状态变成了 <还原02> 中的情况。git reset 命令如果加上 --hard 参数不能再加文件目录，只能将工作区全还原，如果不加默认参数为 --mixed，执行之后修改的文件状态变成了 <还原02> 中的情况。

#### 还原04：版本库中的文件，修改或删除后执行了 git add、git commit 操作

git commit 命令一旦执行了之后就形成了“历史”，我们叫做提交日志，要想回退就得有篡改历史的能力，很幸运 Git 给了我们这种能力，其实提交之后我们可以把本地文件反向修改，然后再提交一次，但是我们说的还原，一般都是只倒退，既然是错误的提交，我们就像把这段“历史”抹去，这时就要用到 git reset HEAD^ 命令。

执行这个命令之后，刚刚的提交记录就被抹掉了，文件状态就回到了 <还原02> 的情况，如果加上参数 --soft 就会回到 <还原03> 的情况，如果加上参数 --hard ，就不能添加 file_name 这个文件名，然后整个工作区倒退到上一次修改之前，其他两种参数 --mixed 和 --soft 就可以指定添加名字。

怎么样，历史被我们抹除了，需要注意的是，如果想还原“历史”，那么 git set 命令后面不能跟文件名，也就是说必须整个还原到上一版本，否则就相当于将单个文件简单反向修改添加到暂存区，而之前对文件的修改保留在本地，文件的日志并没有回退，具体的文件状态还得你自己操作感受一下。

#### 还原05：版本库中的文件，修改或删除后执行了 git add、git commit、git push 操作

这种情况就是还原远程仓库的日志记录了，实际上操作步骤先按照 <还原04> 来处理，然后将本地分支情况推送到远程分支即可。

#### 还原06：两次git commit 之后产生两条日志，只还原第一次提交

这种情况其实发生了两次修改和两次提交，和 <还原05> 情况不同的是要还原的提交不是最后一次，如果使用 git reset 命令必然将最后一次修改也还原了，虽然不能直接完成，但是给我们提供了解决问题的思路：

第一种方法：直接使用 git reset HEAD^^ 命令还原两次提交，然后在工作区将文件按第二次修改再改一次进行提交，这种方法适用于想要抹除第一次提交历史的情况。

第二种方法：如果你不在意提交历史，只是想还原第一次修改，那么可以使用 git revert HEAD^ 命令来反向修改那一次变化，修改之后会自动添加到暂存区，等待提交。



很多操作选择在使用 git status 之后都有列举
Git 2.23版本之后学会用 git switch 和 git restore 命令，因为之前 git checkout 背负了太多了
最后放一幅图吧，只画了主要的，没有画出全部情况，否则会很乱，可以对照着练习一下

![gitfilestate](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9jZG4uanNkZWxpdnIubmV0L2doL2FsYmVydGdpdGh1YmhvbWUvY2RuL2ltZy9naXRmaWxlc3RhdGUucG5n?x-oss-process=image/format,png,image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9hbGJlcnRnaXRodWJob21lLmdpdGh1Yi5pby9ibG9nL2Fib3V0,size_18,color_FFFFFF,t_70#pic_center)

1.git checkout – <file_name>是将暂存区的修改重新放回工作区，但只能操作文件内容，不能添加、删除文件；
2.git restore --staged <file_name>相当于撤销git add 命令，git restore <file_name> 是放弃对工作区的修改，对文件的操作（添加、删除）和文件内容的操作都能使用此命令；
3.而git reset HEAD <file_name>与 git restore --staged <file_name>的作用相同。

当执行提交操作（git commit）时，暂存区的目录树写到版本库（对象库）中，master 分支会做相应的更新。即 master 指向的目录树就是提交时暂存区的目录树。当执行 git reset HEAD 命令时，暂存区的目录树会被重写，被 master 分支指向的目录树所替换，但是工作区不受影响。当执行 git rm --cached <file> 命令时，会直接从暂存区删除文件，工作区则不做出改变。当执行 git checkout 或者git checkout -- <file>命令时，会用暂存区全部或指定的文件替换工作区的文件。这个操作很危险，会清除工作区中未添加到暂存区的改动。当执行 git checkout HEAD或者 git checkout HEAD <file>命令时，会用 HEAD 指向的 master 分支中的全部或者部分文件替换暂存区和以及工作区中的文件。这个命令也是极具危险性的，因为不但会清除工作区中未提交的改动，也会清除暂存区中未提交的改 动。













## Level 3

