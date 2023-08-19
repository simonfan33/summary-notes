## git命令文档

> 一般来说，日常使用只要记住下图6个命令，就可以了。但是熟练使用，恐怕要记住60～100个命令。

![img](https://www.ruanyifeng.com/blogimg/asset/2015/bg2015120901.png)

### 1.配置相关命令

```shell
# 修改用户名
$ git config --global user.name 指定的用户名
# 修改邮箱
$ git config --global user.email 指定的邮箱号
# 显示当前的Git配置
$ git config --list
# 编辑配置文件
$ git config -e --global

```



### 2.常用命令整合

```shell
# 在当前目录新建一个Git代码库
$ git init
# 创建文件
$ touch 文件名
# 将工作区中所有文件添加到暂存区
$ git add .
# 可以把所有已经在repo的文件的变更标记添加，不会把临时生成的文件也添加进来。比 git add . 好用
git add -u 
# 将文件提交到本地仓库
$ git commit -m "文件的提交信息"
# 显示有变更的文件
$ git status
# 查看本地仓库所有文件
$ git ls-files
# 查看版本详细信息，查看详细的版本号
$ git log
# 查看历史版本以及回滚版本的信息，版本号只查看前7位
$ git reflog
# 查看当前版本
$ git log --oneline
# 将暂存区的文件移除出来
$ git rm --cache 文件名
# 修改文件内容，注意：修改完后status查看状态会报红，需依次提交文件到暂存区、本地仓库
$ vim 文件名
# 修改文件名
$ mv 源文件名/修改后的文件名
# 工作区文件误删后从本地仓库拉取文件到工作区
$ git checkout 分支名称  文件名称
# 彻底删除文件，包括本地仓库，依次执行：
$ git rm 文件名
$ git add 文件名
$ git commit -m "Remove file"
# 如果还需要删除远程仓库中的文件，将以下指令提交上去：
$ git push origin 分支名
# 将工作区的改变的内容进行还原（其他位置无法还原）
$ git restore 文件名
#版本回退：--hard 慎用！
$ git reset --hard 
# 冲突的时候，如果想直接用远程的
git checkout --theirs .
git add .
# 冲突的时候，如果想直接用本地的
git checkout --ours .
git add .
# 或者冲突的时候使用明确的路径
git checkout --theirs path/to/the/conflicted/file
git add path/to/the/conflicted/file
```



### 3.代码提交

```shell
# 设置仓库地址
$ git remote add 仓库地址别名  仓库网址
# 查看仓库地址
$ git remote -v
# 删除本地指定的远程地址，xxx为远程仓库地址在本地的简写。
$ git remote rm xxx
# 拉取码云代码
$ git pull 地址别名  分支名  
# 上传到码云  ！上传文件到码云之前记得先拉取。
$ git push 地址别名  分支名
# 克隆码云仓库文件
$ git clone  码云仓库地址
# 提交时显示所有diff信息
$ git commit -v
# 使用一次新的commit，替代上一次提交
# 如果代码没有任何新变化，则用来改写上一次commit的提交信息
$ git commit --amend -m [message]
# 重做上一次commit，并包括指定文件的新变化
$ git commit --amend [file1] [file2] ...
# 提交工作区自上次commit之后的变化，直接到仓库区
$ git commit -a
```



### 4.分支相关命令

```shell
# 创建分支
$ git branch 分支名
# 用于创建并切换到一个全新的分支，不包含原分支的提交历史，也很重要
$ git checkout --orphan 分支名
# 新建一个分支，并切换到该分支
$ git checkout -b 分支名
# 新建一个分支，但依然停留在当前分支
$ git branch [branch-name]
# 新建一个分支，指向指定commit
$ git branch [branch] [commit]
# 新建一个分支，与指定的远程分支建立追踪关系
$ git branch --track [branch] [remote-branch]
# 查看当前主线所有的分支
$ git branch -v
# 列出所有远程分支
$ git branch -r
# 列出所有本地分支和远程分支
$ git branch -a
# 切换分支
$ git checkout 分支名
# 切换到上一个分支
$ git checkout -
# 切换到指定分支，并更新工作区
$ git checkout [branch-name]
# 合并分支#当前分支中，这里输入哪个分支，这个分支就和当前分支合并
$ git merge 分支名
# 选择一个commit，合并进当前分支
$ git cherry-pick [commit]
# 贮藏某个分支的文件的内容，使其能够不被在切换分支的时候把其内容传递到另一个分支的文件中导致不可挽回的损失，当你从   别的分支回来想继续写当前分支的代码的时候，就需要把当前贮藏的代码给它释放！
$ git stash 
# 释放贮藏的代码
$ git stash pop
# 此命令用于将当前分支的提交更改应用到另一个分支上。具体来说，它会将当前分支中的提交复制一份，并在指定的目标分支上   重新应用这些提交。
# 这样可以使得目标分支上的提交历史变得干净整洁，而不像使用git merge命令那样生成合并提交。需要注意的是，使用git   rebase命令可能会修改提交历史，因此在进行此操作前，请确保你了解其影响并且谨慎使用。
$ git rebase  分支名
# 删除分支
$ git branch -d [branch-name]
# 删除远程分支
$ git push origin --delete [branch-name]
$ git branch -dr [remote/branch]
# 建立追踪关系，在现有分支与指定的远程分支之间
$ git branch --set-upstream [branch] [remote-branch]

```



### 5.标签

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



### 6.文件与版本信息的查看

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
# 显示工作区的差异
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
# 显示整个本地仓库的最近提交，包括所有分支
$ git reflog
```



### 7.远程同步


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



### 8.撤销



```shell
# 恢复暂存区的指定文件到工作区
$ git checkout [file]
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
$ git reset --soft [commit]
# 新建一个commit，用来撤销指定commit
# 后者的所有变化都将被前者抵消，并且应用到当前分支
$ git revert [commit]
# 暂时将未提交的变化移除，稍后再移入
$ git stash
$ git stash pop
```



### 9.其他



```shell
# 生成一个可供发布的压缩包
$ git archive
添加每个变化前，都会要求确认

# 对于同一个文件的多处变化，可以实现分次提交
# 下列字母是命令输入完成后需要你选择的分支命令
y：将更改片段添加到暂存区。
n：不将更改片段添加到暂存区。
q：退出并跳过剩余的更改片段。
a：将当前文件中的所有更改都添加到暂存区。
d：不将当前文件中的所有更改都添加到暂存区。
e：手动编辑更改片段，以自定义添加的内容。
$ git add -p


```



## 未完待续......

