# 玩不转的 GitHub

写这篇并不是详细的去写一下关于版本控制和 Git 使用的详细教程，而是整理一下 Git 入门、GitHub 常规使用、Gitee 常规使用以及在工作中常见的一些操作。

现在网上关于 Git 和 GitHub 的资源很多，高质量的也很多，比如

- https://www.imooc.com/learn/1278 【慕课网视频  2 小时】
- https://git-scm.com/book/zh/v2 【官网文档 中文版】
- https://gitee.com/help/categories/43 【码云文档 中文版】
- https://www.liaoxuefeng.com/wiki/896043488029600 【廖雪峰文档 中文版】

如果再从头开始写相关的文档，不过是狗尾续貂或者搬运加工罢了，那这里只介绍一下 Git的一些基本功能以及工作中遇到的问题，不在详细赘述。

正文开始：

Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的分布式版本控制系统。

自诞生于 2005 年以来，Git 日臻成熟完善，在高度易用的同时，仍然保留着初期设定的目标。它的速度飞快，极其适合管理大项目，它还有着令人难以置信的非线性分支管理系统，可以应付各种复杂的项目开发需求。尽管最初 Git 的开发是为了辅助 Linux 内核开发的过程，但是我们已经发现在很多其他自由软件项目中也使用了 Git。对于大部分公司 Git 已经变成了代码管理的首选方案，所以学习 Git 是很重要的。

如果你对 Git 一无所知，建议先通过上面的四种方式的任意一种来学习如何使用 Git 进行文件的管理。



# 一、安装和配置

Git 的安装相当简单，直接在 https://git-scm.com/downloads 下载安装即可，具体的安装过程中，可以观看具体的教程。

## 配置

Git 的配置分为系统配置、全局配置、当前仓库配置以及 worktree 配置，其中直接使用  `git config`  会将设置保存在当前仓库的 `.git/config` 文件中，只对当前仓库有效，使用 `git config --global` 进行配置则对当前用户下的所有仓库有效，使用 `git config --system` 则对当前系统下的所有用户有效。其中个人开发中，常用的为  `git config`  和  `git config --global` 

### 用户信息配置

第一个要配置的是你个人的用户名称和电子邮件地址。这两条配置很重要，每次 Git 提交时都会引用这两条信息，说明是谁提交了更新，所以会随更新内容一起被永久纳入历史记录（后期即使再修改，也不能改变原来的记录）：

比如要设置一个全局有效的配置：

```sh
$ git config --global user.name "zhangyunan"
$ git config --global user.email "zyndev@gmail.com"
```

如果你想不同的仓库采用不同的用户信息，可以分别在不同的仓库下面进行不同的配置，比如在 A 公司你可能使用 zhangyunan@a.com 则可以在对应的 A 公司的仓库下设置用户名

```sh
$ git config user.name "zhangyunan"
$ git config user.email "zhangyunan@a.com"
```

在别的仓库上就可以再设置其他的用户信息

如果用了 `--global` 选项，那么更改的配置文件就是位于你用户主目录下的那个，以后你所有的仓库都会默认使用这里配置的用户信息。如果要在某个特定的仓库中使用其他名字或者邮箱，只要去掉 `--global` 选项重新配置即可，新的设定保存在当前仓库的 `.git/config` 文件里。在 Windows 上是隐藏文件夹，可以通过展示隐藏文件来查看；在 Linux/Mac 上可以使用 `ls -a` 查看。

**总结**

- git config --system：操作 `/etc/gitconfig` 文件，全局的，对所有用户都生效
- git config --global： 操作 `用户目录/.gitconfig` 文件，仅仅对自己生效
- git config：操作仓库 `.git/config` 文件，只对当前仓库有效
- 通过上面的示例也可以推测出 git 配置的优先级为 **local > global > system**

### 查看配置

要查看已有的配置信息，可以使用 `git config --list` 命令：

```shell
$ git config --list
user.email=zyndev@gmail.com
user.name=zhangyunan
core.autocrlf=input
core.excludesfile=/Users/zhangyunan/.gitignore_global
core.quotepath=false
core.ignorecase=false
```

也可以使用  `git config --global --list`  来查看一些全局的配置，如果想单独查看指定的配置可以使用 `git config 配置名称`

```shell
$ git config user.name
zhangyunan
```

更多示例

- git config --system --list  系统级别
- git config --global --list   用户级别
- git config --list  当前仓库级别

### 一些常见配置项说明

| 配置名称           | 值示例                                    | 配置说明                                                     |
| ------------------ | ----------------------------------------- | ------------------------------------------------------------ |
| user.name          | 个人姓名，如 zhangyunan                   | 代码提交时用来展示谁提交的                                   |
| user.email         | 个人邮箱或者公司邮箱，如 zyndev@gmail.com | 代码提交时用来展示谁提交的                                   |
| core.quotepath     | false                                     | 是否转移路径，如果开启，会碰到有一些中文文件名或者路径被转义成\xx\xx\xx |
| core.ignorecase    | false                                     | 是否对**文件名**大小写敏感，建议 false，因为在开发中出现修改文件名大小写的情况还是很常见的，如果为 true 则不跟踪文件名大小写改动。 |
| pull.rebase        | false                                     | 在使用 pull 时，是使用 rebase 进行合并还是使用 merge 进行合并，建议 false，不要在默认情况下破坏提交记录，除非你知道在干什么。关于 merge 和 rebase 的区别，在后面的时候会讲一下 |
| init.defaultbranch | master                                    | 仓库创建时，默认的分支名称，默认是 master, 也可以改成其他名称 |
| commit.template    | 文件路径                                  | 提交代码时提供一个message 的模板信息，默认是一个空文件       |
| core.excludesfile  | 文件路径（默认是在用户目录下）            | git 的全局的忽略文件配置，对所有的仓库有效                   |
|                    |                                           |                                                              |

配置还有一些，具体的可以看 [官网](https://git-scm.com/book/zh/v2/%E8%87%AA%E5%AE%9A%E4%B9%89-Git-%E9%85%8D%E7%BD%AE-Git)

### 一些建议配置

#### git 修改文件夹/文件名大小写

```shell
$ git config --global core.ignorecase false
```

给配置默认是 true，也就是 Git 会忽略掉关于文件夹和文件名的大小写修改，比如我们将文件夹从 `ABC` 改为 `abc` 时，Git 不会认为发生了变化了，这在日常的开发中还是有问题，比如你把一些大小写字母拼错了，导致不符合某些命名规范，这时你想改正过来，发现 Git 并没有跟踪这个变化，你是不是很崩溃，这里建议提前进行设置

#### pull 代码使用 merge

最新的 Git 中增加 `pull.rebase` 配置，让你来决定默认 pull 时是使用 rebase 还是 merge，当你不进行手动设置的时候会使用 merge 进行合并，但是会每次都提示进行配置，这里建议你设置成  false

```shell
git config --global pull.rebase false
```

### 清除指定的配置

对于不想进行配置或者需要清除的配置的可以使用 `unset` 命令

```shell
$ git config --unset --local user.name 
$ git config --unset --global user.name 
$ git config --unset --system user.name
```

这样就可以清除对应级别的配置项



## 别名的配置

有个=一些 git 命令很长，可以通过起别名来缩短命令。
例如：

```
给 commit 起个别名为 ci 则使用 git ci 等同于 git commit
git config --global alias.ci commit
给 status 起个别名为 ci 则使用 git st 等同于 git status
git config --global alias.st status
给 branch 起个别名为 ci 则使用 git br 等同于 git branch
git config --global alias.br branch
```

这种方式对于组合长命令来说很有帮助，例如：

```
git config --global alias.graph "log --online --graph"
```

> 命令如果出现空格需要用引号引起来



## 查看帮助

若你使用 Git 时需要获取帮助，有三种方法可以找到 Git 命令的使用手册：

```bash
$ git help <verb>
$ git <verb> --help
$ man git-<verb>
```

例如，要想获得 config 命令的手册，执行

```
$ git help config
```

# 常见问题

为了演示方便，这里新建了一个名为 git-demo 的仓库，对应的 github 为 https://github.com/useless-project/git-demo

## 使用 git commit --amend 来修改最后一次提交

**情景：**最近产品又改需求了，这已经是今天的第 3 次了，小明在已经千疮百孔的代码上又加上一个大坑，终于完成了这次的改动，心中怨气横生，顺手提交了代码并写到 "再 TMD 改，老子不干了"

```shell
$ git commit -am "再 TMD 改，老子不干了"
```

此时使用 git log 可以看到

```shell
$ git log --oneline
3130f04 (HEAD -> main) 再 TMD 改，老子不干了
bbcc6ee (origin/main, origin/HEAD) Initial commit
```

吐槽归吐槽，生活还得继续呀！这时就需要修改掉提交信息，再重新提交一下 `git commit --amend` 的用途就来了

> The git commit –amend command lets you **modify your last commit**. You can change your log message and the files that appear in the commit. 

修改你最近一次提交可能是所有修改历史提交的操作中最常见的一个。 对于你的最近一次提交，你往往想做两件事情：简单地修改提交信息， 或者通过添加、移除或修改文件来更改提交实际的内容。

如果，你只是想修改最近一次提交的提交信息，那么很简单：

```shell
$ git commit --amend
```

上面这条命令会将最后一次的提交信息载入到编辑器中供你修改。 当保存并关闭编辑器后，编辑器会将更新后的提交信息写入新提交中，它会成为新的最后一次提交。

另一方面，如果你想要修改最后一次提交的实际内容，那么流程很相似：首先作出你想要补上的修改， 暂存它们，然后用 `git commit --amend` 以新的改进后的提交来 **替换** 掉旧有的最后一次提交，

我们使用 `git commit -amend` 来修改下最后一次提交

```shell
$ git commit --amend -m  "这次的改动很符合实际应用情况,果然还是产品了解需求"
```

此时使用 git log 可以看到

```shell
$ git log --oneline
4be98a7 (HEAD -> main) 这次的改动很符合实际应用情况,果然还是产品了解需求
bbcc6ee (origin/main, origin/HEAD) Initial commit
```

> 这一次你好像遭受到了社会的毒打



## 使用 git rebase -i 来修改多次提交历史

小明吐槽的太多了，导致只修改最后一次不管用了。

```shell
$ git log --oneline

75bcb30 (HEAD -> main) 再 TMD 改，老子不干了 x4
2d6b84e 再 TMD 改，老子不干了 x3
e21c6f8 再 TMD 改，老子不干了 x2
875613d 再 TMD 改，老子不干了 x1
4be98a7 这次的改动很符合实际应用情况,果然还是产品了解需求
bbcc6ee (origin/main, origin/HEAD) Initial commit
```

为了修改在提交历史中较远的提交，必须使用更复杂的工具。 Git 没有一个改变历史工具，但是可以使用变基工具来变基一系列提交，基于它们原来的 HEAD 而不是将其移动到另一个新的上面。 通过交互式变基工具，可以在任何想要修改的提交后停止，然后修改信息、添加文件或做任何想做的事情。 可以通过给 `git rebase` 增加 `-i` 选项来交互式地运行变基。 必须指定想要重写多久远的历史，这可以通过告诉命令将要变基到的提交来做到。

例如，如果想要修改最近四次提交信息，或者那组提交中的任意一个提交信息， 将想要修改的最近一次提交的父提交作为参数传递给 `git rebase -i` 命令，即 `HEAD~3^` 或 `HEAD~4`。 记住 `~4` 可能比较容易，因为你正尝试修改最后四次提交；但是注意实际上指定了以前的五次提交，即想要修改提交的父提交：

```console
$ git rebase -i HEAD~4
```

再次记住这是一个变基命令——在 `HEAD~4..HEAD` 范围内的每一个修改了提交信息的提交及其 **所有后裔** 都会被重写。 不要涉及任何已经推送到中央服务器的提交——这样做会产生一次变更的两个版本，因而使他人困惑。

运行这个命令会在文本编辑器上给你一个提交的列表，看起来像下面这样：

```console
pick 875613d 再 TMD 改，老子不干了 x1
pick e21c6f8 再 TMD 改，老子不干了 x2
pick 2d6b84e 再 TMD 改，老子不干了 x3
pick 75bcb30 再 TMD 改，老子不干了 x4

# Rebase 4be98a7..75bcb30 onto 4be98a7 (4 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup <commit> = like "squash", but discard this commit's log message
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
# .       create a merge commit using the original merge commit's
# .       message (or the oneline, if no original merge commit was
# .       specified). Use -c <commit> to reword the commit message.
```

需要重点注意的是相对于正常使用的 `log` 命令，这些提交显示的顺序是相反的。 

注意其中的反序显示。 交互式变基给你一个它将会运行的脚本。 它将会从你在命令行中指定的提交（`HEAD~4`）开始，从上到下的依次重演每一个提交引入的修改。 它将最旧的而不是最新的列在上面，因为那会是第一个将要重演的。

如果只想修改 commit 信息的话，可以将前面的 pick 改为 r 或者 reword 即可，修改方式是按照 vim 编辑器的命令来的，这里通过 vim 的命令将文本进行编辑如下：

```
r 875613d 再 TMD 改，老子不干了 x1
r e21c6f8 再 TMD 改，老子不干了 x2
r 2d6b84e 再 TMD 改，老子不干了 x3
r 75bcb30 再 TMD 改，老子不干了 x4

下面的省略了，之后 wq 保存并退出
```

当保存并退出编辑器时，Git 将你带回到列表中的最后一次提交，把你送回命令行并提示以下信息：

```
再 TMD 改，老子不干了 x1

下面的省略了
```

在这里同样是需要用 vim 命令来修改的，把 message 修改完成后同样 wq 保存退出，会继续提示 `再 TMD 改，老子不干了 x2` 的内容，依次修改即可，最后效果，这样就把多次吐槽的 message 修改完毕了

```shell
$ git log --oneline

57616cd (HEAD -> main) 水光潋滟晴方好，山色空蒙雨亦奇
0aeb8dd 自测完成
a8e5c7f 完成自测
ca7c292 好好学习
4be98a7 这次的改动很符合实际应用情况,果然还是产品了解需求
bbcc6ee (origin/main, origin/HEAD) Initial commit
```

通过命令的提示可以看出 `git rebase -i` 下面有多个修改选项可以使用，通过不同的选项来达到不同的修改目的：

- p, pick <commit> = use commit
- r, reword <commit> = use commit, but edit the commit message
- e, edit <commit> = use commit, but stop for amending
- s, squash <commit> = use commit, but meld into previous commit
- f, fixup <commit> = like "squash", but discard this commit's log message
- x, exec <command> = run command (the rest of the line) using shell
- b, break = stop here (continue rebase later with 'git rebase --continue')
- d, drop <commit> = remove commit
- l, label <label> = label current HEAD with a name
- t, reset <label> = reset HEAD to a label
- m, merge [-C <commit> | -c <commit>] <label> [# <oneline>] create a merge commit using the original merge commit's message (or the oneline, if no original merge commit was specified). Use -c <commit> to reword the commit message.



### 重新排序提交

也可以使用交互式变基来重新排序或完全移除提交。 如果想要移除 “added cat-file” 提交然后修改另外两个提交引入的顺序，可以将变基脚本从这样：

```console
pick f7f3f6d changed my name a bit
pick 310154e updated README formatting and added blame
pick a5f4a0d added cat-file
```

改为这样：

```console
pick 310154e updated README formatting and added blame
pick f7f3f6d changed my name a bit
```

当保存并退出编辑器时，Git 将你的分支带回这些提交的父提交，应用 `310154e` 然后应用 `f7f3f6d`，最后停止。 事实修改了那些提交的顺序并完全地移除了 “added cat-file” 提交。

### 压缩提交

通过交互式变基工具，也可以将一连串提交压缩成一个单独的提交。 在变基信息中脚本给出了有用的指令：

```console
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup <commit> = like "squash", but discard this commit's log message
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
# .       create a merge commit using the original merge commit's
# .       message (or the oneline, if no original merge commit was
# .       specified). Use -c <commit> to reword the commit message.
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

如果，指定 “squash” 而不是 “pick” 或 “edit”，Git 将应用两者的修改并合并提交信息在一起。 所以，如果想要这三次提交变为一个提交，可以这样修改脚本：

```console
pick f7f3f6d changed my name a bit
squash 310154e updated README formatting and added blame
squash a5f4a0d added cat-file
```

当保存并退出编辑器时，Git 应用所有的三次修改然后将你放到编辑器中来合并三次提交信息：

```console
# This is a combination of 3 commits.
# The first commit's message is:
changed my name a bit

# This is the 2nd commit message:

updated README formatting and added blame

# This is the 3rd commit message:

added cat-file
```

当你保存之后，你就拥有了一个包含前三次提交的全部变更的提交。

### 拆分提交

拆分一个提交会撤消这个提交，然后多次地部分地暂存与提交直到完成你所需次数的提交。 例如，假设想要拆分三次提交的中间那次提交。 想要将它拆分为两次提交：第一个 “updated README formatting”，第二个 “added blame” 来代替原来的 “updated README formatting and added blame”。 可以通过修改 `rebase -i` 的脚本来做到这点，将要拆分的提交的指令修改为 “edit”：

```console
pick f7f3f6d changed my name a bit
edit 310154e updated README formatting and added blame
pick a5f4a0d added cat-file
```

然后，当脚本带你进入到命令行时，重置那个提交，拿到被重置的修改，从中创建几次提交。 当保存并退出编辑器时，Git 带你到列表中第一个提交的父提交，应用第一个提交（`f7f3f6d`）， 应用第二个提交（`310154e`），然后让你进入命令行。 那里，可以通过 `git reset HEAD^` 做一次针对那个提交的混合重置，实际上将会撤消那次提交并将修改的文件取消暂存。 现在可以暂存并提交文件直到有几个提交，然后当完成时运行 `git rebase --continue`：

```console
$ git reset HEAD^
$ git add README
$ git commit -m 'updated README formatting'
$ git add lib/simplegit.rb
$ git commit -m 'added blame'
$ git rebase --continue
```

Git 在脚本中应用最后一次提交（`a5f4a0d`），历史记录看起来像这样：

```console
$ git log -4 --pretty=format:"%h %s"
1c002dd added cat-file
9b29157 added blame
35cfb2b updated README formatting
f3cc40e changed my name a bit
```

再次强调，这些改动了所有在列表中的提交的 SHA-1 校验和，所以要确保列表中的提交还没有推送到共享仓库中。

### 核武器级选项：filter-branch

有另一个历史改写的选项，如果想要通过脚本的方式改写大量提交的话可以使用它——例如，全局修改你的邮箱地址或从每一个提交中移除一个文件。 这个命令是 `filter-branch`，它可以改写历史中大量的提交，除非你的项目还没有公开并且其他人没有基于要改写的工作的提交做的工作，否则你不应当使用它。 然而，它可以很有用。 你将会学习到几个常用的用途，这样就得到了它适合使用地方的想法。

| Caution | `git filter-branch` 有很多陷阱，不再推荐使用它来重写历史。 请考虑使用 `git-filter-repo`，它是一个 Python 脚本，相比大多数使用 `filter-branch` 的应用来说，它做得要更好。它的文档和源码可访问 https://github.com/newren/git-filter-repo 获取。 |
| ------- | ------------------------------------------------------------ |
|         |                                                              |

#### 从每一个提交中移除一个文件

这经常发生。 有人粗心地通过 `git add .` 提交了一个巨大的二进制文件，你想要从所有地方删除。 可能偶然地提交了一个包括一个密码的文件，然而你想要开源项目。 `filter-branch` 是一个可能会用来擦洗整个提交历史的工具。 为了从整个提交历史中移除一个叫做 `passwords.txt` 的文件，可以使用 `--tree-filter` 选项给 `filter-branch`：

```console
$ git filter-branch --tree-filter 'rm -f passwords.txt' HEAD
Rewrite 6b9b3cf04e7c5686a9cb838c3f36a8cb6a0fc2bd (21/21)
Ref 'refs/heads/master' was rewritten
```

`--tree-filter` 选项在检出项目的每一个提交后运行指定的命令然后重新提交结果。 在本例中，你从每一个快照中移除了一个叫作 `passwords.txt` 的文件，无论它是否存在。 如果想要移除所有偶然提交的编辑器备份文件，可以运行类似 `git filter-branch --tree-filter 'rm -f *~' HEAD` 的命令。

最后将可以看到 Git 重写树与提交然后移动分支指针。 通常一个好的想法是在一个测试分支中做这件事，然后当你决定最终结果是真正想要的，可以硬重置 `master` 分支。 为了让 `filter-branch` 在所有分支上运行，可以给命令传递 `--all` 选项。

#### 使一个子目录做为新的根目录

假设已经从另一个源代码控制系统中导入，并且有几个没意义的子目录（`trunk`、`tags` 等等）。 如果想要让 `trunk` 子目录作为每一个提交的新的项目根目录，`filter-branch` 也可以帮助你那么做：

```console
$ git filter-branch --subdirectory-filter trunk HEAD
Rewrite 856f0bf61e41a27326cdae8f09fe708d679f596f (12/12)
Ref 'refs/heads/master' was rewritten
```

现在新项目根目录是 `trunk` 子目录了。 Git 会自动移除所有不影响子目录的提交。

#### 全局修改邮箱地址

另一个常见的情形是在你开始工作时忘记运行 `git config` 来设置你的名字与邮箱地址， 或者你想要开源一个项目并且修改所有你的工作邮箱地址为你的个人邮箱地址。 任何情形下，你也可以通过 `filter-branch` 来一次性修改多个提交中的邮箱地址。 需要小心的是只修改你自己的邮箱地址，所以你使用 `--commit-filter`：

```console
$ git filter-branch --commit-filter '
        if [ "$GIT_AUTHOR_EMAIL" = "schacon@localhost" ];
        then
                GIT_AUTHOR_NAME="Scott Chacon";
                GIT_AUTHOR_EMAIL="schacon@example.com";
                git commit-tree "$@";
        else
                git commit-tree "$@";
        fi' HEAD
```

这会遍历并重写每一个提交来包含你的新邮箱地址。 因为提交包含了它们父提交的 SHA-1 校验和，这个命令会修改你的历史中的每一个提交的 SHA-1 校验和， 而不仅仅只是那些匹配邮箱地址的提交。



**快救救我。我** 将测试分支 ，合并到了 自己的分支（将别人不上线的），然后将自己的分支合并到了 stating 然后推送了staing，现在怎么恢复。。因为stating出现了测试分支的代码。这些不应该出现，

