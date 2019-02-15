# git 操作

免责声明: 这个文档只是我学习git时写的,里面查阅了很多git资料,很多都不是原创
因此,如果你看完了这篇文档,你就会发现,很多内容都可能会重复出现...
而且重复的内容并不在一起,那是因为,我学到一些,就随机添加一些
比如 `分支操作`,这篇文章就出现了很多次,如果你有兴趣把它们整理到一起,欢迎你这么做
前人栽树,后人乘凉.相信我,这篇文章一定会让你对git了解很多的.

---

### 常用操作

- 先在本地你想要新建项目的文件夹中git init ,让你的文件被git 管理起来
- 然后git status 查看文件状态(红色)
- git add --all 把文件添加进暂存区
- git commit -m ":tada:Initial commit" 提交内容可以添加emotion表情
- 在github上创建自己的仓库
- git remote add origin `你在github上创建的项目仓库地址,就是那串http://XxXX.git` 告诉git推到哪儿
- git push -u origin master   origin就是仓库地址的别名,master 主分支
  + 第一次这样提交,以后再提交直接 git push 就可以了
- git checkout -b develop master  创建develop并切换到该分支
- git checkout master 切换到master分支
- git merge develop 将develop分支合并到当前分支(当前分支为主分支)(合并完只是本地合并,线上并没有
  所以push一下master分支,提交到线上即可)
- git push origin master(提交本地master分支到线上的master分支,完整写法:git push origin master:master)
- git branch -D develop 然后删除本地分支
- git push origin --delete develop 然后删除远程分支
- 再创建新的其他功能分支
  git checkout -b feature-rights 创建feature-rights并切换到该分支
  然后把本地的这个功能分支提交到线上 git push -u origin feature-rights: -u建立追踪关系,下次直接push

---

git commit --amend -m"XXXXX" : 提交本次的修改,最近一次的和本次的一起提交(就是修改上一次的提交)

git commit 如果你没有写 -m "XXX",就会报错 ,这时就进入vim了
先按键盘字母i , 按ESC键, 然后按 shift+: 输入wq 保存并退出 就可以了

---
- git pull origin master 从master分支拉取代码
- git merge --no-ff develop 对develop分支进行合并  --no-ff参数,默认情况下,
  git执行"快进式合并" (fast-farword merge),会直接将Master分支指向Develop分支,
  使用--no-ff参数后，会执行正常合并，在Master分支上生成一个新节点。
  为了保证版本演进的清晰，我们希望采用这种做法。

- git reflog 查看你都做了哪些操作
- git reset --hard 93ffed   93ffed就是你提交时的操作id  这样就能版本回退到你想要的版本

---

> 功能（feature）分支
> 预发布（release）分支
> 修补bug（fixbug）分支
> clear 清除git 屏   cls 清除shell cmd屏

> git log 查看操作日志,按 "q" 退出日志

---

- 查看我们提交的历史记录

git log  或 git log --pretty=oneline

- 版本回退

HEAD 其实是一个指针
把版本回退到前面的版本 当前版本 HEAD 上一个版本HEAD^  往上100个版本 HEAD~100
git reset --hard HEAD^

- 查看自己的每一次命令的记录

如果回退到某一个版本之后又后悔了,那么可以再回到某一次提交,这时可以查看自己的写过的命令
git reflog

- 回到某一次的提交

回到某一次提交就要找到某一次提交的id ,使用fit reflog可以查看自己的命令id
git reset --hard id号

---

## git 常用命令清单

> Workspace：工作区
> Index / Stage：暂存区
> Repository：仓库区（或本地仓库）
> Remote：远程仓库

- 1.新建代码库

```shell
# 在当前目录新建一个Git代码库
$ git init

# 新建一个目录，将其初始化为Git代码库
$ git init [project-name]

# 下载一个项目和它的整个代码历史
$ git clone [url]
```
- 2.配置
- Git的设置文件为`.gitconfig`，它可以在用户主目录下（全局配置），也可以在项目目录下（项目配置）。

```shell
# 显示当前的Git配置
$ git config --list

# 编辑Git配置文件
$ git config -e [--global]

# 设置提交代码时的用户信息
$ git config [--global] user.name "[name]"
$ git config [--global] user.email "[email address]"
```
- 3.增加/删除文件

```shell
# 添加指定文件到暂存区
$ git add [file1] [file2] ...

# 添加指定目录到暂存区，包括子目录
$ git add [dir]

# 添加当前目录的所有文件到暂存区
$ git add .

# 添加每个变化前，都会要求确认
# 对于同一个文件的多处变化，可以实现分次提交
$ git add -p

# 删除工作区文件，并且将这次删除放入暂存区
$ git rm [file1] [file2] ...

# 停止追踪指定文件，但该文件会保留在工作区
$ git rm --cached [file]

# 改名文件，并且将这个改名放入暂存区
$ git mv [file-original] [file-renamed]
```
- 4.代码提交

```shell
# 提交暂存区到仓库区
$ git commit -m [message]

# 提交暂存区的指定文件到仓库区
$ git commit [file1] [file2] ... -m [message]

# 提交工作区自上次commit之后的变化，直接到仓库区
$ git commit -a

# 提交时显示所有diff信息
$ git commit -v

# 使用一次新的commit，替代上一次提交
# 如果代码没有任何新变化，则用来改写上一次commit的提交信息
$ git commit --amend -m [message]

# 重做上一次commit，并包括指定文件的新变化
$ git commit --amend [file1] [file2] ...

```
- 5.分支

```shell
# 列出所有本地分支
$ git branch

# 列出所有远程分支
$ git branch -r

# 列出所有本地分支和远程分支
$ git branch -a

# 新建一个分支，但依然停留在当前分支
$ git branch [branch-name]

# 新建一个分支，并切换到该分支
$ git checkout -b [branch]

# 新建一个分支，指向指定commit
$ git branch [branch] [commit]

# 新建一个分支，与指定的远程分支建立追踪关系
$ git branch --track [branch] [remote-branch]

# 切换到指定分支，并更新工作区
$ git checkout [branch-name]

# 切换到上一个分支
$ git checkout -

# 建立追踪关系，在现有分支与指定的远程分支之间
$ git branch --set-upstream [branch] [remote-branch]

# 合并指定分支到当前分支
$ git merge [branch]

# 选择一个commit，合并进当前分支
$ git cherry-pick [commit]

# 删除分支
$ git branch -d [branch-name]

# 删除远程分支
$ git push origin --delete [branch-name]
# 或者
$ git branch -dr [remote/branch]  # dr就是delete,remote, Remote：远程仓库

```
- 6.标签

```shell
# 列出所有tag
$ git tag

# 新建一个tag在当前commit
$ git tag [tag]

# 新建一个tag在指定commit
$ git tag [tag] [commit]

# 删除本地tag
$ git tag -d [tag]

# 删除远程tag
$ git push origin :refs/tags/[tagName]

# 查看tag信息
$ git show [tag]

# 提交指定tag
$ git push [remote] [tag]

# 提交所有tag
$ git push [remote] --tags

# 新建一个分支，指向某个tag
$ git checkout -b [branch] [tag]
```
- 7.查看信息

```shell
# 显示有变更的文件
$ git status

# 显示当前分支的版本历史
$ git log

# 显示commit历史，以及每次commit发生变更的文件
$ git log --stat

# 搜索提交历史，根据关键词
$ git log -S [keyword]

# 显示某个commit之后的所有变动，每个commit占据一行
$ git log [tag] HEAD --pretty=format:%s

# 显示某个commit之后的所有变动，其"提交说明"必须符合搜索条件
$ git log [tag] HEAD --grep feature

# 显示某个文件的版本历史，包括文件改名
$ git log --follow [file]
$ git whatchanged [file]

# 显示指定文件相关的每一次diff
$ git log -p [file]

# 显示过去5次提交
$ git log -5 --pretty --oneline

# 显示所有提交过的用户，按提交次数排序
$ git shortlog -sn

# 显示指定文件是什么人在什么时间修改过
$ git blame [file]

# 显示暂存区和工作区的差异
$ git diff

# 显示暂存区和上一个commit的差异
$ git diff --cached [file]

# 显示工作区与当前分支最新commit之间的差异
$ git diff HEAD

# 显示两次提交之间的差异
$ git diff [first-branch]...[second-branch]

# 显示今天你写了多少行代码
$ git diff --shortstat "@{0 day ago}"

# 显示某次提交的元数据和内容变化
$ git show [commit]

# 显示某次提交发生变化的文件
$ git show --name-only [commit]

# 显示某次提交时，某个文件的内容
$ git show [commit]:[filename]

# 显示当前分支的最近几次提交
$ git reflog
```
- 8.远程同步

```shell
# 下载远程仓库的所有变动
$ git fetch [remote]

# 显示所有远程仓库
$ git remote -v

# 显示某个远程仓库的信息
$ git remote show [remote]

# 增加一个新的远程仓库，并命名
$ git remote add [shortname] [url]

# 取回远程仓库的变化，并与本地分支合并
$ git pull [remote] [branch]

# 上传本地指定分支到远程仓库
$ git push [remote] [branch]

# 强行推送当前分支到远程仓库，即使有冲突
$ git push [remote] --force

# 推送所有分支到远程仓库
$ git push [remote] --all
```
- 9. 撤销

```shell
# 恢复暂存区的指定文件到工作区
$ git checkout [file]  # git checkout 文件名

# 恢复某个commit的指定文件到暂存区和工作区
$ git checkout [commit] [file]

# 恢复暂存区的所有文件到工作区
$ git checkout .

# 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变
$ git reset [file]

# 重置暂存区与工作区，与上一次commit保持一致
$ git reset --hard

# 重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变
$ git reset [commit]

# 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
$ git reset --hard [commit]

# 重置当前HEAD为指定commit，但保持暂存区和工作区不变
$ git reset --keep [commit]

# 新建一个commit，用来撤销指定commit
# 后者的所有变化都将被前者抵消，并且应用到当前分支
$ git revert [commit]

# 暂时将未提交的变化移除，稍后再移入
$ git stash
$ git stash pop
```
- 10.其他

```shell
# 生成一个可供发布的压缩包
$ git archive
```
---
### 功能分支
功能分支的名字，可以采用feature-*的形式命名。

- 创建一个功能分支：git checkout -b feature-x develop
- 开发完成后，将功能分支合并到develop分支：
  + git checkout develop
  + git merge --no-ff feature-x
- 删除feature分支：git branch -d feature-x

### 预发布分支
- 参考文献[git分支管理-阮一峰](http://www.ruanyifeng.com/blog/2012/07/git.html)

---
## git 使用规范流程

- 第一步: 新建分支 
  首先，每次开发新功能，都应该新建一个单独的分支

```shell
# 获取主干最新代码
$ git checkout master
$ git pull

# 新建一个开发分支myfeature
$ git checkout -b myfeature
```
- 第二步:提交分支commit
  + 分支修改后就可以提交commit了

```shell
$ git add --all
$ git status
$ git commit --verbose

```
  + git add 命令的all参数，表示保存所有变化（包括新建、修改和删除）。从Git 2.0开始，all是 git add 的默认参数，所以也可以用 `git add .` 代替。
  + git status 命令，用来查看发生变动的文件。
  + git commit 命令的verbose参数，会列出 diff 的结果。
- 第三步: 撰写提交信息
  + 提交commit时，必须给出完整扼要的提交信息，下面是一个范本。

```shell
Present-tense summary under 50 characters

* More information about commit (under 72 characters).
* More information about commit (under 72 characters).

http://project.management-system.com/ticket/123
```
  + 第一行是不超过50个字的提要，然后空一行，罗列出改动原因、主要变动、以及需要注意的问题。
  + 最后，提供对应的网址（比如Bug ticket）。
- 第四步: 与主干同步
  + 分支的开发过程中,要经常与主干保持同步
  
```shell
$ git fetch origin
$ git rebase origin/master
```
- 第五步: 合并commit
  + 分支开发完成后，很可能有一堆commit，但是合并到主干的时候，往往希望只有一个（或最多两三个）commit，这样不仅清晰，也容易管理。
  + 将多个commit合并要用到 git rebase 命令: $ git rebase -i origin/master
  + git rebase命令的i参数表示互动（interactive），这时git会打开一个互动界面，进行下一步操作。
  + 具体请参考[阮一峰老师的git使用规范流程](http://www.ruanyifeng.com/blog/2015/08/git-use-process.html)
- 第六步: 推送到远程仓库
  + 合并commit后，就可以推送当前分支到远程仓库了:$ git push --force origin myfeature
  + git push命令要加上force参数，因为rebase以后，分支历史改变了，跟远程分支不一定兼容，有可能要强行推送
- 第七步: 发出Pull Request
  + 提交到远程仓库以后，就可以发出 Pull Request 到master分支，然后请求别人进行代码review，确认可以合并到master。

---
### git 远程操作

git clone
git remote
git fetch
git pull
git push

- 1.git clone 版本库的网址
  + 该命令会在本地主机生成一个目录，与远程主机的版本库同名。
  + 如果要指定不同的目录名，可以将目录名作为git clone命令的第二个参数。
  + $ git clone <版本库的网址> <本地目录名>

```shell
# 比如克隆jquery的版本库
$ git clone https://github.com/jquery/jquery.git

```
  + git clone支持多种协议，除了HTTP(s)以外，还支持SSH、Git、本地文件协议等，下面是一些例子。

```shell
$ git clone http[s]://example.com/path/to/repo.git/
$ git clone ssh://example.com/path/to/repo.git/
$ git clone git://example.com/path/to/repo.git/
$ git clone /opt/git/project.git 
$ git clone file:///opt/git/project.git
$ git clone ftp[s]://example.com/path/to/repo.git/
$ git clone rsync://example.com/path/to/repo.git/
```
  + SSH协议还有另一种写法: `$ git clone [user@]example.com:path/to/repo.git/`
- 2.git remote
  + 为了便于管理，Git要求每个远程主机都必须指定一个主机名。
  + git remote命令就用于管理主机名。
  + 不带选项的时候，git remote命令列出所有远程主机。使用-v选项，可以参看远程主机的网址。

```shell
$ git remote
origin

# 使用 -v
$ git remote -v
origin  git@github.com:jquery/jquery.git (fetch)
origin  git@github.com:jquery/jquery.git (push)

```

  + 上面命令表示，当前只有一台远程主机，叫做origin，以及它的网址。
    克隆版本库的时候，所使用的远程主机自动被Git命名为origin。
    如果想用其他的主机名，需要用git clone命令的-o选项指定。

```shell
# 克隆的时候，指定远程主机叫做jQuery。
$ git clone -o jQuery https://github.com/jquery/jquery.git
$ git remote
jQuery

# git remote show命令加上主机名，可以查看该主机的详细信息。
$ git remote show <主机名>

# git remote add命令用于添加远程主机。
$ git remote add <主机名> <网址>

# git remote rm命令用于删除远程主机。
$ git remote rm <主机名>

#git remote rename命令用于远程主机的改名。
$ git remote rename <原主机名> <新主机名>

```
- 3.git fetch
  + 一旦远程主机的版本库有了更新（Git术语叫做commit），需要将这些更新取回本地，这时就要用到git fetch命令。
  + git fetch命令通常用来查看其他人的进程，因为它取回的代码对你本地的开发代码没有影响。
  + 默认情况下，git fetch取回所有分支（branch）的更新。如果只想取回特定分支的更新，可以指定分支名。
  + 所取回的更新，在本地主机上要用"远程主机名/分支名"的形式读取。
  比如origin主机的master，就要用origin/master读取。 

```shell
# 将某个远程主机的更新，全部取回本地。
$ git fetch <远程主机名>

# 取回origin主机的master分支。
$ git fetch <远程主机名> <分支名>
$ git fetch origin master

# git branch命令的-r选项，可以用来查看远程分支，-a选项查看所有分支。

$ git branch -r
origin/master

$ git branch -a
* master
  remotes/origin/master
```
上面命令表示，本地主机的当前分支是master，远程分支是origin/master。
取回远程主机的更新以后，可以在它的基础上，使用git checkout命令创建一个新的分支。
此外，也可以使用git merge命令或者git rebase命令，在本地分支上合并远程分支。

```shell
# 在origin/master的基础上，创建一个新分支。
$ git checkout -b newBrach origin/master

#在当前分支上，合并origin/master。
$ git merge origin/master
# 或者
$ git rebase origin/master

```
- 4.git pull
  + git pull命令的作用是，取回远程主机某个分支的更新，再与本地的指定分支合并。

```shell

$ git pull <远程主机名> <远程分支名>:<本地分支名>

# 取回origin主机的next分支，与本地的master分支合并
# 如果远程分支是与当前分支合并，则冒号后面的部分可以省略。
$ git pull origin next:master

# 取回origin/next分支，再与当前分支合并。实质上等同于先做git fetch，再做git merge。
$ git pull origin next

$ git fetch origin
$ git merge origin/next

```

在某些场合，Git会自动在本地分支与远程分支之间，建立一种追踪关系（tracking）。
比如，在git clone的时候，所有本地分支默认与远程主机的同名分支，建立追踪关系，也就是说，本地的master分支自动"追踪"origin/master分支。
Git也允许手动建立追踪关系。

```shell
git branch --set-upstream master origin/next
#上面命令指定master分支追踪origin/next分支。

# 如果当前分支与远程分支存在追踪关系，git pull就可以省略远程分支名。
$ git pull origin
# 上面命令表示，本地的当前分支自动与对应的origin主机"追踪分支"（remote-tracking branch）进行合并。
#如果当前分支只有一个追踪分支，连远程主机名都可以省略。

$ git pull
# 上面命令表示，当前分支自动与唯一一个追踪分支进行合并。
#如果合并需要采用rebase模式，可以使用--rebase选项。

$ git pull --rebase <远程主机名> <远程分支名>:<本地分支名>
# 如果远程主机删除了某个分支，默认情况下，git pull 不会在拉取远程分支的时候，删除对应的本地分支。
# 这是为了防止，由于其他人操作了远程主机，导致git pull不知不觉删除了本地分支。
# 但是，你可以改变这个行为，加上参数 -p 就会在本地删除远程已经删除的分支。

$ git pull -p
# 等同于下面的命令
$ git fetch --prune origin 
$ git fetch -p

```
- 5.git push
  + git push命令用于将本地分支的更新，推送到远程主机。它的格式与git pull命令相仿。
  + 分支推送顺序的写法是<来源地>:<目的地>，所以git pull是<远程分支>:<本地分支>，而git push是<本地分支>:<远程分支>。
  + 如果省略远程分支名，则表示将本地分支推送与之存在"追踪关系"的远程分支（通常两者同名），如果该远程分支不存在，则会被新建。
  + 如果省略本地分支名，则表示删除指定的远程分支，因为这等同于推送一个空的本地分支到远程分支。
  + 如果当前分支与远程分支之间存在追踪关系，则本地分支和远程分支都可以省略。
  + 如果当前分支只有一个追踪分支，那么主机名都可以省略。直接 `$ git push`
  + 如果当前分支与多个主机存在追踪关系，则可以使用-u选项指定一个默认主机，这样后面就可以不加任何参数使用git push。
  + 不带任何参数的git push，默认只推送当前分支，这叫做simple方式。
  此外，还有一种matching方式，会推送所有有对应的远程分支的本地分支。
  Git 2.0版本之前，默认采用matching方法，现在改为默认采用simple方式。如果要修改这个设置，可以采用git config命令。


```shell
$ git push <远程主机名> <本地分支名>:<远程分支名>

$ git push origin master
# 上面命令表示，将本地的master分支推送到origin主机的master分支。如果后者不存在，则会被新建。

# 删除origin主机的master分支。
$ git push origin :master
# 等同于
$ git push origin --delete master

# 将当前分支推送到origin主机的对应分支。
$ git push origin

# 将本地的master分支推送到origin主机，同时指定origin为默认主机，后面就可以不加任何参数使用git push了。
$ git push -u origin master

# 不带任何参数的git push，默认只推送当前分支，如果要修改这个设置，可以采用git config命令。
$ git config --global push.default matching
# 或者
$ git config --global push.default simple

#还有一种情况，就是不管是否存在对应的远程分支，将本地的所有分支都推送到远程主机，这时需要使用--all选项。
$ git push --all origin
# 上面命令表示，将所有本地分支都推送到origin主机。

# 如果远程主机的版本比本地版本更新，推送时Git会报错，要求先在本地做git pull合并差异，然后再推送到远程主机。这时，如果你一定要推送，可以使用--force选项。
$ git push --force origin 
# 上面命令使用--force选项，结果导致远程主机上更新的版本被覆盖。除非你很确定要这样做，否则应该尽量避免使用--force选项。

# 最后，git push不会推送标签（tag），除非使用--tags选项。
$ git push origin --tags

```
---
### 工作实战
你在公司中参与公司现有的项目,要先把公司的项目代码拉下来

- 在你想创建项目的文件夹下git bash, 在线上项目仓库复制它的仓库地址
- git clone 复制项目地址, 把项目复制到本地,然后初始化 npm install
- git checkout -b testing6 创建跟项目相同的同名分支testing6分支,并切换到该分支
- git pull origin testing6 从远程testing6分支拉取代码到本地,并与本地分支testing6合并,如果也有冲突就解决冲突,解决冲突后要看一下状态,添加并commit一次
- git fetch  将远程主机的更新，全部取回本地。

- 当你操作完代码,(新增功能或者修改完bug 的时候) 先查看状态,git status ,再添加到本地暂存区,git add .
- 再提交到本地分支git commit -m "填写提交信息"
- 在往线上推之前,要先把线上的代码拉下来合并,避免有人更新了代码,你并不知道,假如你这时候提交会覆盖别人的代码
- 这是要先再pull一下 git pull origin testing6 这时候会自动合并,如果有冲突,会自动解决冲突,自动不了就需要手动解决冲突
- 然后冲突解决完,在往线上推就行了git push origin testing6 

---
### 生成公钥和私钥
- 在git bash 中 输入命令` ssh-keygen -t rsa -C "your_email@example.com" `,然后一直回车到底
- 找到公钥地址  `c盘` --> `用户` --> `你的电脑名(比如我的电脑名为T****na)` --> `.ssh`文件夹 --> `id_rsa.pub`公钥
- 打开`id_rsa.pub`文件,使用记事本打开就行,复制里面的全部内容
- 打开github --> `settings` --> `SSH and GPG keys`
- 把你复制的内容添加到`SSH keys`的大框里面
