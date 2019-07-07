---
title: git命令速查
date: 2018-03-20 20:59:00
tags: 
  - 工具
  - 命令行
  - git
categories:
 - 工具
 - git
---
### 克隆项目：
git clone https://github.com/shoukailiang/test.git

### 设置
  > 如果你不设置可能还push不上去，第一个要配置的是你个人的用户名称和电子邮件地址。这两条配置很重要，每次 Git 提交时都会引用这两条信息，说明是谁提交了更新，所以会随更新内容一起被永久纳入历史记录：
 - git config --global user.name "shoukailiang"
 - git config --global user.email shoukailiang@qq.com

### git status 查看状态
- [master ≡ +2 ~0 -0!]（红色代表是工作区）：工作区有两个添加的文件，0个修改，0个删除   
- [master ≡ +1 ~0 -0（绿色） | +1 ~0 -0 !（红色）]：绿色表示暂存区   ！表示有冲突

- git add 添加到暂存区
- git commit 提交到版本库，会弹出一个文件填注释

- git add .把修改过得文件全部提交到暂存区（后面是.）
- git commit -m "change demo01":注释写在引号里面，不会弹出记事本

- git commit -a -m "drag.js"：先添加到暂存区后添加到版本库的简写，引号里面写注释

- git log：查看提交的历史，历史多了之后要按回车来显示，退出按q

- git diff :工作区和暂存区的文件的差异的对比
- git diff --cached：暂存区与版本库之间的对比（或者git diff --staged）
- git diff master:工作区与版本库之间的差异 ，master那个是分支的名字

### 撤销
- git reset HEAD drag.js  从暂存区撤销回工作区
- git checkout -- drag.js 从版本库撤销回工作区（会先从暂存区撤销，如果没有才会到版本库中撤销）
- git commit -m "change3 drag.js and demo01.html" --amend  ：比如有两个文件1,2 我把1 add到暂存区，然后commit了全部，
这时候就除了错误，可以使用上述代码：就达到了撤销上一次的commit 然后把文件全部commit ；使用git log 并不会有那次commit的注释信息


### 删除：
- git rm test.txt ：把暂存区中的test.txt删除（要先把工作区中的这个文件删除，否侧会报错）
- git rm -f test.txt 会把暂存区和工作区的test.txt删掉（要先把test.txt  add 到暂存区）
- git rm --cached test.txt 把暂存区的test.txt删掉，不会吧工作区的test.txt删掉


### 恢复：
- 指定文件：比如一不小心在工作区把一个文件给误删了，这时候git log 后找到commit 的id  然后 git checkout e154cb8c45eb drag.js
- git reset --hard e154cb8c45eb   恢复版本，不管有多少文件，可以去过去也可以去未来（未来至回退之后的未来）
- git reset  --hard HEAD^ 回到上一个版本
- git reset  --hard HEAD~2 回退两个版本（跳过两个，回退到第三个）

- git reflog：把历史版本全都打印出来

- git remote 查看远程仓库的名字
- git remote -v 查看名字所对应的远程仓库的地址
- git push origin master  （origin是远程仓库的名字  master是分支名）


### 解决冲突：

- git fetch 拉取远端仓库代码但不进行合并，需要手动合并（在工作区内并不会直接合并还是原来的代码，可以先查看区别后再合并）
	- 查看区别 git diff master origin/master
	- 合并: git merge origin/master,合并会把两个都合并，让我们自己选择（多余的删除）

- git pull  拉取远端仓库代码直接合并


### 开源项目协作（没权限）：  
- fork:相当于一个镜像放在自己的仓库，克隆在本地，修改代码，（也可以在github上直接修改）发布到自己的仓库，
然后pull request  ,写一些留言，对方会看到，对方可以选择 合并


### 分支：
- git branch 查看分支
- git branch new1 创建new1的新分支
- git checkout new1 切换到new1的分支
- git checkout -b new2 创建new2分支并且切换到new2分支上

- （先切换到master分支上）， git merge new1   将new1与master合并
 - git branch --merged ：可以知道当前分支下有被合并的分支 ，比如master分支下合并了new1
 - git branch --no-merged 当前分支下没有被合并的分支
 - git branch -d new1          
    - new1和branch合并后，把new1删除，只能删掉被合并的分支，比如new2没有和master合并就不能删除new2
- git branch -D new2 强制把没有合并的分支删掉
- 有冲突的合并（两个分支不同）
  - 在master上git merge new1  会让你自己选择留下哪个然后 git commit -a -m "....."


- git push origin  new1  把new1分支提交到远端仓库上


- git tag 查看标签
- git tag v1.0   v1.0版 
之后：git push origin v1.0 （这里v1.0不是分支）
- git reset HEAD~  撤销回上一次提交的，和之前在暂存区里提交前一样的状态

### 推荐git 教程
- [廖雪峰git，非常的好](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
- [git小书](http://www.ituring.com.cn/book/1870)
- [官方](https://git-scm.com/book/zh/v2)