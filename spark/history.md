# spark history server

1. 创建hdfs配置

   ```shell
   kubectl -n spark create configmap hdfs-site --from-file=hdfs-site.xml
   kubectl -n spark create configmap core-site --from-file=core-site.xml
   ```

2. 修改chart value

3. helm打包

   ```shell
   helm package spark-history-server/   
   ```

   

4. 安装

   ```shell
   helm install local/spark-history-server --namespace=spark --name=spark-history
   ```

   