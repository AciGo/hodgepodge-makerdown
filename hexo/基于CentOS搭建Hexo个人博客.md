<!-- TOC -->

- [1. 准备环境](#1-准备环境)
    - [安装 Git](#安装-git)
    - [安装 Node.js](#安装-nodejs)
- [2. 安装 Hexo](#2-安装-hexo)
    - [安装 Hexo](#安装-hexo)
        - [用命令创建 hexo 文件夹](#用命令创建-hexo-文件夹)
        - [用命令安装 hexo](#用命令安装-hexo)
        - [初始化 hexo](#初始化-hexo)
        - [测试安装成功](#测试安装成功)
- [3. 将博客部署到 GitHub](#3-将博客部署到-github)
    - [如果没有GitHub账户的先去注册，有 GitHub 账户的直接下一步](#如果没有github账户的先去注册有-github-账户的直接下一步)
    - [设置 user.name 和 user.email](#设置-username-和-useremail)
    - [生成 ssh 密匙](#生成-ssh-密匙)
    - [查看 ssh 密匙](#查看-ssh-密匙)
    - [在 GitHub 账户下添加 SSH key](#在-github-账户下添加-ssh-key)
    - [创建 GitHub 仓库](#创建-github-仓库)
    - [修改 hexo 配置](#修改-hexo-配置)
    - [测试并部署](#测试并部署)
- [4. 绑定域名](#4-绑定域名)
    - [添加解析记录](#添加解析记录)
    - [配置 hexo](#配置-hexo)
- [5. Hexo 博客已搭建完成](#5-hexo-博客已搭建完成)
    - [恭喜](#恭喜)

<!-- /TOC -->

## 1. 准备环境

### 安装 Git
~~~
sudo yum install git-core
~~~

### 安装 Node.js

使用以下命令安装 Node.js
~~~
wget -qO- https://raw.github.com/creationix/nvm/master/install.sh | sh
~~~

更新
~~~
source ~/.bash_profile
~~~

安装 Node.js
~~~
nvm install stable
~~~

## 2. 安装 Hexo

### 安装 Hexo

#### 用命令创建 hexo 文件夹
~~~
mkdir hexo
~~~

#### 用命令安装 hexo
~~~
npm install -g hexo-cli
~~~

#### 初始化 hexo
~~~
cd hexo/
hexo init
~~~

#### 测试安装成功

打开 hexo 服务
~~~
hexo server
~~~

打开浏览器访问 <您的 CVM IP 地址>:4000 即可看到搭建成功的博客页面

## 3. 将博客部署到 GitHub

### 如果没有GitHub账户的先去注册，有 GitHub 账户的直接下一步

去注册 [GitHub](https://github.com/) 账户

### 设置 user.name 和 user.email

把以下命令中的 "Your user.name" 和 "You user.email" 换成自己的
~~~
git config --global user.name "Your user.name"
git config --global user.email "You user.email"
~~~

### 生成 ssh 密匙

user.email 就是自己注册 GitHub 的邮箱
~~~
ssh-keygen -t rsa -C user.email
~~~

下面要输入要保存到的路径
~~~
/root/.ssh/id_rsa
~~~

然后直接回车回车

### 查看 ssh 密匙

打开
- id_rsa.pub

### 在 GitHub 账户下添加 SSH key

[去 GitHub 添加 SSH key](https://github.com/settings/keys)

### 创建 GitHub 仓库

命名格式为"账户的 userName".github.io 例如 ： zhangsan.github.io 去创建 [GitHub 仓库](https://github.com/new)

### 修改 hexo 配置

打开 hexo 配置文件
- _config.yml

修改对应部分
~~~
deploy:
    type: git
    repo: git@github.com:(BoView)/(BoView).github.io.git #括号里面换成自己的用户名和仓库名,去掉括号
    branch: master
~~~
保存一下

### 测试并部署

清空静态页面
~~~
hexo clean
~~~

生成静态页面
~~~
hexo g
~~~

将public文件内容部署到 github 仓库
~~~
hexo d
~~~

如果部署遇到错误的时候，先运行下面这条命令
~~~
npm install hexo-deployer-git --save
~~~

然后重新部署一下
~~~
hexo d
~~~

此时可以打开浏览器访问 userName.github.io（GitHub仓库名）即可以访问到搭建的博客页面

## 4. 绑定域名

### 添加解析记录
- 如果想通过域名访问的就继续，前提是要有自己的域名，要是通过上面的仓库名可以访问就满足的可以跳过这一步
- 去自己的域名下添加解析记录类型为 CNAME 主机记录为 @ 线路选择默认，TTL 选择 600，记录值为 github 的仓库名 userName.github.io

### 配置 hexo

创建 CNAME 配置文件
~~~
touch ~/hexo/source/CNAME
~~~

去 CNAME 文件 下添加刚才解析的域名 例如： zhangsan.com

然后重新部署一下
~~~
hexo g
hexo d
~~~
## 5. Hexo 博客已搭建完成

### 恭喜

此时打开浏览器访问自己的域名即可以访问自己搭建的博客 开启自己的[博客之旅](https://hexo.io/zh-cn/)吧