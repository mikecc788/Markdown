[toc]

# sourceTree的使用

## 安装

- https://www.sourcetreeapp.com/

### 简介

•免费Git图形界面工具，可以操作任何Git库

•可以直接添加本地git仓库，也可以选择“New”-“Clone from URL”直接从远程克隆到本地

•可以实时监控自己代码的修改状态，还可以可视化地查看每个代码版本之间的差异

## 服务端新建项目

- 远端新建一个仓库

  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1guyn3x5yehj61ql0u00x302.jpg" alt="image-20210930143107480" style="zoom:50%;" />

## clone远程仓库

- 点击sourcetree新建，选择从URL克隆，填入工程地址

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1guyn02ugbhj60w30u0myw02.jpg" alt="image-20210930142513695" style="zoom:50%;" />



### 上传本地项目到远程服务器

- cd到拉取下来的本地仓库 

  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1guyn907riuj60po03qmxg02.jpg" alt="image-20210930143609769" style="zoom:50%;" />

- 新建一个本地项目在sourcetree目录

  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1guynd187r0j60pg04m0sy02.jpg" alt="image-20210930143956692" style="zoom:50%;" />

  

- 打开sourcetree软件，点击提交

  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1guynfxh35ij61l00ritfr02.jpg" alt="image-20210930144247690" style="zoom:50%;" />



- 提交成功之后，点击推送

  <img src="image-20210930144339391.png" alt="image-20210930144339391" style="zoom:50%;" />



- 项目上传成功

### 关联仓库

- ```xml
  git remote add origin git@github.com:<github username>/<repository name>.git
  ```

- ```crmsh
  git pull origin master --allow-unrelated-histories
  ```







## **本地仓库的创建和提交**

- 桌面新建本地项目,创建file1文件

  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1guypzuwjetj60le03qglv02.jpg" alt="image-20210930161106785" style="zoom:50%;" />

- 打开sourcetree点击新建创建本地仓库

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1guyq40y0ejj60yy0r4gnf02.jpg" alt="image-20210930161504455" style="zoom:50%;" />



- 第一次上传需要验证

  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1gv7vguxdjej61360u041g02.jpg" alt="image-20211008141020299" style="zoom:50%;" />

- 查看标签 git remote -v 

- pull  git pull --rebase tree2 master   

- git push -u tree2 master 

### ssh生成

- 本地生成ssh文件

  - 客户端生成SSH key （公钥和私钥）

    - ```shell
      $ ssh-keygen -t rsa -C "your_email@example.com" -b 2048
      ```

  - 在服务端的配置文件中加入你的公钥  .pub文件

    - 查看公钥 $ cat ~/.ssh/id_rsa.pub

- 配置token

  <img src="https://tva1.sinaimg.cn/large/008i3skNly1gv6pb1vqjaj60vv0u0dj602.jpg" alt="image-20211007135142100" style="zoom:50%;" />

- 关联远程仓库

  - 服务器创建一个新的空仓库，复制仓库地址

  - 点击面板右上角设置

    <img src="https://tva1.sinaimg.cn/large/008i3skNgy1guyqai07qpj61ls0mgaci02.jpg" alt="image-20210930162116409" style="zoom:50%;" />

  - 填写远程仓库名字和地址

    <img src="https://tva1.sinaimg.cn/large/008i3skNgy1guyqe2omkaj61xe0r6jwj02.jpg" alt="image-20210930162446603" style="zoom:50%;" />
  
  - 第一次推送会输入账号密码
  
    <img src="https://tva1.sinaimg.cn/large/008i3skNly1gv6k8zu6w2j61ig0u0act02.jpg" alt="image-20211007105645343" style="zoom:50%;" />

## 重置当前分支到某次提交

- 文件状态选择某次提交

  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1guyrhcwm78j61fy0qs43x02.jpg" alt="image-20210930170227196" style="zoom:50%;" />

- 点击强行合并

  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1guyrj1wt4kj60w40egdhi02.jpg" alt="image-20210930170400521" style="zoom:50%;" />

> 这个功能一般不推荐使用

## 查看历史

- 选择之前的某次提交，点击检出

  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1guyrovaoorj61640jkwhi02.jpg" alt="image-20210930170942307" style="zoom:50%;" />

- 会出现一个HEAD，创建了一个分支

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1guyrq5fzjhj614u0e2gnd02.jpg" alt="image-20210930171052826" style="zoom:50%;" />

## 修改历史并创建分支

- 当有未提交更改时不要创建分支

  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1guyrub07ouj617y0lq78s02.jpg" alt="image-20210930171454200" style="zoom:50%;" />

- 可以点击放弃文件

  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1guys196v4zj61fm0mcdjn02.jpg" alt="image-20210930172134303" style="zoom:50%;" />

### 新建分支命名

- 选中分支，点击分支

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1guysd7yunhj60zk0p4mzr02.jpg" alt="image-20210930173304345" style="zoom:50%;" />



- ==分支合并==

  - 分支1合并到master，先切换到master分支，选中分支1点击合并

    <img src="https://tva1.sinaimg.cn/large/008i3skNgy1guysrxwcr6j61bg0neq6902.jpg" alt="image-20210930174719845" style="zoom:50%;" />

  - 合并成功

    <img src="https://tva1.sinaimg.cn/large/008i3skNgy1guysud31lmj61a20gy41o02.jpg" alt="image-20210930174935054" style="zoom:50%;" />