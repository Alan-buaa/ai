# R

### 支持Rserve

1. 环境中安装install.packages("Rserve", repos = 'http://www.rforge.net') 安装1.8.6版本

2. engine启动脚本中增加 R CMD Rserve --RS-enable-remote 来启动后台服务

   (/mnt/aicephfs/engine/jai-engine/startservice.sh脚本中增加该服务命令)

   注意R CMD Rserve --RS-enable-remote ***--vanilla***  是否保存会话信息参数，否则会报Fatal error: you must specify '--save', '--no-save' or '--vanilla'

3. k8s svc中映射上6311端口



install.packages('rJava') 

 先通过R CMD javareconf在R中注册java环境

install.packages('DBI')

install.packages('RJDBC')

