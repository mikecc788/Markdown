[toc]

# Git使用

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

### origin

- git remote -v 查看分支指针名

- fork的话会变成upstream

  

## GIT 基本理论

> 工作区域

git本地三个工作区域  工作目录（本地文件）、暂存区（add .）、资源库(本地仓库 commit) 、远程目录（push）

> 工作流程



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
```

### 合并分支 merge

把bugFix合并到master  git merge bugFix

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1guw2gs6uszj60ky0kkmxv02.jpg" alt="image-20210928090336042" style="zoom:50%;" />

合并成功之后

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1guw2gg6u91j60kq0js75202.jpg" alt="image-20210928090531646" style="zoom:50%;" />

==step==

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1guw2jjznsuj60q40gmmxx02.jpg" alt="image-20210928090831488" style="zoom:50%;" />



