# 常见问题

### 安装报undefined symbol: libiconv

```shell
Error: package or namespace load failed for ‘haven’ in dyn.load(file, DLLpath = DLLpath, ...):
 无法载入共享目标对象‘/opt/conda/lib/R/library/haven/libs/haven.so’：:
  /opt/conda/lib/R/library/haven/libs/haven.so: undefined symbol: libiconv
错误: 载入失败
停止执行
ERROR: loading failed
```



  withr::with_makevars(c(PKG_LIBS = "-liconv"), install.packages("haven"), assignment = "+=")

   withr::with_makevars(c(PKG_LIBS = "-liconv"), install.packages("readxl"), assignment = "+=")
  

### icudt55l.zip安装

install.packages("stringi", configure.vars="ICUDT_DIR=/my/directory/for/")