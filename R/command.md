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