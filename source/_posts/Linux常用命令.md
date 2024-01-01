---
title: Linux常用命令
top: true
categories: [Linux]
tags: [Linux, 常用命令]
toc: true
---

# 1基础命令:
cd -, cd .. , history, !5 ,  /home/ , /tmp/ , >,&，jobs,nohup 1,2,0 
## 1 cd(change directory)
cd ..返回上一级目录
cd ../..返回上两级目录
cd 进入家目录
cd ~进入家目录(目前用户身份所在的主文件夹)等同于cd
cd ~account 进到account这个用户的主文件夹(/home/account)
cd -返回上次目录
## 2 history
history 显示历史目录

# 2文件和目录操作：
ls,cd,pwd,mkdir,rm,mv,cp
## 1 ls() 最常用ls -al
ls有很多参数，常用的有以下
-a 显示全部文件，包括隐藏文件（开头为.）一起显示
-d 仅列出目录本身，而不是列出目录内的文件数据
-f 直接列出结果，不进行排序（ls默认以文件名排序）
-l 列出长数据串，包含文件的属性与权限
-r 排序结果反向输出
-S 以文件容量大小排序（而不是以文件名排序）
-t 以时间排序，而不是文件名排序
--full-time 以完整时间模式输出，可以呈现文件的修改时间
color=auto 系统自行根据设置判断是否给予颜色
## 2 pwd(print working directory)
显示当前目录
## 3 mkdir(make directory)
创建新目录
mkdir -p 递归创建，自行创建多层目录
mkdir -p test1/test2/test3如果没有test1，那么就会自行创建
## 4 rm和rmdir(remove directory)
删除目录
rmdir -p 连同上层的空目录一起删除（rmdir仅能删除空的目录）
若要删除所有目录下的所有东西用rm命令
rm - r
## 5 复制、移动、删除：cp，rm，mv
### 1 复制cp(copy)
cp 源文件 目标文件
常用的参数有
-a 复制的文件属性完全一样
-i 若目标文件已经存在，会询问是否覆盖
-r 递归持续复制，用于目录的复制行为
### 2 删除rm(remove)
rm [-fir]
-f force的意思，强制删除
-i 询问是否删除
-r 递归删除
### 3 移动或改名mv(move)
mv [-fiu] 源文件 目标文件
-f 强制覆盖，若目标文件已经有，不会询问
-i 若目标文件存在，会询问
-u 若目标文件存在，且源文件比较新，才会更新update

# 3文件内容查阅touch,head,tail,less,more，cat
cat 第一行开始显示文件内容
tac 最好一行开始显示，是cat的倒写
more 一页一页显示文件内容
less和more类似，但它可以往前翻页
head 只看头几行
tail只看后几行
## 1 touch 创建或修改文件
touch 文件
## 2 cat(concatenate)：一口气输出到屏幕
cat -n 最常用 打印出行号，空白行也有行号
cat -b 列出行号，但仅针对非空白行，空白行不标，注意与-n的区别
这个命令单独用比较少，当文件行数超过40，来不及在屏幕看到结果，所以更常用的是more和less
## 3 more一页一页翻
more hg38.tss
```
NR_046018       chr1    9874    13874   0
NR_024540       chr1    27370   31370   1
NR_104148       chr7    64664083        64668083        0
NR_111960       chrX    44871175        44875175        0
NR_028458       chr14   92104621        92108621        1
NR_028459       chr14   92104621        92108621        1
NR_026818       chr1    34081   38081   1
NR_026820       chr1    34081   38081   1
NR_026822       chr1    34081   38081   1
NM_001005484    chr1    67091   71091   0
NR_039983       chr1    138566  142566  1
NR_103753       chr5    102669464       102673464       1
NR_104160       chr10   32345218        32349218        1
NR_026823       chr1    204597  208597  1
NM_001005221    chr1    449678  453678  1
NM_001005224    chr1    449678  453678  1
NM_001005277    chr1    449678  453678  1
NR_028322       chr1    493445  497445  1
NR_028325       chr1    493445  497445  1
NR_125957       chr1    627009  631009  1
NM_001005221-HOMER2     chr1    684654  688654  1
NM_001005224-HOMER2     chr1    684654  688654  1
NM_001005277-HOMER2     chr1    684654  688654  1
NR_028327       chr1    728351  732351  1
NR_033908       chr1    776688  780688  1
NR_103536       chr1    815371  819371  0
NR_047526       chr1    825591  829591  0
NR_047519       chr1    825591  829591  0
NR_047521       chr1    825591  829591  0
--More--(0%)
```
重点在最好一行，光标在这里等待命令,可以执行以下
空格键：向下翻一页
enter：向下滚动一行
/字符串：在显示的内容中，向下查询‘字符串’这个关键字
q：离开more，不再显示该文件内容
## 4 less一页一页翻
less比more更好，可以前后翻，也可以前后查询
空格键：向下翻一页
pagedown：向下翻一页
pageup：向上翻一页
/字符串：向下查询‘字符串’
？字符串：向上查询‘字符串’
q：离开less
各个参数如下
-N显示每行行号
-s显示连续空行为一行
-S行过长将超出部分舍弃
-f强迫打开特殊文件
## 5 head和tail
以行为单位进行数据选取
head [-n number] 文件  默认10行
注意，number可以是负数，比如
head -n -100 文件    
假若文件有141行，那么上面这个就是显示前面41行，后面100行不显示
### 取11到20行呢
先取前20行，再取后10行
head -n 20 文件|tail -n 10,

# 3系统管理： 
df,du,top,free,ps,ipconfig,netstat,ssh,scp,
## 1 df
列出文件系统的整体磁盘使用量
df [-ahikHTm] 目录或文件名，默认以KB列出
-h 较易阅读的GB,MB,KB格式显示
-i 不用硬盘容量，而已inode的数量显示
## 2 du
du [-ahskm]文件名或目录名
-s 列出总量，不列出每个各别的目录占用容量
-h 以人们容易读的容量格式显示(G)
du -sh常用
## top

# 4用户权限：
chown,chgrp,groups,ls
# 5文本操作：
awk,grep,sed,paste,cat,diff,wc,vi
## 5.1 wc
wc命令计算文件的行数，字数和字节数
用法
```
wc [-clw] 文件
```
-c 只显示bytes数
-l 只显示行数
-w 只显示字数
```
$ wc testfile           # testfile文件的统计信息  
3 92 598 testfile       # testfile文件的行数为3、单词数92、字节数598
```
## 5.2 diff
diff命令用了比较两个文件的差异
```
diff me.txt me_copy.txt -y
This is the first post.                                       | This is the first first post.
This is the second line.                                      | This is the second second line.
This is the third line.                                       | This is the third third line..
```
diff的参数很多，具体参考[Linux diff命令](http://www.runoob.com/linux/linux-comm-diff.html),常用-y
结果解释
>"|"表示前后2个文件内容有不同
"<"表示后面文件比前面文件少了1行内容
">"表示后面文件比前面文件多了1行内容
## 5.3 paste命令
paste命令用于合并文件的列，把文件内容列列合并。简单就是列追加，类似R中的`cbind`命令
```
paste [-s] [-d<间隔字符>][文件1][文件2][文件...]
```
```
$ cat p1
1
2
3
```
```
$ cat p2
a
b
c
d
```
```
$ cat p3
I
II
III

```
```
$ paste  p1 p2 p3
1       a       I
2       b       II
3       c       III
        d
```

```
$ paste -d '%^&' p1 p2 p3
1%a^I
2%b^II
3%c^III
%d^
```
看下-s参数
```
$ paste -s p1 p2 p3
1       2       3
a       b       c       d
I       II      III
```
相当于excel里的“转置粘贴”
## 5.4 sort命令
sort是用来对数据进行排序的命令，用法为
```
sort [-bcfMnrtk][源文件][-o 输出文件]
```
注意，sort可以对文本文件以行为单位进行排序。
>sort的参数比较多，但常用的为一下几个
>`-n` `-k` `-t` `-r`

举例如下
### 5.4.1-n的用法
```
$ sort t1

1
13
14
2
4
5
6
```
```
$ sort -n t1

1
2
4
5
6
13
14
```
`-n`的作用很清楚了，可以使得number不是以字符排序，而是以数字排序。
```

$ sort -nr t1
14
13
6
5
4
2
1
```
-r(reverse)反向排序
### 5.4.2-kt的用法
对按字段分隔的数据非常有用
-t指定字段分隔符（sep）
-k指定排序的字段（key）
```
$ cat t2
banana:30:5.5
apple:10:2.5
pear:90:2.3
orange:20:3.4
```
```
$ sort -t ':' -k 2 t2
apple:10:2.5
orange:20:3.4
banana:30:5.5
pear:90:2.3
```
上例中，以'：'进行分隔，按第2个字段进行排序。
