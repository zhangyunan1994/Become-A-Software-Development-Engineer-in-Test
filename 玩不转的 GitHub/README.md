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

Git 的配置分为全局配置和当前仓库配置，其中直接使用  `git config`  会将设置保存在当前仓库的 `.git/config` 文件中，只对当前仓库有效，使用 `git config --global` 进行配置则全局有效。

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

> **总结**：如果用了 `--global` 选项，那么更改的配置文件就是位于你用户主目录下的那个，以后你所有的仓库都会默认使用这里配置的用户信息。如果要在某个特定的仓库中使用其他名字或者电邮，只要去掉 `--global` 选项重新配置即可，新的设定保存在当前仓库的 `.git/config` 文件里。在 Windows 上是隐藏文件夹，可以通过展示隐藏文件来查看；在 Linux/Mac 上可以使用 `ls -a` 查看。

### 查看配置

要查看已有的配置信息，可以使用 `git config --list` 命令：

```sh
$ git config --list
user.email=zyndev@gmail.com
user.name=zhangyunan
core.autocrlf=input
core.excludesfile=/Users/zhangyunan/.gitignore_global
core.quotepath=false
core.ignorecase=false
```

也可以使用  `git config --global --list`  来查看一些全局的配置，如果想单独查看指定的配置可以使用 `git config 配置名称`

```sh
$ git config user.name
zhangyunan
```

### 一些常见配置项说明

| 配置名称   | 值示例                                    | 配置说明   |
| ---------- | ----------------------------------------- | ---------- |
| user.name  | 个人姓名，如 zhangyunan                   | 代码提交时 |
| user.email | 个人邮箱或者公司邮箱，如 zyndev@gmail.com |            |
|            |                                           |            |





