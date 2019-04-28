# image

### 多hive、hadoop客户端设置

1. hive多客户端

   设置该hive客户端对应的hadoop环境变量

   /usr/lib/hive-kuai/bin/hive-config.sh文件中行首 

   设置export HADOOP_HOME="/usr/lib/hadoop-kuai"， export HIVE_HOME="/usr/lib/hive-kuai"

2. hadoop多客户端

   设置hadoop命令启动时该命令对应的环境变量

   /usr/lib/hadoop-kuai/bin/hadoop 首行添加export HADOOP_HOME="/usr/lib/hadoop-kuai"

   

3. 为多客户端命令设置别名

   hive: /usr/lib/hive-kuai/bin/  加入到path中

   hadoop：设置软链