# git命令使用

[TOC]

***

## 本地版本库

暂存区，临时存放文件，只有一个档，add之后就被覆盖，diff比较当前保存的文件与暂存区区别，暂存区还必须commit提交的版本库，然后版本回退

#### 初始化

+ `git init`

  项目目录下，初始化创建git版本库repository

#### 工作区

+ `git status`

  查看工作区的状态

+ `git help`

#### 撤销修改

+ `git checkout --文件名`

  将工作区的文件回退到缓存区，甚至是删除的文件，但新增文件无法比较，`--`必须，否则切换分支

+ `git checkout`

  显示变化的文件，用符号表示文件操作

  + M 被修改
  + D 被删除

#### 缓存区

- `git add 文件名`或`git add .`

  将文件从工作区提交到暂存区/所有文件

- `git diff 文件名`或`git diff`

  比较工作区的版本和缓存区的差别/所有文件

#### 本地版本库

+ `git commit -m "提交版本的注释信息"`

  将工作区提交到版本库，增加新的一个版本

#### 日志

+ `git log`

  提交的文件的版本，版本回退，log发生变化

#### 版本回退

+ `git reset --hard HEAD^`

  + 从已经commit提交的版本库当中，找最新的版本的上一个，也就是上一个版本，此时的log也发生变化，记录的是对应版本之前的日志信息，此时再修改完毕，提交新的版本，则之间的版本log不显示

  + 已经commit版本1，2，3，当前版本三，回退到版本一，修改提交版本4，则log显示1和4的版本，中间2和3不显示
  + 版本回退，缓存区自动变成版本回退的版本
  + 回退包括删除文件和新增文件

+ `git reset --hard HEAD~2`

  向上回退两个版本

+ `git reflog`

  通过该命令读取log被忽略的版本

+ `git reset --hard 版本号`

***

## 远程同步

+ 上传

  + 服务端新建一个空版本库

  + `git remote add origin https://github.com/callmexiaoyu/-.git`

  + `git push -u origin master`

    第一次推送加 -u 参数，执行命令输入name和password

  + `git push origin master`

    推送版本库

+ 下载

  `git clone https://github.com/callmexiaoyu/本地文件夹`

  仓库网址

+ 查看

  `git remote`

  `git remote -v`

***

## 分支

+ 创建分支

  `git branch 分支名`

+ 切换分支

  `git checkout 分支名`

+ 创建并切换分支

  `git checkout -b 分支名`

+ 检查分支

  `git branch`

+ 分支融合

  `git merge 分支名`

  切换到主分支上将分支融合到主分支上

+ 分支删除

  `git branch -d 分支名`

#### bug分支

文件未add到缓存区，同时也没提交，但功能未解决，不能形成版本提交，且现在需要切换到其他分支，此时切换分支导致缓存区被新的分支覆盖，stash保存在没有commit提交情况下，工作区以及缓存区保存起来，退回到刚拿到版本时候

+ `git stash`隐藏工作区和缓存区

  多次创建，栈结构

+ `git stash list`展示存储的缓存区

+ `git stash apply`

  读取最近刚保存的stash

+ `git stahs drop`

  删除最近刚保存的stash

***

## 多人合作分支管理

+ 提交分支

  `git push origin dev`

+ 克隆

+ `git checkout -b dev origin/dev`

  创建本地分支

+ 提交分支，多人提交相同地方都修改，后来的提交上去报错

  + 下载最新的分支，本地融合提交

    `git pull`

## 规范

add用于修改结束，功能结束commit形成新的一个版本