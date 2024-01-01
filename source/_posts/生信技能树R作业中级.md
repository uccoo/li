---
title: 生信技能树R作业答案-中级
top: true
categories: [R]
tags: [R, 统计,生物信息学]
toc: true
---


#[作业在这里](http://www.bio-info-trainee.com/3750.html)
```
rm(list = ls()) 
options()$repos 
options()$BioC_mirror
options(BioC_mirror="https://mirrors.ustc.edu.cn/bioc/")
options("repos" = c(CRAN="https://mirrors.tuna.tsinghua.edu.cn/CRAN/"))
options()$repos 
options()$BioC_mirror
# https://bioconductor.org/packages/release/bioc/html/GEOquery.html
if (!requireNamespace("BiocManager", quietly = TRUE))
  install.packages("BiocManager")
BiocManager::install("KEGG.db",ask = F,update = F)
BiocManager::install(c("GSEABase","GSVA","clusterProfiler" ),ask = F,update = F)
BiocManager::install(c("GEOquery","limma","impute" ),ask = F,update = F)
BiocManager::install(c("genefu","org.Hs.eg.db","hgu133plus2.db" ),ask = F,update = F)
options()$repos
install.packages('WGCNA')
install.packages(c("FactoMineR", "factoextra"))
install.packages(c("ggplot2", "pheatmap","ggpubr"))
library("FactoMineR")
library("factoextra")
library(GSEABase)
library(GSVA)
library(clusterProfiler)
library(genefu)
library(ggplot2)
library(ggpubr)
library(hgu133plus2.db)
library(limma)
library(org.Hs.eg.db)
library(pheatmap)
```
# Task1: 通过R注释包从ensemble到symbol

> 请根据R包org.Hs.eg.db找到下面ensembl 基因ID 对应的基因名(symbol)
```
# ENSG00000000003.13
# ENSG00000000005.5
# ENSG00000000419.11
# ENSG00000000457.12
# ENSG00000000460.15
# ENSG00000000938.11
```
```
library(org.Hs.eg.db)
ls("package:org.Hs.eg.db")
g2s <- toTable(org.Hs.egSYMBOL)
g2e <- toTable(org.Hs.egENSEMBL)
head(g2s)
head(g2e)
g2se <- merge(g2s, g2e, by='gene_id')
head(g2se)
ensID1 <- c("ENSG00000000003.13","ENSG00000000005.5","ENSG00000000419.11",
           "ENSG00000000457.12","ENSG00000000460.15","ENSG00000000938.11")
ensID <- data.frame(ensID1)
```
## Method1:for循环
```
for (i in 1:nrow(ensID)) {
  ens_i <- strsplit(as.character(ensID[i,1]),'.',fixed = TRUE)[[1]][1]
  ensID[i,2]<-ens_i
}
ensID$ensembl_id  <- NULL
colnames(ensID)[1] <- 'ensembl_id' 
ensID_last <- merge(ensID, g2se, 'ensembl_id')
ensID_last
```
## Method2:apply函数
```
ensID$ensembl_id <- unlist(lapply(ensID1,  function(x) {strsplit(as.character(x),'.',fixed = TRUE)[[1]][1]
  }))
ensID_last2 <- merge(ensID, g2se,"ensembl_id")
```

# Task2:由探针找到对应的基因名
>根据R包hgu133a.db找到下面探针对应的基因名(symbol)
053_at
117_at
121_at
1255_g_at
1316_at
1320_at
1405_i_at
1431_at
1438_at
1487_at
1494_f_at
1598_g_at
160020_at
1729_at
177_at

```
library(hgu133a.db)
ids <- toTable(hgu133aSYMBOL)
head(ids)
myids <- read.table(file = "my_probe_ids.txt")
colnames(myids) <- "probe_id"
myid_sym <- merge(myids, ids, by="probe_id")
head(myid_sym)
```
# Task3 找某个基因在表达矩阵中的值并分组绘图
```
# 找到R包CLL内置的数据集的表达矩阵里面的TP53基因的表达量，并且绘制在 progres.-stable分组的boxplot图
# 提示：
# suppressPackageStartupMessages(library(CLL))
# data(sCLLex)
# sCLLex
# exprSet=exprs(sCLLex)   
# library(hgu95av2.db)
```
以下是代码部分
```
rm(list=ls())
library(CLL)
data(sCLLex)
expr <- exprs(sCLLex)
pdata <- pData(sCLLex)
head(expr)
library(hgu95av2.db)
probe_sym <- toTable(hgu95av2SYMBOL)
tp53_probes <- probe_sym[grep("TP53$", probe_sym$symbol),]
```
```
# probe_id symbol
# 966    1939_at   TP53
# 997  1974_s_at   TP53
# 1420  31618_at   TP53
```
```
boxplot(expr['1939_at',]~pdata$Disease)
```
ggpubr
http://www.sthda.com/english/articles/24-ggpubr-publication-ready-plots/
```
library(ggpubr)
tp53_pub <- cbind(expr['1939_at',], as.data.frame(pdata$Disease))
colnames(tp53_pub) <- c('Expression',"Group")
p <- ggboxplot(tp53_pub, y='Expression', x='Group',fill = 'Group', 
            palette = 'aaas' ,add = 'jitter')
# "npg", "aaas", "lancet", "jco", "ucscgb", "uchicago", "simpsons" "rickandmorty"
p + stat_compare_means()
p + stat_compare_means(method = 't.test')
#the violin plot
my_comparisons <- list(c("progress.", "stable"))
ggviolin(tp53_pub, y='Expression', x='Group',fill = 'Group', 
         palette = 'aaas' ,add = 'boxplot', add.params = list(fill = 'white'))+
         stat_compare_means(comparisons = my_comparisons, method = "t.test")+
         stat_compare_means(label.y = 5)
```
# Task4:对任意肿瘤的任意基因进行分类做图（用cBioPortal）

>cbioportal下载，即利用在线数据（下载），本地进行改观
http://www.cbioportal.org/index.do
Select Studies 选项中输入Breast Invasive Carcinoma PanCancer 搜索并选中目标数据集
Enter Genes 数据框中输入目标基因BRCA1
点击Submit Query提交搜索请求
结果页面中，Plots选项卡选择合适数据进行绘图，或下载数据

```
rm(list = ls())
options(stringsAsFactors = F)
a=read.table('e4-plot.txt',sep = '\t',fill = T,header = T)
View(a)
colnames(a)=c('id','subtype','expression','mut')
dat=a
library(ggstatsplot)
ggbetweenstats(data =dat, x = subtype,  y = expression)
library(ggplot2)
ggsave('plot-again-BRCA1-TCGA-BRCA-cbioportal.png')
```
# Task5:某基因在某肿瘤中的生存曲线作图美化

>找到TP53基因在TCGA数据库的乳腺癌数据集的表达量分组看其是否影响生存
提示使用：http://www.oncolnc.org/
值得商榷的问题，low和high分组问题，若50：50分组，无差异
个人理解，这里不必平分，可以30：30，也就是就挑高和低的30%，这是可以说明问题的

```
rm(list = ls())
options(stringsAsFactors = F)
a=read.table('BRCA_7157_50_50.csv',sep = ',',fill = T,header = T)
#View(a)
dat=a
library(ggplot2)
library(survival)
library(survminer) 
table(dat$Status)
dat$Status=ifelse(dat$Status=='Dead',1,0)
sfit <- survfit(Surv(Days, Status)~Group, data=dat)
sfit
summary(sfit)
ggsurvplot(sfit, conf.int=F, pval=TRUE)
ggsave('survival_TP53_in_BRCA_TCGA.png')
```

# Task6：GEO下载表达数据并提取特定的基因做热图（要去重复）

>下载数据集GSE17215的表达矩阵并且提取下面的基因画热图
ACTR3B ANLN BAG1 BCL2 BIRC5 BLVRA CCNB1 CCNE1 CDC20 CDC6 CDCA1 CDH3
CENPF CEP55 CXXC5 EGFR ERBB2 ESR1 EXO1 FGFR4 FOXA1 FOXC1 GPR160 GRB7
KIF2C KNTC2 KRT14 KRT17 KRT5 MAPT MDM2 MELK MIA MKI67 MLPH MMP11 MYBL2
MYC NAT1 ORC6L PGR PHGDH PTTG1 RRM2 SFRP1 SLC39A6 TMEM45B TYMS UBE2C UBE2T
GSE17215数据在https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE17215
可知，是U133A的芯片数据,共6个样本
title为Expression data from paclitaxel and salinomycin-treated HMLER breast cancer cells

```
library(hgu133a.db)
```

## 有三种方法得到表达数据
### 1.下载.CEL格式文件，自己进行处理
### 2.手动下载series Matrix Files，然后读入
### 3.利用R下载表达数据
```
library(GEOquery)
library(GEOquery)
#GEO使用https://www.bioconductor.org/packages/release/bioc/vignettes/GEOquery/inst/doc/GEOquery.html
gse17215 <- getGEO('GSE17215',
                   AnnotGPL = F,     ## 注释文件
                   getGPL = F)       ## 平台文件
show(gse17215)
dat <- exprs(gse17215[[1]])
pdata_17215 <- pData(gse17215[[1]])
pdata_17215$`agent:ch1`

#提取hgu133a包中的probe_id和symbol
ids <- toTable(hgu133aSYMBOL)
dat <- dat[ids$probe_id,]#合并，也可以用merge
#这里有个问题，一共50个基因名，但是得到的是87个探针，也就是有一个gene对应几个探针的情况
#怎么处理？ 取median或者去p最小的,也有取均值的,
#先把median加到ids最后一列
ids$median <- apply(dat, 1, median)
head(ids)
ids <- ids[order(ids$symbol, ids$median, decreasing = T),]
head(ids)
ids_dup <- ids[!duplicated(ids$symbol),]
head(ids_dup)
dat <- dat[ids_dup$probe_id,]
dim(dat)
#这样就得到了去重重复的探针在6个样本中的表达信息，但没有基因名
dat <- as.data.frame(dat)
dat$probe_id <- rownames(dat)
dat_symbol <- merge(dat, ids, by ="probe_id")
#载入50个基因名
genes <- 'ACTR3B ANLN BAG1 BCL2 BIRC5 BLVRA CCNB1 CCNE1 CDC20 CDC6 CDCA1 CDH3 CENPF CEP55 CXXC5 EGFR ERBB2 ESR1 EXO1 FGFR4 FOXA1 FOXC1 GPR160 GRB7 KIF2C KNTC2 KRT14 KRT17 KRT5 MAPT MDM2 MELK MIA MKI67 MLPH MMP11 MYBL2 MYC NAT1 ORC6L PGR PHGDH PTTG1 RRM2 SFRP1 SLC39A6 TMEM45B TYMS UBE2C UBE2T'
genes <- strsplit(genes, ' ')[[1]]
genes <- data.frame(genes)
colnames(genes) <- 'symbol'
genes_expr <- merge(genes, dat_symbol, by ='symbol')
#至此完成了探针过滤，加基因名等操作
#取对数
genes_expr <- log2(dat+1)
heat_expr <- genes_expr[,-c(2,9)]
rownames(heat_expr) <- heat_expr$symbol 
heat_expr <- heat_expr[,-1]
heat_expr <- log2(heat_expr+1)
pheatmap::pheatmap(heat_expr, scale = 'row')
ggcorrplot::ggcorrplot(cor(heat_expr))
```


# Task7:GEO下载表达矩阵做样本的相关性热图，需要标记样本分组信息

>作业7
下载数据集GSE24673的表达矩阵计算样本的相关性并且绘制热图，需要标记上样本分组信息
数据地址https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE24673

```
rm(list=ls())
options(stringsAsFactors = F)   
gse24673 <- getGEO('GSE24673',
                   AnnotGPL = F,     ## 注释文件
                   getGPL = F)       ## 平台文件
show(gse24673)
dat <- exprs(gse24673[[1]])
pdata_24673 <- pData(gse24673[[1]]) 
pdata_24673 <- pdata_24673$source_name_ch1
pdata_24673 <- data.frame(Group = pdata_24673)
#添加分组信息
rownames(pdata_24673) <- colnames(cor_dat)
pheatmap::pheatmap(cor_dat, annotation_col=pdata_24673)
```
# Task8： 找到芯片对应平台的注释包并进行安装

>找到 GPL6244 platform of Affymetrix Human Gene 1.0 ST Array 对应的R的bioconductor注释包，并且安装它！
>参考http://www.bio-info-trainee.com/1399.html
```
options()$repos
options()$BioC_mirror 
options(BioC_mirror="https://mirrors.ustc.edu.cn/bioc/")
options("repos" = c(CRAN="https://mirrors.tuna.tsinghua.edu.cn/CRAN/"))
BiocManager::install("hugene10sttranscriptcluster.db",ask = F,update = F)
options()$repos
options()$BioC_mirror
```
# Task9：GEO下载表达矩阵，所有样本中挑选所需要的探针

>下载数据集GSE42872的表达矩阵，并且分别挑选出所有样本的(平均表达量/sd/mad/)最大的探针，
>并且找到它们对应的基因。
```
rm(list=ls())
options(stringsAsFactors = F)   
gse42872 <- getGEO('GSE42872',
                   AnnotGPL = F,     ## 注释文件
                   getGPL = F)       ## 平台文件
show(gse42872)
dat <- as.data.frame(exprs(gse42872[[1]]))
pdata_42872 <- pData(gse42872[[1]]) 
pdata_42872 <- pdata_42872$source_name_ch1
pdata_42872 <- data.frame(Group = pdata_42872)

max_mean <- names(sort(apply(dat,1, mean), decreasing = T))[1]
max_sd <- names(sort(apply(dat,1, sd), decreasing = T))[1]
max_mad <- names(sort(apply(dat,1, mad), decreasing = T))[1]
str(max_max)
library(hugene10sttranscriptcluster.db)
list(hugene10sttranscriptcluster.db)
###find the corresponding genes for the probes
ids <- toTable(hugene10sttranscriptclusterSYMBOL)
ids[match(max_mad, ids$probe_id),]
#或者
select(hugene10sttranscriptcluster.db,
       keys = c(max_sd, max_mad, max_mean),
       columns = c("SYMBOL"),
       keytype = "PROBEID")
##注意，max_mad探针对应着很多基因名
```

# 10 Task10

> 这部分内容参考http://www.bio-info-trainee.com/bioconductor_China/software/limma.html
> 下载数据集GSE42872的表达矩阵，并且根据分组使用limma做差异分析，
> 得到差异结果矩阵

```
library(GEOquery)
rm(list=ls())
options(stringsAsFactors = F)   
gse42872 <- getGEO('GSE42872', GSEMatrix = TRUE,
                   AnnotGPL = F,     ## 注释文件
                   getGPL = F)       ## 平台文件
show(gse42872)
dat <- as.data.frame(exprs(gse42872[[1]]))
pdata_42872 <- pData(gse42872[[1]]) 
pdata_42872 <- pdata_42872$title


group <- unlist(lapply(pdata_42872, function(x)
{
  strsplit(x,' ')[[1]][4]
}
  ))
#pdata_42872 <- data.frame(Group = pdata_42872)
library(hugene10sttranscriptcluster.db)
ids <- toTable(hugene10sttranscriptclusterSYMBOL)
dim(ids)
dim(dat)
# > dim(ids)
# [1] 19827     2
# > dim(dat)
# [1] 33297     6
#很多探针没有注释信息，进行过滤,/可以用merge但是需要加列
dat_filter <- dat[match(ids$probe_id,rownames(dat)),]
dim(dat_filter)
#用%in%进行筛选
dat_filter2 <- dat[rownames(dat) %in% ids$probe_id,]
# > dim(dat_filter)
# [1] 19827     6
#dat_filter is the expression matrix
```

## 差异表达分析
需要分组矩阵（design），比较矩阵（contrast）
```
library(limma)
##########分组矩阵design，group中的说明。1代表是。用于每张芯片的RNA样本分类
design <- model.matrix(~0+factor(group))
colnames(design) <- levels(factor(group))
rownames(design) <- colnames(dat_filter)
design
# Control Vemurafenib
# GSM1052615       1           0
# GSM1052616       1           0
# GSM1052617       1           0
# GSM1052618       0           1
# GSM1052619       0           1
# GSM1052620       0           1
###############比较矩阵
colnames(design) <- levels(factor(group))
rownames(design) <- colnames(dat_filter)
design
#再做一个比较矩阵【一般是case比control】，规定实验组和对照组并定义谁比谁
contrast.matrix<-makeContrasts(paste0(unique(group),collapse = "-"),levels = design)
#或者下面。注意是Vemurafenib/Control
contrast.matrix_2 <- makeContrasts("Vemurafenib-Control" ,levels = design)
```
## 筛选差异基因DEGs

###分步进行1，这步用到实验设计矩阵
```
fit <- lmFit(dat_filer, design)
```
###分步进行2，用到比较矩阵
```
fit2 <- contrasts.fit(fit, contrast.matrix)
fit2 <- eBayes(fit2)
```
###分步进行3 
```
tem_output <- topTable(fit2, coef = 1, n= Inf)
mydegs <- na.omit(tem_output)
```
###写成函数
```
deg<- function(dat_filter,design,contrast){
  ##step1
  fit <- lmFit(dat_filter,design)#用到实验设计矩阵
  ##step2
  fit2 <- contrasts.fit(fit, contrast) #用到比较矩阵
  fit2 <- eBayes(fit2)  
  ##step3
  mtx = topTable(fit2, coef=1, n=Inf)
  deg_mtx = na.omit(mtx) 
  return(deg_mtx)
}
deg_mtx <- DEG(dat_filter,design,contrast) #得到全部的差异基因矩阵
head(DEG_mtx)
```
## 下面和symbole对应
```
library(hugene10sttranscriptcluster.db)
ids <- toTable(hugene10sttranscriptclusterSYMBOL)
deg_mtx$probe_id <- rownames(deg_mtx)
deg_mtx <- merge(ids,deg_mtx, by = 'probe_id')
write.csv(deg_mtx, 'GSE42872_DEGs_limma.csv', quote = F)
```
