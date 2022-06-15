[TOC]

# Git使用

## 443报错

1. 可能DNS污染 修改host   
   - 查询ip地址 https://www.ipaddress.com/
   - 打开host  sudo vim /etc/hosts  
     - 140.82.113.3 github.com 复制到host

- [x] 方法可行

  ## 版本控制

> 版本管理器

- 协同开发
- 追踪和记载一个或者多个文件的历史记录

**集中版本控制**

需要中央服务器，必须联网

1. svn

**分布式版本控制**

​	每个人都拥有全部代码

1. git

## 环境安装

> 启动git

git bash :Unix与linux风格的命令行，使用最多，推荐

git cmd ：windows风格的命令行

git Gui：图形界面的git 

> git配置 

查看配置 git config -l



git config --global --list

![image-20210927152800128](https://tva1.sinaimg.cn/large/008i3skNly1guv7w0sb6uj60o206oab102.jpg)

==设置用户名和邮箱==

Git config --global user.name 'xxx'

git config -- global user.email 33333@email.com



取消config配置

- 取消代理

  - git config --global --unset http.proxy
  - git config --global --unset https.proxy

- ​

  ```
  比如想取消 url.git://.insteadof=https://
  git config --global --unset url.git://.insteadof
  查询 git config --list
  ```

### origin

- git remote -v 查看分支指针名

- fork的话会变成upstream


## GIT 基本理论

> 工作区域

git本地三个工作区域  工作目录（本地文件）、暂存区（add .）、资源库(本地仓库 commit) 、远程目录（push）

> 工作流程

## Git命令

- git pull:  git pull 根据配置的不同，可为git fetch + git merge 或 git fetch + git rebase
- 跳到之前分支  git checkout  <分支>
- git log --oneline       **每个提交在一行内显示**
- git reset --hard <提交的哈希> **重置到相应提交**
- git reflog 查看之前操作记录 找到版本号
- git diff --cached git diff HEAD 比较内容不同
- **git revert**
- 本地和远程仓库不同步问题 先git pull origin master
- git commit --amend -m "更好的提交日志"  编辑上一次提交
  - 在上次提交中附加一些内容，保持提交日志不变git add . && git commit --amend --no-edit

#### push

- 密码无效配置

  [config]()

  ![WeChate17bd659b9bd97485725e2b7b08d1738](/Users/feellife/Library/Containers/com.tencent.xinWeChat/Data/Library/Caches/com.tencent.xinWeChat/2.0b4.0.9/4100b5b68703b908e35e8fd25bba4804/dragImgTmp/WeChate17bd659b9bd97485725e2b7b08d1738.png)



- 常用 **git push origin master**

错误上传

- git push -u origin master -f   强制覆盖已有的分支
- 方法二
  - git pull origin master --allow-unrelated-histories (该选项可以合并两个独立启动仓库的历史)
  - git push -u origin master

### git fetch

- `git fetch`是将远程主机的最新内容拉到本地，用户在检查了以后决定是否合并到工作本机分支中。而`git pull` 则是将远程主机的最新内容拉下来后直接合并
- ​

## GIT文件操作

> 文件的四种状态

- Untracked：未跟踪
- Unmodify： 已经入库，未修改
- Modified ：已修改
- Staged 暂存状态

> 忽略文件

gitignore  空行或者以#开始的行将会被忽略

*.txt #忽略所有 .txt结尾的文件

## GIT分支

git分支中常用指令

```swift
git branch -r //列出所有远程分支
git branch [branch-name] //新建一个分支
git checkout -b [branch]// 切换分支
git merge [branch] 
git branch 查看当前使用分支
```

### 合并分支 merge

把bugFix合并到master  git merge bugFix

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1guw2gs6uszj60ky0kkmxv02.jpg" alt="image-20210928090336042" style="zoom:50%;" />

合并成功之后

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1guw2gg6u91j60kq0js75202.jpg" alt="image-20210928090531646" style="zoom:50%;" />

==step==

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1guw2jjznsuj60q40gmmxx02.jpg" alt="image-20210928090831488" style="zoom:50%;" />

## 常见问题分析

1. push前commit log有问题 添加文件

   - git commit --amend -m "fix"

2. 撤销某个文件的修改 

   <img src="https://tva1.sinaimg.cn/large/008i3skNgy1gvbfs6xf60j60qk03k74p02.jpg" alt="image-20211011161002777" style="zoom:50%;" />

http://inntechhk.asuscomm.com/INNFY/BTA-1413.git
