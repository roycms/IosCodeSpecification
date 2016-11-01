# Git基本使用

## 配置用户信息
Git 提供了一个叫做 git config 的工具，专门用来配置或读取相应的工作环境变量。而正是由这些环境变量，决定了 Git 在各个环节的具体工作方式和行为。这些变量可以存放在以下三个不同的地方：

* `/etc/gitconfig` 文件：系统中对所有用户都普遍适用的配置。若使用 `git config` 时用 `--system` 选项，读写的就是这个文件。
* `~/.gitconfig` 文件：用户目录下的配置文件只适用于该用户。若使用 `git config` 时用 `--global` 选项，读写的就是这个文件。
* 当前项目的 Git 目录中的配置文件（也就是工作目录中的 `.git/config` 文件）：这里的配置仅仅针对当前项目有效。每一个级别的配置都会覆盖上层的相同配置，所以 `.git/config` 里的配置会覆盖 `/etc/gitconfig` 中的同名变量。

在 Windows 系统上，Git 会找寻用户主目录下的 `.gitconfig` 文件。主目录即 $HOME 变量指定的目录，一般都是 `C:\Documents and Settings\$USER`。此外，Git 还会尝试找寻 `/etc/gitconfig` 文件，只不过看当初 Git 装在什么目录，就以此作为根目录来定位。

**用户信息**

第一个要配置的是你个人的用户名称和电子邮件地址。这两条配置很重要，每次 Git 提交时都会引用这两条信息，说明是谁提交了更新，所以会随更新内容一起被永久纳入历史记录：

```git
$ git config --global user.name "输入你的名字"
$ git config --global user.email abc@example.com
$ git config [--global] core.editor <editor>　　　　　　　设置编辑器
$ git config [--global] github.user <user>　　　　　　 　 设置github帐号名
$ git config [--global] github.token <token>　　　　　　　设置github的token
```
--global是对当前系统用户的全局设置，在～/.gitconfig中。 对系统所有用户进行配置，/etc/gitconfig。对当前项目，.git/config

**查看配置信息**

要检查已有的配置信息，可以使用 ·git config --list· 命令：

``` git
$ git config --list
user.name=Scott Chacon
user.email=schacon@gmail.com
color.status=auto
color.branch=auto
color.interactive=auto
color.diff=auto
...
```
## .gitignore 文件针对oc的忽律配置

> 引用自 https://github.com/github/gitignore

```
##系统
.DS_Store
.Trashes

# Thumbnails
._*

##Xcode
build/
*.pbxuser
!default.pbxuser
*.mode1v3
!default.mode1v3
*.mode2v3
!default.mode2v3
*.perspectivev3
!default.perspectivev3
xcuserdata
*.xccheckout
*.moved-aside
DerivedData
*.hmap
*.ipa
*.xcuserstate

##CocoaPods
Pods/
```

## GIT基本命令

Git 创建库
```
git clone <url>　　　　　　　　　　　　　　　　　　　 ssh/http(s)/git三种协议，ssh和https可推送

git init　　　　　　　　　　　　　　　　　　　　　　  初始化Git仓库
```
Git 日常操作
```
git add <file>　　　　　　　　　　　　　　　  将文件加入index file

git rm [--cached]　　　　　　　　　　　　　   删除，加--cached表示仅从index file中删除文件，即放弃跟踪

git mv <src> <dest>　　　　　　　　　　　　   移动/更名

git diff --cached/--staged　　　　　　　　　　当前索引与上次提交（有哪些需要commit）

git diff　　　　　　　　　　　　　　　　　    当前索引与工作目录（有哪些需要add）

git diff HEAD[^]　　　　　　　　　　　　　　  工作目录与上次提交（当前目录与上次提交有何改变）

git commit [-a] -m <msg>　　　　　　　　　　  提交

git commit --amend [-m <msg>]　　　　　　　   修复上次提交

git reset HEAD <file>　　　　　　　　　　　   同--mixed，default option

git reset --mixed HEAD　　　　　　　　　　    撤销 commit 和index file,只保留 working tree 的信息

git reset --hard HEAD[^]　　　　　　　　　　  将 working tree 和 index file 都撤销到以前状态

git reset --soft HEAD[^]　　　　　　　　　　  只撤销 commit,而保留 working tree 和 index file 的信息

　　　　　　　　　　　　　　　　　　　　　    回复到某个状态。以git reset --soft HEAD为例，commit回退到

　　　　　　　　　　　　　　　　　　　　　    HEAD（相当于无变化），若是HEAD^，则commit回退到HEAD^

git gc　　　　　　　　　　　　　　　　　　    用垃圾回收机制清除由于 reset 而造成的垃圾代码

git status　　　　　　　　　　　　　　　　　  显示当前工作目录状态

git log [-p]　　　　　　　　　　　　　　　    显示提交历史（many useful options to be learned）

git branch [branch]　　　　　　　　　　　　   显示/新建分支

git branch -d/-D　　　　　　　　　　　　　　  删除分支（d表示“在分支合并后删除分支”，D表示无论如何都删除分支）

git show-branch

git checkout <branch>　　　　　　　　　　　 切换分支（分支未commit无法切换）

git merge <branch>　　　　　　　　　　　　  合并分支

git merge == git pull .

git show <branch | commit | tag | etc>　　　     显示对应对象的信息

git grep <rep> [object]　　　　　　　　　　　  （在指定对象（历史记录）中）搜索　　　　　　　　

git cat-file 　　　　　　　　　　　　　　　　   查看数据

git cat-file <-t | -s | -e | -p | (type)> <object>        type can be one of: blob, tree, commit, tag

git ls-files [--stage]　　　　　　　　　　　　　 show information about files in the index and the working tree（实际是查看索引文件）

git watchchanged <since>..<until>　　　　　　   显示两个commit（当然也可以是branch）的区别

git remote [-v]　　　　　　　　　　　　　　     显示远程仓库，加-v选项可显示仓库地址

git remote add <repo_name> <url>　　　　　  　　添加远程仓库，repo_name为shortname，指代仓库地址

git remote rename <old_name> <new_name>  　　   更名

git remote rm <repo_name>　　　　　　　　　　   删除远程仓库

git remote show <repo_name>　　　　　　　　　   查看远程仓库信息

git remote fetch <repo_name>　　　　　　　　　  从远程仓库抓取数据（并不合并）

git pull <repo_name> <branch_name>　　　　　　  拉去数据并合并到当前分支

git push <repo_name> <branch_name>　　　　　    推送指定分支到指定仓库

git fetch <repo_name> <branch_name>[:<local_branch_name>]　　　　拉去数据，未合并
```
Git 标签
```
Git 相关环境变量

GIT_DIR: 如果指定了那么git init将会在GIT_DIR指定的目录下创建版本库

GIT_OBJECT_DIRECTORY: 用来指示对象存储目录的路径。即原来$GIT_DIR/objects下的文件会置于该变量指定的路径下

Git 常见变量

HEAD: 表示最近一次的 commit。

MERGE_HEAD: 如果是 merge 产生的 commit,那么它表示除 HEAD 之外的另一个父母分支。

FETCH_HEAD: 使用 git-fetch 获得的 object 和 ref 的信息都存储在这里,这些信息是为日后 git-merge 准备的。

HEAD^: 表示 HEAD 父母的信息

HEAD^^: 表示 HEAD 父母的父母的信息

HEAD~4: 表示 HEAD 上溯四代的信息

HEAD^1: 表示 HEAD 的第一个父母的信息

HEAD^2: 表示 HEAD 的第二个父母的信息

COMMIT_EDITMSG: 最后一次 commit 时的提交信息。
```
