---
title: github图表加到网页角落
top: true
categories: [hexo]
tags: [hexo, github]
---
# 1 去[这里](https://github.blog/2008-12-19-github-ribbons/)或[这里](http://tholman.com/github-corners/)找喜欢的样式
找一个合适并喜欢的样式，复制方框的代码
前面的ribbons容易位置错误
# 2 打开主题目录下的_layout.swig文件文件
\themes\next\layout
打开后，在` <div class="headband"></div>`下面（大概23行）

粘贴刚才复制的代码，改成你自己的github名字就可以了。

另外，这里可以延伸。放自己可以链接的外地址，并加上图片，道理一样。