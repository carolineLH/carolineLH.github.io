---
title: git
date: 2019-11-08 11:24:03
categories: 
  - git
  - 工具
tags: 
  - git
  - 工具
---

## 什么是Git？
 <!--more-->
* 被最广泛应用的现代软件管理系统
* github上成熟且活跃的开源社区
* 分布式架构
* 支持大多数的操作系统和IDE

### 对分布式架构的理解
* 分布式和集中式，典型代表Git/SVN
* 集中式，中心化的系统，有一个中心服务器，所有的操作都要基于中心服务器，一旦服务器挂了，或者没网了，开发者就没有办法提交
* 分布式，就是去中心化，每个人都有一个完整的库，就是我们的本地仓库，在离线情况下，仍然可以进行提交
* 虽然我们平时的工作流程是：每个人的本地仓库提交之后推送到远程仓库，远程仓库看起来好像是一个中心，但其实远程仓库也不过是一个用户而已，实际上你也可以pull张三的库，push给李四等等


### git相关的基本概念

* 工作区（Working Directory/work tree）:就是你的工作目录，就是你在电脑里能看到的目录
* 工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。
* 暂存区（stage）/索引（index）：里面放的是修改过的文件，简而言之就是git add操作之后的文件就会放进来。
* 本地仓库（简称：本地）：输入命令：git commit此次修改的描述，此次改动就放到了本地仓库，每个commit，我们叫他为一个版本。
* 远程仓库（简称：远程）输入命令git push 远程仓库,此次改动就放到了远程仓库(Github等)。


下面就是.git的目录
![](/img/.git.png)

* index就是暂存区，是Git中的重要概念，当对工作区修改、新增文件执行git add命令之后，就会产生一个新纪录存入暂存区中。
* HEAD是一个游标，当我们创建版本库的时候，git会主动为我们创建一个分支master。此时HEAD指向master最近的一个提交，如果你切换到dev分支了，那么HEAD指向的就是dev分支最近的一个提交。
* 针对某些危险操作，Git通过记录HEAD这真的上次所在的位置ORIG_HEAD提供了回退的功能。当你发现某些操作失误了，比如错误的的reset到了一个很早很早的版本，可以使用git reset --hard ORIG_HEAD回退到上一次reset之前。
* hooks：顾名思义，钩子。里面存放了多钟钩子文件，都是.sample后缀，是示例文件，不生效的。去掉.sample后缀才生效，比如pre-commit.sample文件，这个钩子在执行git commit 之前调用，我们在项目中使用的husky+lint-staged就是改了这个钩子文件来实现提交前的校验。
* COMMIT_EDITMSG存放了你最近一个提交的提交信息。
* refs保存了各个本地分支和远程分支以及tags当前所指向的提交，HEAD和refs的关系： HEAD -> refs/heads/branchName -> commit。heads,remote,tags这三个文件夹是一定存在，存放各个分支和tags指向的提交，保存的都是提交的sha1。stash是当你执行了git stash暂存之后生成的一个文件，将当前最前面的stash记录生成一个sha1写进这里面。
* objects 存放了所有的git对象，有兴趣了解更多关于Objects的东西，可以点[这里](https://zhaohuabing.com/post/2019-01-21-git/)

### git配置
在git中，我们使用git config命令来配置git的配置文件，git配置级别主要有以下3类：
+ 仓库级别 local 【优先级最高】
+ 用户级别 global【优先级次之】
+ 系统级别 system【优先级最低】

通常，git仓库级别对应的配置文件是当前仓库下的.git/config

用户信息：

```
git config --global user.name "John Doe"

git config --global user.email johndoe@example.com

```
如果使用了 --global 选项，那么该命令只需要运行一次，因为之后无论你在该系统上做任何事情， Git 都会使用那些信息。 当你想针对特定项目使用不同的用户名称与邮件地址时，可以在那个项目目录下运行没有 --global 选项的命令来配置。

检查配置信息：
如果想要检查你的配置，可以使用 git config --list 命令来列出所有 Git 当时能找到的配置：
```
$ git config --list
user.name=John Doe
user.email=johndoe@example.com
color.status=auto
color.branch=auto
color.interactive=auto
color.diff=auto
...
```

你可以通过输入 git config <key>： 来检查 Git 的某一项配置:

```
git config user.name
John Doe
```

## git常用命令
`git init`, `git clone`, `git add`, `git commit`,`git pull`,`git push`,`git status`

`git merge`, `git rebase`, `git reset`, `git revert`, `git cherry-pick`

### **git init**
生成有个git版本库，在当前目录下生成一个隐藏文件.git

### **git clone**
创建一个本地仓库的克隆版本（克隆某个远程仓库到本地）

```
git clone path/to/reposit
```
从远端服务器上克隆一个版本库

```
git clone username@host:/path/to/repository
```
git主要使用协议、本地协议、http协议、git协议、ssh协议。http协议是最方便的，git协议是最快的。

### **git add**

Add file contents to the index.将指定文件添加到暂存区
```
git add path/to/file
```

批量添加文件

```
// .表示当前目录，这个命令代表将当前目录下所有工作区的改动添加到暂存中
git add .

// -A表示--all添加工作区下所有的改动，不仅仅是当前目录下。
git add -A
```

### **git commit**

将所有暂存区的内容生成一个commit提交记录保存到本地版本库中。每次使用git commit命令我们都会在本地版本库生成一个40位的哈希值，这个哈希值也叫commit-id,commit-id在版本回退的时候是非常有用的，它相当于一个快照，可以在未来的任何时候通过与git reset的组合命令回到这里。

```
// 这种是比较常见的用法，-m参数表示可以直接输入后面的"message",如果不加-m参数，那么是不能直接输入message的，而是会调用一个编辑器一般是vim编辑器让你来输入这个message。
git commit -m "commit message"

// 其他功能如-m参数，加的-a参数可以将所有已跟踪的文件中的执行修改或删除操作的文件都提交到本地仓库，即使他们没有经过git add添加到暂存区。新加的文件（即没有被git系统管理的文件）是不能被提交到本地仓库的。建议一般不要使用-a参数，正常的提交还是使用git add先将要改动的文件添加到暂存区，再用git commit 提交到本地版本库。
git commit -a -m "message"

// 修改最近一次提交的提交信息（也叫追加提交，他可以在不增加一个心得commit-id的情况下将新修改的代码追加到前一次的commit-id中）
git commit --amend

// 查看帮助
git commit --help

```

##### 在git中，任何已提交的东西几乎都是可以恢复的，但是那些未提交丢失之后就无法恢复了。

### ** git log**
在提交了若干更新之后，又或者克隆了某个项目，想回顾下提交历史，可以使用 git log 命令查看。

```
// --pretty选项，可以指定使用完全不同于默认格式的方式展示提交历史。比如用`online`将每个提交放在一行显示。另外还有`short`,`full`,`fuller`可以用
$ git log --pretty=oneline
ca82a6dff817ec66f44342007202690a93763949 changed the version number
085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7 removed unnecessary test code
a11bef06a3f659402fe7563abf99ad00de2209e6 first commit

// 但最有意思的是 format，可以定制要显示的记录格式，这样的输出便于后期编程提取分析，像这样：
$ git log --pretty=format:"%h - %an, %ar : %s"
ca82a6d - Scott Chacon, 11 months ago : changed the version number
085bb3b - Scott Chacon, 11 months ago : removed unnecessary test code
a11bef0 - Scott Chacon, 11 months ago : first commit
```
更多关于`git log`的使用可以看[这里](https://git-scm.com/book/zh/v1/Git-%E5%9F%BA%E7%A1%80-%E6%9F%A5%E7%9C%8B%E6%8F%90%E4%BA%A4%E5%8E%86%E5%8F%B2)

### **git-reflog**
可以用来恢复本地错误操作很重要的一个命令。

git log和git reflog的区别：git log 命令可以显示所有提交过的版本信息，如图所示：
![](/img/log.png)
git reflog 可以查看所有分支的所有操作记录（包括已经被删除的 commit 记录和 reset 的操作。有commit记录，checkout记录）如图：
![](/img/reflog.png)

### **git remote**
远程仓库：托管在因特网或其他网络中的你的项目的版本库。你的项目可以只有一个远程仓库，也可以有多个远程仓库。
```
// 查看远程仓库。选项 -v，会显示需要读写远程仓库使用的 Git 保存的简写与其对应的 URL。 如果你的远程仓库不止一个，该命令会将它们全部列出
git remote -v 

// 添加远程库
git remote add <shortname> <url>

// 删除远程仓库
git remote rm name
```

### ** git pull **
* Fetch from and integrate with another repository or a local branch，从另一个仓库或者本地分支获取并与之集成。 git pull其实是一个简写，是git fetch 和git merge FETCH_HEAD这两个命令的简写。

```
git pull [<options>] [<repository> [<refspec>…​]]

```
`<repository>`是仓库的名字，`<refspec>` 是分支的名字。如果都不写，会有一个默认值。


例子：
```
 A---B---C master on origin
	      /
D---E---F---G master
	      ^
    origin/master in your repository
```
远程的master分支到了c,本地的开发到了G。

```
	        A---B---C origin/master
	       /         \
D---E---F---G---H master
```
git pull之后会生成一个新的H,合并两个分支。

如果发生了冲突，可以使用git reset --merge进行回退。

#### options(选项)
```
// 允许无关的历史，这个选项，更多是在更改远程仓库的时候用到。
–allow-unrelated-histories

// When the merge resolves as a fast-forward, only update the branch pointer, without creating a merge commit. This is the default behavior.
-ff

// Create a merge commit even when the merge resolves as a fast-forward. This is the default behaviour when merging an annotated (and possibly signed) tag that is not ///// stored in its natural place in refs/tags/ hierarchy.
–no-ff

// Refuse to merge and exit with a non-zero status unless the current HEAD is already up to date or the merge can be resolved as a fast-forward.
–ff-only
```
ff选项，这几个选项是说合并时是否开启fast-forward，快速合并。

常用的git pull命令：
```
git pull origin dev:dev
git pull <远程主机名> <远程分支名>:<本地分支名>

// 这条命令是下面这两条命令的简写
git fetch origin
git merge orign/dev

// 本地分支可以省略, pull的默认行为就是merge到你当前所在的分支
git pull origin dev

// 当前dev分支与origin/dev分支建立了追踪关系，所以远程分支名也可以省略
git pull origin

// 当前版本库只存在一个fetch url，远程名也可以省略
git pull

```

git pull --rebase使用git rebase来代替git merge，至于为什么，我们后面会讲到。
```
git pull --rebase origin dev

// 这条命令等同于执行下面两条命令

git fetch origin
git rebase orgin/dev
```

### **git fetch**
```
git fetch <远程主机名> //这个命令将某个远程主机的更新全部取回本地

git fetch <远程主机名> <分支名> //只想取回特定分支的更新
```

取回更新后，会返回一个FETCH_HEAD ，指的是某个branch在服务器上的最新状态，我们可以在本地通过它查看刚取回的更新信息：
```
git log -p FETCH_HEAD
```

#### git pull 和 git fetch的区别

git fetch是将远程主机的最新内容拉到本地，用户在检查了以后决定是否合并到工作本机分支中。

而git pull 则是将远程主机的最新内容拉下来后直接合并，即：git pull = git fetch + git merge，这样可能会产生冲突，需要手动解决。


### **git push**
Updates remote refs using local refs,将本地提交推送到远程仓库

* 完整使用
```
git push [<repository> [<refspec>...]]
```
git push <远程主机名> <本地分支名>  <远程分支名>

* 其他
```
// -u是--set-upstream的简写，表示origin/master分支和local/master分支建立追踪关系，此后使用git push就是采用这种默/// 认方式推送到origin/master分支上
git push -u origin master

// 省略了远程分支名，表示将本地分支推送到与之存在追踪关系的远程分支，通常是一个同名的分支，如果远程没有该分支就会被创/// 建
git push origin master

// 删除origin/dev分支
git push origin :dev

// 这个也是删除origin/dev分支
git push origin -d dev

// 在多人协作开发下，如果其中一个小伙伴删除了远程的某个分支，但是没有主动告诉其他伙伴他删除了分支，所以其他伙伴就可以使用这个命令来手动检查更新远程分支的情况
git remote prune origin

// 这条命令也是检查远程分支的情况
git fetch --prune
```

### **git status**
查看工作区的状态，这个命令可以看作是一种智能提示，因为这个命令不仅会显示working tree当前状态，同时还会提示你下一步的操作。

### **git merge**

* `git merge`是通过合并其他分支的histories来达到当前分支往前走的目的
* `git merge`有`fast-forward`和`no-fast-forward`两种默认方式进行合并，git会根据不同的情况采取不同的默认方式进行合并。

例一：这种情况下master分支相对于dev分支，只是简单的落后于dev分支，在dev分支前进的时间里，master分支没有产生其他的mommit，此时执行`git merge dev`是采用`fast-forward`，直接快进，不会产生一个新的commit节点。
![](/img/merge1.png)

快速合并的结果：
![](/img/merge4.png)

例二：此时在master分支相对于dev分支，不仅仅是简单的落后于dev分支，在dev分支前进的同时master分支也在前进，此时执行git merge dev就会产生一个新的commit节点来记录这一次的merge。
![](/img/merge2.png)

合并后产生一条新的commit
![](/img/merge3.png)

值得注意的是，在fast-forword默认模式下，你可以使用—no-ff(no fast forward)来阻止快速合并，强制生成一条合并commit记录此次合并，但是在no-fast-forward默认模式下，不能强制更改为fast-forward合并。只能使用别的当时去合并，这便是下面要说的，rebase,变基。

### **git rebase**
第二种合并分支的方法是`git rebase`。Rebase实际上就是取出一些列的提交记录，“复制”他们，然后在另外一个地方逐个的放下去。也就是改变当前分支的初始commit，然后在新的base上一个个地运行这个分支上所有的commits。

Rebase的优势就是可以创造更线性的提交历史。如果只允许使用Rebase的话，代码库的提交历史将会变得异常清晰。

![](/img/rebase1.png)

例一： 像上面这种情况：可能因为你的同事在master上改了bug，或者开发了新的功能上传到了master，你开发的功能依赖你同事开发的部分，所以这时候你需要合并代码。如果此时使用merge的话，一定会产生一条merge树，这样非常丑，所以你可以是使用这样的操作：
```
git checkout bugFix
git rebase master

```

![](/img/rebase2.png)



