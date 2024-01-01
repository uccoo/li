---
title: R检验数据是否符合正态分布
top: true
categories: [R]
tags: [R, 统计]
---
正态分布又叫高斯分布，很多统计学的理论都是假设所用的数据符合正态分布。所以在研究数据时，首先要看数据是否符合正态分布。

首先，R中很多安装包中有自带的数据集，所以在使用某个数据前先看它是在哪个包中。具体可以参考[R各个包里面的数据集列表](https://blog.csdn.net/xmuecor/article/details/37512125).
这次主要用MASS包中的crabs数据
# 1 直方图检验crabs对象是否正态分布
```
library(lattice)
library(MASS)
histogram(crabs$CW)
histogram(~CW|sex, data = crabs,col='lightblue')
```
>解释：
>`|`是统计学符号，表示“基于.....的条件”，这里按螃蟹的性别进行区分处理。

![](https://upload-images.jianshu.io/upload_images/7976641-df90b666660ce070.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


# 2这条 QQ图进行检测
`qqnorm()`可以绘制QQ图。通过绘制的图是否呈现一直线判断是否符合正态分布。另外还有一个`qqline()`函数，在QQ图中绘制一条直线，QQ图中的点越接近这条直线，表示数据越接近正态分布。
```
qqnorm(crabs$CW, main ="QQ for Crabs")
qqline(crabs$CW)
```
![](https://upload-images.jianshu.io/upload_images/7976641-6bd671b7f31bccfb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


# 3 shapiro.test()函数
上述判断方法相对比较主观，`shapiro.test()`相对比较客观。只需将检验的数据当作shapiro。test()的函数即可。
```
nortest1<-shapiro.test(crabs$CW)
nortest1
```
显示为
```
> nortest1

	Shapiro-Wilk normality test

data:  crabs$CW
W = 0.99106, p-value = 0.2542

```
p-value反应服从正态分布的概率，值越小越小的概率符合，通常0.05做标准，大于0.05则表示符合正态分布（此处为0.2542），故符合正态分布

接下来分别检验公螃蟹和母螃蟹是否符合正态分布
```
nortest2 <- with(crabs, tapply(CW, sex,shapiro.test))
nortest2
#结果如下
> nortest2
$F

	Shapiro-Wilk normality test

data:  X[[i]]
W = 0.98823, p-value = 0.5256


$M

	Shapiro-Wilk normality test

data:  X[[i]]
W = 0.98327, p-value = 0.2368

```
可见都符合正态分布。
