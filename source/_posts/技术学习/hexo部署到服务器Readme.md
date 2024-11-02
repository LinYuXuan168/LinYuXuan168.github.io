---
title: hexo部署到服务器Readme
date: 2023-12-01 10:33:02
description: hexo部署
tags:
- hexo
categories:
- 技术学习
music:
 server: netease   # netease, tencent, kugou, xiami, baidu
 type: playlist        # song, playlist, album, search, artist
 id: 8109052701      # song id / playlist id / album id / search keyword
author: 林宇轩
---

## 前言

- 所用的服务器为腾讯云的轻量应用服务器
- 域名也是在腾讯云上购买的
- 我自身的话是现在本地上已经搭建好hexo博客了，并且是可以使用的，一开始是在本地上进行使用的，后续把博客部署到服务器上
- 具体使用的是nginx和git进行搭配部署
## hexo配置
- 在服务器上新建好一个部署目录即可
## 服务器配置
### 配置宝塔界面
拿到服务器后先把宝塔面板配置好，这里可以参照腾讯云文档给出的配置教程进行配置:https://cloud.tencent.com/document/product/1207/54078
### 安装并且配置nginx
nginx ： http和反向代理web服务器
- 这里的安装是直接采用宝塔面板进行安装，方便快捷，然后在面板上进行管理对nginx进行配置
- 如果没有安装宝塔的话nginx的安装会比较复杂，使用命令行进行安装，然后通过nginx.conf进行配置
#### nginx配置
![[Pasted image 20231120141037.png]]
- 修改listen，如果不是80，要改成80的，不然后面会报错，同时端口80的要记得开放
- server_name 改成自己的域名
- root 为部署到服务器上的hexo目录
- 配置完后记得重载(也可以命令行重载 ./nginx -s reload )
### 安装Git服务并且配置
#### 安装Git
```shell
yum install -y git
git --version # 显示版本说明安装成功
```
#### 配置git
- 为服务器添加git用户用于博客自动部署到服务器上面
```shell
useradd git # 添加名为git的用户
passwd git # 修改git用户的密码
```
- 授予git用户sudo权限，在/etc/sudoers 文件下添加：
  git ALL = (ALL) ALL
##### 添加密钥
- 添加ssh密码，避免每一次同步都需要进行验证
```shell
# 本地生成密钥对
ssh-keygen
# 本地的.ssh文件下的id_rsa.pub复制
vim id_rsa.pub
# 在服务器上找到.ssh文件（没有则建立）后新建authorized——keys文件，将密钥复制进去
# 设置权限
chmod  600 ... authorized_keys
chmod  700 ... .ssh
# 将git密钥文件的拥有者改成git用户的
chown -R git:git （.ssh的路径)
```
### 配置git仓库
```shell
# 创建git仓库的位置
mkdir -p /repo 
# 初始化git仓库
git init --bare blog.git
# 把git仓库的拥有者改成git用户
chown -R git:git {仓库路径}
# 配置部署目录 路径 bloh.git -> hooks -> post-receive（找不到则新建一个）在文件输入下列内容
git --work-tree={hexo部署目录} --git-dir= {git仓库目录}
# 增加可执行权限
chown -R git:git {post-receive的路径}
```
## hexo部署配置
### _ config.yml文件配置
- 在hexo的_config.yml文件中，把repo改成git@服务器ip：git仓库目录
```shell
# 部署
hexo clean
hexo generate 
hexo deplot
```
## 添加域名解析
- 在服务器那里添加域名解析即可，记录值为服务器的公网ip
## 可能遇到的问题
### 在git的时候缺乏权限
- 检查部署目录和仓库目录的每一个文件是否都是属于git用户
### 配置完成后使用ip打不开，显示找不到站点
- 检查nginx的配置是否正确，修改后进行重载nginx
