## Git

分布式版本控制

## 原理

在本地电脑中处理文件，放置本地暂存区，然后推送到版本库

在本地工作目录和暂存区之间有个 add 命令

在暂存区和版本库之间有个 commit 命令

版本库的交流
  * 推送 push
  * 拉取 pull

## 命令

* git add：将本地文件增加到暂存区
* git commit：将暂存区的内容提交到本地仓库（本地分支，默认master）
* git push：将本地仓库（本地分支）的内容推送到远程仓库（远程分支）
* git pull：将远程仓库（远程分支）的内容拉去到本地仓库（本地分支）

## 下载
[Git下载](https://github.com/git-for-windows/git/releases/)

## 安装

勾选Use Git From Git Bash Only

其余默认

## 配置

将bin目录配置到PATH

## 配置Git

* 用户名：git config --global user.name ""
* 邮箱：git config --global user.emall ""

## 配置SSH

ssh-keygen -t rsa -C emall

放入gihub

测试连通性：ssh -T git@github.com

## 使用

* 初始化项目：git init
* 项目关联：git remote add origin ...
* 文件放置暂存区：git add .
* 提交本地仓库：git commit -m "注释内容"
* 推送到远程：
   * 第一次：git push -u origin master
   
## 下载

git clone 地址

## 提交

* git add .
* git commit -m ""
* git push origin master

## 更新到本地

git push

