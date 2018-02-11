title: 前端开发环境搭建
tags:
  - 前端
  - 环境配置
categories:
  - 前端
author: 徐子玉
date: 2018-02-03 14:45:00
---
## 代码运行环境安装  
* ### NodeJs  
> [利用nvm安装和在多个Node.js版本之间切换](http://www.jianshu.com/p/07c3456e875a)  
npm更换源与yarn类似  
`$ npm install -g cnpm --registry=https://registry.npm.taobao.org //下载cnpm`  
`$ npm config set registry https://registry.npm.taobao.org`
* ### Ruby
> 1. 下载Ruby安装包，[官网地址](https://rubyinstaller.org/downloads/),在安装的时候，请勾选Add Ruby executables to your PATH这个选项，添加环境变量，不然以后使用编译软件的时候会提示找不到ruby  
> 2. [RubyGems 镜像- Ruby China](https://gems.ruby-china.org/)更换Ruby下载源  
`$ gem sources --add https://gems.ruby-china.org/ --remove https://rubygems.org/ //删除默认源，添加镜像源`  
`$ gem sources -l  //显示更换后的下载源`  
`https://gems.ruby-china.org`  
`# 确保只有 gems.ruby-china.org`
> 3. 安装**Sass**的gem，`gem install sass`
## 开发软件下载安装
* ### WebStorm
> 前端主要开发IDE，功能齐全，但是需要验证码。  
> [注册码获取网址1](http://idea.lanyus.com/)  
> [注册码获取网址2](https://www.iteblog.com/idea/)  
* ### Visual Studio Code  
> 前端次要开发编辑器，拥有很多好用插件，可以当一个很不错的编辑器使用  
* ### yarn
> npm的替代品，操作的指令更加简单，可以使用安装包进行安装，不过安装包经常下载失败。  
> 1. 安装包下载：[链接地址](http://pan.baidu.com/s/1pLRuEnx) 密码：drtf （失败则使用步骤2） 
> 2. npm下载cnpm，然后用cnpm下载yarn。  `cnpm install yarn -g//安装yarn`
> 3. yarn更换下载源  
>> `yarn config get registry // 查看下载源` 
`yarn config set registry https://registry.npm.taobao.org //更换为淘宝源`

* ### Git
> 代码版本控制软件，可使用命令符进行控制。也可以加装[**SourceTree**](https://www.sourcetreeapp.com/)（git的图形界面版）  
**SourceTree** 安装之后需要使用账号登陆以授权，因为墙的原因，无法登陆。这里记录一下跳过这个初始化的步骤。  
> 1. 安装之后，转到用户本地文件夹下的 SourceTree 目录，没有则新建  
`%LocalAppData%\Atlassian\SourceTree\`
> 2. 新建 accounts.json 文件  
`%LocalAppData%\Atlassian\SourceTree\accounts.json`  
> 3. 输入以下内容保存即可  
> `[`  
  `{`  
    `"$id": "1",`  
    `"$type": "SourceTree.Api.Host.Identity.Model.IdentityAccount,`   `SourceTree.Api.Host.Identity",`  
    `"Authenticate": true,`  
    `"HostInstance": {`  
      `"$id": "2",`  
      `"$type": "SourceTree.Host.Atlassianaccount.AtlassianAccountInstance, SourceTree.Host.AtlassianAccount",`  
      `"Host": {`  
        `"$id": "3",`  
        `"$type": "SourceTree.Host.Atlassianaccount.AtlassianAccountHost, SourceTree.Host.AtlassianAccount",`  
        `"Id": "atlassian account"`  
      `},`  
      `"BaseUrl": "https://id.atlassian.com/"`  
    `},`  
    `"Credentials": {`  
      `"$id": "4",`  
      `"$type": "SourceTree.Model.BasicAuthCredentials,`   `SourceTree.Api.Account",`  
      `"Username": "",`  
      `"Email": null`  
    `},`  
    `"IsDefault": false`  
  `}`  
`]`  
> 4. 现在再打开 SourceTree，直接显示主窗口了 
> 5. git中配置autocrlf来正确处理crlf，防止发生报错。  
`$ git config --global core.autocrlf true`  
Git可以在你提交时自动地把行结束符CRLF转换成LF，而在签出代码时把LF转换成CRLF。用core.autocrlf来打开此项功能，如果是在Windows系统上，把它设置成true，这样当签出代码时，LF会被转换成CRLF
* ### 微信开发者工具
> 适用于微信公众号网页开发，微信小程序开发
## 搭建项目
> [项目配置文件](http://pan.baidu.com/s/1i4Ts7mH) 密码：*2dxp*
* ### 项目结构
> project/  
    ├── dist/（生产环境代码）  
    ├── docs/（开发文档）  
    ├── src/（开发环境代码）   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├── asset/（开发环境代码）   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├── sass/（scss文件，需要用sass转换）  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├── js/（js文件，可能是es6格式，需要babel转换）  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├── images/（图片，需要进行压缩）  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├── plugins/（第三方插件）  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├── .html（html页面，需压缩）  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├── .gitignore（git忽略文件配置）  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├── gulpfile.js（gulp任务文件）  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├── package.json（node依赖包配置文件）  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├── yarn.lock（node依赖包关系文件）
* ### 建立版本控制
> 1. 创建SSH key  
在户主目录下（*C:/users/电脑名/.ssh/id_rsa*），里面有id_rsa（私钥，不可泄露），id_rsa.pub（公钥，可以告诉任何人）  
> 2. 如没有，则用Git Bash创建  
`$ ssh-keygen -t rsa -C "xuqipeter@qq.com"`  
然后一路回车，使用默认值即可  
> 3. 登陆GtiHub=>Accout Settings=>SSH Keys页面=>点Add SSH Key=>填上任意Title，在key文本框里粘贴id-rsa.pub的内容
> 4. 在码云或者GitHub创建仓库（公司可以用码云，私人可以用GitHub）
> 5. 设置本地仓库，并创建版本。  
>`$ git config --global user.name "xuqipeter" //全局设置用户名字`  
`$ git config user.email "xuqipeter@qq.com" //全局设置用户邮箱`  
`$ git init //建立本地仓库`  
`$ git add -A //添加全部文件`  
`$ git commit -m "提交的相关说明" //创建本地仓库版本`  
> 6. 添加远程仓库&nbsp;&nbsp;[**教程点击这里**](http://www.yiibai.com/git)  
`$ git remote add origin git@github.com:/github用户名/learngit.git //远程仓库名字默认为origin`  
`$ git fetch origin master:tmp //拉取远程master分支,命名为tmp分支`  
`$ git diff tmp //比较本地的master分支和远程的origin/master分支的差别`  
`$ git branch -a  //查看所有分支`  
`$ git merge tmp ($ git merge origin/master) //合并远程分支,pull=fetch+merge`  

> 7. git 在pull或者合并分支的时候有时会遇到这个界面。可以不管(直接下面3,4步)，如果要输入解释的话就需要:  
`1.按键盘字母 i 进入insert模式`  
`2.修改最上面那行黄色合并信息,可以不修改`  
`3.按键盘左上角"Esc"`  
`4.输入":wq",注意是冒号+wq,按回车键即可`  
`$ git push -u origin master //把本地内容推送到远程库上`  
`$ git branch -d tmp //删除无用分支`  
pull：本地<=远程  
push：本地=>远程  
> 8. 从远程库克隆  
`git clone git@github.com:/github用户名/learngit.git`
* ### 安装依赖包
> 1. 使用yarn安装依赖包（*node_modules*文件夹）  
`$ yarn init`  
`$ yarn install`  
> 2. 可能遇到的问题，请百度解决。
* ### 创建生产环境代码
> 1. 使用gulp（在webstorm中设置node环境为6.x.x版本）  
因为gulp已经开始过时，所以不再继续更新gulpfile.js，以后会替换为webpack  
> 2. 在html中的css和js链接末尾添加`?rev=@@hash`，自动生成版本号
> 3. css和js文件合并（*build:js asset/js/p-cruise-line.js*是合并后的文件路径）  
>`<!-- build:js asset/js/p-cruise-line.js?rev=@@hash -->`  
    `这里放要合并的文件`  
`<!-- endbuild -->`

