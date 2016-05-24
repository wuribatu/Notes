#Git笔记
##创建版本库
```
$ mkdir learngit
$ cd learngit
$ pwd
/Users/michael/learngit
```
* 初始化一个Git仓库，使用git init命令。

* 添加文件到Git仓库，分两步：

* 第一步，使用命令git add <file>，注意，可反复多次使用，添加多个文件；

* 第二步，使用命令git commit，完成。。

##时光机穿梭

* 要随时掌握工作区的状态，使用git status命令。

* 如果git status告诉你有文件被修改过，用git diff可以查看修改内容。
###版本回退
* HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。

* 穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。

* 要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。

* git diff HEAD -- readme.txt命令可以查看工作区和版本库里面最新版本的区别

* git checkout -- file命令中的--很重要，没有--，就变成了“切换到另一个分支”的命令，我们在后面的分支管理中会再次遇到git checkout命令
###撤销修改
* 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

* 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。

* 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。
###删除文件
* 一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用rm命令删了：

* 命令git rm用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。

* 如果你没有看到.git目录，那是因为这个目录默认是隐藏的，用ls -ah命令就可以看见。
###添加远程库
* 要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；关联后，使用命令git push -u origin master第一次推送master分支的所有内容；此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；
分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！
###从远处库克隆
* 要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。

* Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快。

##分支管理
###创建与合并分支
* Git鼓励大量使用分支：

* 查看分支：git branch

* 创建分支：git branch <name>

* 切换分支：git checkout <name>

* 创建+切换分支：git checkout -b <name>

* 合并某分支到当前分支：git merge <name>

* 删除分支：git branch -d <name>
###解决冲突
* 当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

* 用git log --graph命令可以看到分支合并图。
###分支策略
* 在实际开发中，我们应该按照几个基本原则进行分支管理：

* 首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

* 那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；

* 你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。

* 所以，团队合作的分支看起来就像这样：
![](http://www.liaoxuefeng.com/files/attachments/001384909239390d355eb07d9d64305b6322aaf4edac1e3000/0)
###BUG分支
* 修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

* 当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场。

* 工作区是干净的，刚才的工作现场存到哪去了？用git stash list命令看看：

* 一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；

* 另一种方式是用git stash pop，恢复的同时把stash内容也删了：
###Feature 
* 开发一个新feature，最好新建一个分支；

* 如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。
###多人协作
* 查看远程库信息，使用git remote -v；

* 本地新建的分支如果不推送到远程，对其他人就是不可见的；

* 从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；

* 在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；

* 建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；

* 从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。

##标签管理
###创建标签
* 命令git tag <name>用于新建一个标签，默认为HEAD，也可以指定一个commit id；

* git tag -a <tagname> -m "blablabla..."可以指定标签信息；

* git tag -s <tagname> -m "blablabla..."可以用PGP签名标签；

* 命令git tag可以查看所有标签。
###操作标签
* 命令git push origin <tagname>可以推送一个本地标签；

* 命令git push origin --tags可以推送全部未推送过的本地标签；

* 命令git tag -d <tagname>可以删除一个本地标签；

* 命令git push origin :refs/tags/<tagname>可以删除一个远程标签。

##使用GitHub
* 在GitHub上，可以任意Fork开源仓库；

* 自己拥有Fork后的仓库的读写权限；

* 可以推送pull request给官方仓库来贡献代码。

##自定义Git
* 比如，让Git显示颜色，会让命令输出看起来更醒目：

* git config --global color.ui true
这样，Git会适当地显示不同的颜色，比如git status命令：
###忽略特殊文件
* 忽略某些文件时，需要编写.gitignore；

* .gitignore文件本身要放到版本库里，并且可以对.gitignore做版本管理！
###配置别名
``` 
$ git config --global alias.st status
$ git config --global alias.co checkout
$ git config --global alias.ci commit
$ git config --global alias.br branch
$ git config --global alias.unstage 'reset HEAD'
$ git config --global alias.last 'log -1'
```