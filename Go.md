
[TOC]

# Go
## mac环境配置

```shell
查询版本
feellife@iMac ~ % go version
go version go1.18.3 darwin/arm64
3. 配置 go的环境变量
feellife@iMac ~ % vim ~/.zshrc
export PATH=$PATH:/usr/local/go/bin
//安装前需要打开代理
feellife@iMac ~ %  go env -w GO111MODULE=on
feellife@iMac ~ % o env -w GOPROXY=https://goproxy.io,direc
vscode shift+command+p 搜索:>Go: Insatall/Update Tools 全选后确定,

```



# Text Style

**Bold**

*Italic*

***Bold+Italic***

<u>Underline</u>

~~Strikthrough~~

>Blockquote - Walter said that he is very poor

1. 1st item
2. 2nd item
3. 3rd item

* 1st item
* 2nd Item
+ 3rd item
+ 4th item

- [x] task list
- [ ] [Check this URL](http://www.hello.com)

---

Display `code` in same line 

```C
print("Hello World")
​````

![Innovation Logo](MD_Demo.assets/innlogo.gif)

Table
| Title(1,1) | Title(1,2) | Title(1,3) |
| :--------- | :--------: | ---------: |
| Cell(2,1)  | Cell(2,2)  |  Cell(2,3) |
| Cell(3,1)  | Cell(3,2)  |  Cell(3,3) |

Title(1,1) | Title(1,2) | Title(1,3)
--------- | :--------: | ---------:
Cell(2,1)  | Cell(2,2)  |  Cell(2,3)
Cell(3,1)  | Cell(3,2)  |  Cell(3,3)


```
# 安装sdk

- go.dev 

- 安装验证 go version

- 设置 GoPath  

  - feellife@apps-iMac ~ % cd ~


  - 打开 sudo nano .bash_profile


  - GOPATH='/Users/feellife/GoPath' 

    export GOPATH

- Go env 验证

### Module包管理

- go mod init 