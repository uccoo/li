---
title: ggstatsplot包的安装和简单使用
top: true
toc: true
categories: [R]
tags: [R, 绘图, SCI绘图]
---
[ggstatsplot]([https://paulvanderlaken.com/2018/08/26/ggstatsplot-creating-graphics-including-statistical-details/](https://paulvanderlaken.com/2018/08/26/ggstatsplot-creating-graphics-including-statistical-details/)
)是很优秀的统计作图包,需要3.5及以上R
安装如下
```
devtools::install_github(
  repo = "IndrajeetPatil/ggstatsplot", # package path on GitHub
  dependencies = TRUE,                 # installs packages which ggstatsplot depends on
  upgrade_dependencies = TRUE          # updates any out of date dependencies
)
```
用某个基因在乳腺癌中突变类型和mRNA表达关系做了一个图，比较漂亮
```
getwd()
a = read.table('plot.txt',header = TRUE, sep = '\t', fill=T)
tail(a)
colnames(a)=c('ID','Alternations','mRNA_expression','Mutations')
dat = a
library(ggstatsplot)
ggbetweenstats(data=dat, x=Alternations, y=mRNA_expression)
```
![](https://upload-images.jianshu.io/upload_images/7976641-f196ac4e89be8ca5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
