---
title: How to work with hexo and git pages
date: 2016-10-21 15:20:56
tags:
- Hexo
- GitHub
---

###安装Hexo###
```
cd d:/hexo
npm install hexo-cli -g
hexo init blog
cd blog
npm install
hexo g # 或者hexo generate
hexo s # 或者hexo server，可以在http://localhost:4000/ 查看
```

这里有必要提下Hexo常用的几个命令：
hexo generate (hexo g) 生成静态文件，会在当前目录下生成一个新的叫做public的文件夹
hexo server (hexo s) 启动本地web服务，用于博客的预览
hexo deploy (hexo d) 部署播客到远端（比如github, heroku等平台）
另外还有其他几个常用命令：

```
hexo new "postName" #新建文章
hexo new page "pageName" #新建页面
```

###常用简写###
```
hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy
```

###常用组合###
```
hexo d -g #生成部署
hexo s -g #生成预览
```



###使用git命令行部署###

clone github repo
```
cd d:/hexo/blog
git clone https://github.com/foursquarebehind/foursquarebehind.github.io.git .deploy/foursquarebehind.github.io
```

将我们之前创建的repo克隆到本地，新建一个目录叫做.deploy用于存放克隆的代码。
创建一个deploy脚本文件

```
hexo generate
cp -R public/* .deploy/foursquarebehind.github.io
cd .deploy/foursquarebehind.github.io
git add .
git commit -m “update”
git push origin master
```

简单解释一下，hexo generate生成public文件夹下的新内容，然后将其拷贝至foursquarebehind.github.io的git目录下，然后使用git commit命令提交代码到foursquarebehind.github.io这个repo的master branch上。
需要部署的时候，执行这段脚本就可以了（比如可以将其保存为deploy.sh）。执行过程中可能需要让你输入Github账户的用户名及密码，按照提示操作即可。


to be continued...