---
title: github+hexo+个人域名建立个人网站
top: true
categories: [hexo]
tags: [hexo,github,域名,个人网站]
toc: true
---
> 写在前面：
> 之前一直用hugo结合github使用，这次尝试hexo，发现非常喜欢，可塑性很强。
>**主要记录一些坑**
> 这个博文主要是大体记录流程，至于中间参考过的3篇建站文章会列出来。总之根据这篇博文你可以完全搭建属于自己的个性网站。

# 1 github的工作
## 1 注册github，不再描述
## 2 建立repo
记得repo名字要和github id一样。也就是如果你的id是hello,那么要申请一个hello.github.io的repo。

# 2 本地电脑的工作
请参考https://www.jianshu.com/p/189fd945f38f
大体分为以下几步
## 1[下载安装Git](https://gitforwindows.org/)
## 2[下载安装node.js](https://nodejs.org/en/)
## 3安装hexo并进行初始化
选择某个盘，创建hexo文件夹，别的名字也行
```
npm install -g hexo
hexo init
```
## 4修改config.yml文件，关键是theme
稍许，hexo文件夹里会出现很多内容。用notepad++打开`_config.yml`文件，修改
title（博客名字）
subtite(副标题）
author（作者名字）
关键是theme ，系统默认是landscape，也可以[查看更多主题](https://hexo.io/themes/)。个人比较喜欢next主题，另外，
black-blue, BlueLake, pure主题也不错。每个主题都会告诉你下载方式，直接参照进行就行，手动下载或git clone下载。
其余部分可以暂时不动。（现在也可以更改，看2.7.1）下载后的主题会存放在hexo下的themes文件夹里。
> 这个地方是hugo是不一样的处理方式，hugo要吧主题的example文件下的内容进行copy替换。而hexo是同时处理根目录和主题目录。
## 5生成网站并在线查看
现在就可以在本地电脑查看雏形了.
```
hexo g
hexo s
```
若要查看更多hexo命令，请移步[查看更多hexo命令](https://segmentfault.com/a/1190000002632530)
hexo s后，会提示端口，复制就可以本地访问，都是localhost：4000
## 6 写第一篇文章
> 刚才说了hexo是根目录和主题下目录多要修改一些内容。所以，往下会加上根目录（代表\hexo），主题目录代表（\hexo\themes\next）。
 
写文章是在根目录：\hexo\source\_posts。如果你在简书有文章，可以直接复制，可以完美识别。记得如果用toc功能，需要#后又空格，否则不被识别。
写完后，再次hexo s既可以看到更新。
## 7 部署到github
### 1 更改根目录_config.yml，最好需要有以下内容
```
deploy:
  type: git
  repository: https://github.com/你的名字/你的名字.github.io.git
  branch: master 
```
### 2 安装[hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)自动部署发布工具
```
npm install hexo-deployer-git  --save
```
### 3 发布到github
```
hexo clean && hexo g && hexo d
```
如果初次使用需要输入用户名密码之类，牢记，这样你的github repo就会有你刚才push的内容
### 4 在线访问
https://你的名字.github.io/
我的是https://hyinli.github.io/可以查看

# 3主题配置
这部分花费时间比较多，所以单列出来。
现在雏形已经具备，接下来要进行主题配置，添加一些比较有用的插件等。[hexo有很多插件](https://hexo.io/plugins/)
各种各样的配置，一定有你喜欢的。
这部分主要参考这两篇文章,非常详尽，如果还有你想不到的内容，那就单项google搜索。
[Windows下使用hexo搭建博客](https://www.zdaiot.com/%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2/Windows%E4%B8%8B%E4%BD%BF%E7%94%A8hexo%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2/)
[Hexo+NexT搭建博客拥抱舒爽](https://blog.enjoytoshare.club/article/hexo-do-optimization.html)
主要包括有
语言，类别，标签，菜单，头像，统计在线时间，访问量，等。这些不再赘述。
## 1 开启文章目录
请参考[Hexo博客NexT主题开启文章目录和调整样式](https://blog.enjoytoshare.club/article/hexo-do-catalog.html)
nextT本身有目录的，我们需要开启它
### 1 修改custom.styl
`themes/next/source/css/_custom`
notpad++打开，添加以下内容
打开及调整目录字体
```
#文章目录默认展开
//文章目录默认展开
.post-toc .nav .nav-child { display: block; }
#目录字体大小调整
.post-toc ol {  
  font-size : 13px;     
}
```
修改主题配置文件`themes/next/_config.yml`

目录换行
```
toc:
  enable: true  
  wrap: true
```
#文章目录无法锚定的问题
假如出现，文章目录生成，但是就不能链接到正确的地方。如果自己的操作没错误，那就执行
```
hexo clean命令
#然后
hexo d -g
```
# 4 被百度和google搜索引擎能搜索到，参考上述两篇文章
# 5 绑定个人域名
## 1 购买个人域名
我是阿里云买的
## 2 域名解析
设置如下
![域名解析](https://upload-images.jianshu.io/upload_images/7976641-ac066dbbf3c6c003.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
## 3 Hexo设置
`\hexo\source`下创建CNAME文件（没有后缀，且全部大写），notepad++打开，首行写上你购买的域名
我的就是
http://www.ucco.net
网上有人建议必要加www 但是这样设置后 看起来非常不好看，我又加上了
## 4 github设置
再回到github，打开`你的名字.github.io`中的setteings，修正以下内容
`custom domain`中，
![github设置](https://upload-images.jianshu.io/upload_images/7976641-1af6441085da394a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


