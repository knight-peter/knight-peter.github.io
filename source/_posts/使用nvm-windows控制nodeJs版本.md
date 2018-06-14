title: 使用nvm-windows控制nodeJs版本
author: 徐子玉
tags:
  - 环境配置
  - nvm
  - node
categories:
  - 前端
date: 2018-06-14 14:04:00
---
> 为了在windows系统中切换不同开发环境的nodeJs版本依赖。决定使用**nvm-windows**来管理nodeJs版本。

### 安装步骤
 1. 下载
首先从[nvm官网](https://github.com/coreybutler/nvm-windows/releases)下载安装包 ，选择setup版本的，解压后里面是一个exe，安装方式就如同一个常见的exe安装包，双击运行即可
 2. 配置nvm的安装位置，任意一个你喜欢的位置都可以（**这个文件夹的名字一定不能含有中文或空格！**）。
 3. 设置node的symlink文件夹位置。（**这个文件夹的名字一定不能含有中文或空格！**）
> 上面两个步骤目录出现空格，在使用`nvm use`的时候会报错`exit status 1`
 
 4. 如果在安装nvm之前，电脑上就已经安装有node的，会看到如下图，询问你是否用nvm管理已经存在的node版本。一定要选`是`，这个弹窗可能会出现好几次，都点`是`。
 5. 安装完成
 <!-- more -->
### 配置步骤
 1. 检查nvm是否安装成功
 使用管理员权限打开一个命令行。输入nvm v，会显示nvm的版本号，有则表示安装成功。
 2. 使用淘宝node镜像，使用淘宝npm镜像
 `nvm node_mirror https://npm.taobao.org/mirrors/node/`
 `nvm npm_mirror https://npm.taobao.org/mirrors/npm/`
 3. 安装指定版本的node： `nvm install 版本号`
 比如安装`8.11.3`：
 `nvm install 8.11.3`
 4. 查看当前电脑上已经安装的全部node版本,正在使用中的版本号前有个**星号**`*`
 `nvm ls`
 5. 切换nodeJs版本
 比如使用`8.11.3`：
 `nvm use 8.11.3`

### 常用命令
- 安装指定版本的node： `nvm install 版本号`
`nvm install 8.9.3`
- 使用指定版本的node： `nvm use 版本号`
`nvm use 8.9.3`
- 使用淘宝node镜像：`nvm node_mirror`
`nvm node_mirror https://npm.taobao.org/mirrors/node/`
- 使用淘宝npm镜像：`nvm npm_mirror`
`nvm npm_mirror https://npm.taobao.org/mirrors/npm/`
- 查看当前电脑上已经安装的全部node版本,正在使用中的版本号前有个**星号**`*`：
`nvm ls`
- 查看可用的（可下载的）全部node版本：
`nvm ls available`