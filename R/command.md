# 常用命令

### 仓库设置

```R
options("repos"=c(CRAN="http://repos.jd.com/CRAN/"))
```

### 安装包

shell环境中用如下命令安装

```shell
Rscript -e "install.packages(c('mclust','dbscan'), repos = 'https://mirrors.tuna.tsinghua.edu.cn/CRAN')"
```

R 环境中使用安装

```R
install.packages(c('mclust','dbscan'), repos = 'https://mirrors.tuna.tsinghua.edu.cn/CRAN')"
```

R安装包的时候最好激活某个conda环境，否则由于gcc的设置问题 可能会找不到某些lib编译文件（如：/opt/conda/bin/../lib/gcc/x86_64-conda_cos6-linux-gnu/7.2.0/../../../../x86_64-conda_cos6-linux-gnu/bin/ld: cannot find -lssl）



Rserve 1.8-6版本的安装，联网环境下

```R
install.packages("Rserve", repos = 'http://www.rforge.net/')
```

### 常用机器学习包安装

Rscript installpackage.R

```R
pkgs = c("nnet","rpart","gbm","kernlab","mboost","randomForest",
    "tree","xgboost","party","lars","boost","e1071","BayesTree","gafit",
    "arules","caret","DWwR","mlr","survival","readr","readxl",
	"haven","xml2","tidyverse","dplyr","tidyr","plyr","reshape2",
	"stringr","stringi","lubridata","hms","formatR","mcmc",
	"data.table","mclust","dbscan","zoo","xts","timeDate",
	"tseries","forecast","quantmod","RQuantLib","portfolio","TTR",
	"sde","YieldCurve","parma","evd","evdbayes","evir","extRemes",
	"ismev","Rwordseq","jiebaR","chinese.misc","tm","animation",
	"ggplot2","GGally","lattice","aplpack","plotly","rwordmap",
	"ggmap","googleVis","ggpubr","showtext","wordcloud2","wordcloud",
	"car","forecast","Imtest","plm","sandwish","urca","snow","Rmpi",
	"Rcpp","OpenCL","devtools","roxygen2","testthat")
for (pkg in pkgs)
if (!require(pkg, character.only = TRUE)){
  install.packages(pkg)
}
```

