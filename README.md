## 目录

[hadoop基础](https://github.com/RogerGold/hadoop/blob/master/Hadoop_basic_0.md)

[HDFS](https://github.com/RogerGold/hadoop/blob/master/The%20Hadoop%20Distributed%20File%20System.md)

[WordCount实例](https://github.com/RogerGold/hadoop/blob/master/Hadoop_WordCount.md)

## Git

1.安装和配置用户信息
用户信息
当安装完 Git 应该做的第一件事就是设置你的用户名称与邮件地址。 这样做很重要，因为每一个 Git 的提交都会使用这些信息，并且它会写入到你的每一次提交中，不可更改：

    $ git config --global user.name "John Doe"
    $ git config --global user.email johndoe@example.com

你可以通过输入 git config <key>： 来检查 Git 的某一项配置

    $ git config user.name
    John Doe

检查配置信息
如果想要检查你的配置，可以使用 git config --list 命令来列出所有 Git 当时能找到的配置。

在现有目录中初始化仓库
如果你打算使用 Git 来对现有的项目进行管理，你只需要进入该项目目录并输入：

    $ git init


2.新建README.txt
    …or create a new repository on the command line

    echo "# hadoop" >> README.md
    git init
    git add README.md
    git commit -m "first commit"
    git remote add origin https://github.com/RogerGold/hadoop.git
    git push -u origin master
    …or push an existing repository from the command line

    git remote add origin https://github.com/RogerGold/hadoop.git
    git push -u origin master


3.常用命令

检查当前文件状态
要查看哪些文件处于什么状态，可以用 git status 命令。

查看提交历史
在提交了若干更新，又或者克隆了某个项目之后，你也许想回顾下提交历史。 完成这个任务最简单而又有效的工具是 git log 命令。

撤消操作
你提交后发现忘记了暂存某些需要的修改，可以像下面这样操作：

    $ git commit -m 'initial commit'
    $ git add forgotten_file
    $ git commit --amend
    
最终你只会有一个提交 - 第二次提交将代替第一次提交的结果。

从远程仓库中抓取与拉取
如果想查看你已经配置的远程仓库服务器，可以运行 git remote 命令。 它会列出你指定的每一个远程服务器的简写。
运行 git pull [remote-name]通常会从最初克隆的服务器上抓取数据并自动尝试合并到当前所在的分支。

ref:[git book](https://git-scm.com/book/zh/v2)
