# 徐子玉的博客--用 hexo 做的博客
清除缓存文件 (db.json) 和已生成的静态文件 (public)。
$ hexo clean
文件生成后立即部署网站
$ hexo g -d
启动服务器
$ hexo server

## 本地资料丢失
当重装电脑之后，或者想在其他电脑上修改博客，可以使用下列步骤：
使用git clone git@github.com:strivebo/strivebo.github.io.git拷贝仓库（默认分支为hexo）；
在本地新拷贝的strivebo.github.io文件夹下通过Git bash依次执行下列指令：npm install hexo、npm install、npm install hexo-deployer-git（记得，不需要hexo init这条指令）。