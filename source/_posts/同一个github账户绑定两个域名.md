---
title: 同一个github账户绑定两个域名
top: true
categories: [hexo]
tags: [hexo, github, 个人域名]
toc: true
---

一个github账号可以建立很多仓库，但是只能有一个个人主页仓库，其余都是项目仓库。主页仓库就是`你的名字.github.io这个`，其余的就是https://github.com/hyinli/项目名称这种。
前面已经建立个人主页仓库并绑定域名，现在再利用项目仓库网页并绑定个人域名。具体如下
# 1 先建立个人项目仓库比如名字kk
# 2 建`gh-pages`分支（不能用master）
![建立gh-pages分支](https://upload-images.jianshu.io/upload_images/7976641-3a8c25bdb79f5fce.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
# 3 域名解析
![域名解析](https://upload-images.jianshu.io/upload_images/7976641-fb1673e5a704fce4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
其中，IP这样获得
`ping 你的名字.github.io`
# 4 根目录的source文件夹见CNAME文件
首行写入你要绑定的域名，可以带www
说明：网上建议裸域名。但是是可以带www的，只是项目仓库必须建在`gh-pages`分支，master分支会一直提示错误。
# 5 根目录的'config.yml文件，修改
![config.yml修改](https://upload-images.jianshu.io/upload_images/7976641-1bbe5d185b4a7703.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
# 6 github 项目仓库，setting
填入你的域名，带www
---The end---



