# pyspark

1. jupyterlab环境里使用pip安装pyspark

2. 编写jupyterlab的pyspark kernel

   * 使用jupyter kernelspec list查看已有的kernel

   ```shell
   [root@jupyter conda]# jupyter kernelspec list
   Available kernels:
     ir         /root/.local/share/jupyter/kernels/ir
     pyspark    /opt/conda/share/jupyter/kernels/pyspark
     python3    /opt/conda/share/jupyter/kernels/python3
     python2    /usr/local/share/jupyter/kernels/python2
   [root@jupyter conda]# 
   ```

   * 在/opt/conda/share/jupyter/kernels目录下新建pyspark目录

   * 在该目录下创建kernel.json

     ```json
     {
      "display_name": "PySpark",
      "language": "python",
      "argv": [
       "/opt/conda/bin/python",
       "-m",
       "ipykernel",
       "-f",
       "{connection_file}"
      ],
      "env": {
       "SPARK_HOME": "/opt/conda/lib/python3.7/site-packages/pyspark",
       "HADOOP_CONF_DIR":"/usr/lib/hadoop/etc/hadoop",
       "PYSPARK_SUBMIT_ARGS": "--master k8s://https://SPARKMASTER --conf spark.kubernetes.container.image=IMAGEREPO/SPARKIMAGE  --conf spark.kubernetes.namespace=SAPRKNS --conf spark.driver.host=DRIVERHOST.kubeflow  pyspark-shell",
       "PYSPARK_PYTHON":"/usr/bin/python3"
      }
     }
     ```

     其中SPARK_HOME为pyspark的安装目录，

     HADOOP_CONF_DIR为hadoop的配置文件路径 用于支持hive查询（需要将hive-site.xml copy到此目录下），

     PYSPARK_PYTHON为executor环境里的python路径，

     PYSPARK_SUBMIT_ARGS为任务提交的参数，各参数释义如下：

     * master为kubernetes api server地址。可以用kubectl cluster-info查看url地址

       ```shell
       $ kubectl cluster-info
       Kubernetes master is running at http://127.0.0.1:6443
       ```

       

     * spark.kubernetes.container.image为excutor的镜像文件 。

     * spark.kubernetes.namespace为excutor在k8s上运行的命名空间。可以通过限制namespace的cpu、内存等资源来达到限制spark计算资源的目的

     * spark.driver.host为driver的主机地址，pyspark目前只支持client模式，该模式下driver就是该jupyterlab环境，所以该主机地址在executor中是可访问的。需要为该jupyterlab pod建立一个Headless Service

   * 编写完kernel.json再次使用jupyter kernelspec list就可以看到相应的pyspark kernel了

   * 刷新jupyterlab界面就可以看到对应的notebook了

     ![1551758588901](C:\Users\gaofengbin\AppData\Roaming\Typora\typora-user-images\1551758588901.png)

3. 编写初始化kernel.json的启动脚本pyspark-kernel.sh

   该脚本的作用是初始化kernel.json里的动态参数

   ```shell
   #!/usr/bin/env bash
   
   KERNELPATH=/opt/conda/share/jupyter/kernels/pyspark/kernel.json
   
   set -x
   
   sed  -i "s/SPARKMASTER/$SPARKMASTER/g;s/IMAGEREPO/$IMAGEREPO/g;s/SPARKIMAGE/$SPARKIMAGE/g;s/SAPRKNS/$SAPRKNS/g;s/DRIVERHOST/$JUPYTERHUB_USER/g" $KERNELPATH
   ```

   jupyterlab容器启动时加载该初始化脚本， . /usr/local/bin/pyspark-kernel.sh

4. k8s  kubeflow命名空间创建相应的configmap

   ```yaml
   apiVersion: v1
   kind: ConfigMap
   metadata:
     name: spark-config
     namespace: kubeflow
   data:
     SPARKMASTER: 172.25.58.1:8043
     IMAGEREPO: 172.25.58.1:8000
     SPARKIMAGE: spark-py37:2.4.0
     SAPRKNS: spark-test
   ```

   

5. 修改镜像生成代码，设置上述参数的值

   kubespawner源码中有关于容器的额外配置设置，代码如下：

   ```python
   extra_container_config = Dict(
           config=True,
           help="""
           Extra configuration (e.g. ``envFrom``) for notebook container which is not covered by other attributes.
   
           This dict will be directly merge into `container` of notebook server,
           so you should use the same structure. Each item in the dict must a field
           of the `V1Container specification <https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.11/#container-v1-core>`_.
   
           One usage is set ``envFrom`` on notebook container with configuration below::
   
               c.KubeSpawner.extra_container_config = {
                   "envFrom": [{
                       "configMapRef": {
                           "name": "special-config"
                       }
                   }]
               }
   
           The key could be either a camelCase word (used by Kubernetes yaml, e.g.
           ``envFrom``) or a snake_case word (used by Kubernetes Python client,
           e.g. ``env_from``).
           """
       )
   ```

   修改/kfapp/ks_app/vendor/kubeflow/core/kubeform_spawner.py

   ```python
   c.KubeSpawner.extra_container_config = {
       "envFrom": [{
           "configMapRef": {
               "name": "spark-config"
           }
       }]
   }
   ```

   ks apply default -c jupyterhub  更新设置，并重启jupyterhub容器

6. 创建该jupyterlab的Headless Service

   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: kuai1-1-gaofengbin
     namespace: kubeflow
   spec:
     clusterIP: None
     ports:
     - port: 4040
       protocol: TCP
       targetPort: 4040
     selector:
       app-name: kuai1-1-gaofengbin
     type: ClusterIP
   ```

   这个服务的创建暂时先放在proxy代码里

7. 给jupyterlab  serviceaccount 赋予权限

   ```shell
   kubectl create clusterrolebinding spark-role --clusterrole=cluster-admin --serviceaccount=kubeflow:jupyter-notebook --namespace=kubeflow
   ```

   否则jupyterlab无法管理别的namespace，无法创建executor容器

8. 安装监控插件

   ```shell
   source activate base
   jupyter nbextension install --py sparkmonitor --symlink --sys-prefix
   jupyter nbextension enable sparkmonitor --py --sys-prefix
   jupyter serverextension enable --py sparkmonitor --sys-prefix
   ipython profile create
   echo "c.InteractiveShellApp.extensions.append('sparkmonitor.kernelextension')" >>  $(ipython profile locate default)/ipython_kernel_config.py
   ```

   

9. 使用

   ```python
   import pyspark
   import os
   import pandas as pd
   from pyspark import SparkConf, SparkContext
   from pyspark.sql import SparkSession, HiveContext
   from operator import add
   from random import random
   
   sparkConf = SparkConf()
   #以下注释的几个配置已内置在内核中无需配置
   #sparkConf.setMaster('k8s://https://172.25.58.1:8043') #设置master，
   #sparkConf.set("spark.kubernetes.container.image", "172.25.58.1:8000/spark-py:2.4.0") executor镜像
   #sparkConf.set("spark.kubernetes.namespace", "spark-test") 命名空间用于资源隔离
   #sparkConf.set("spark.app.name", "pi-test") app name
   #sparkConf.set("spark.driver.host", "spark-test-pod.lab.svc") driver host
   #sparkConf.set("hive.metastore.uris", "thrift://YZ-25-65-49.h.chinabank.com.cn:9083,thrift://YZ-25-65-50.h.chinabank.com.cn:9083")  hive元数据
   sparkConf.set("spark.executor.instances", "3") #实例数
   sparkConf.set("spark.executor.cores", "2") #单实例cpu数
   sparkConf.set("spark.executor.memory", "4G") #单实例内存大小
   spark = SparkSession.builder.config(conf=sparkConf).enableHiveSupport().getOrCreate()
   sc = spark.sparkContext
   
   #pi计算示例
   from operator import add
   partitions = 100000
   n = 1000 * partitions
   
   def f(_):
       x = random() * 2 - 1
       y = random() * 2 - 1
       return 1 if x ** 2 + y ** 2 <= 1 else 0
   
   count = spark.sparkContext.parallelize(range(1, n + 1), partitions).map(f).reduce(add)
   print("Pi is roughly %f" % (4.0 * count / n))
   ```

   

10. spark读取数据

    executor节点需用当前erp用户账号启动，并且在sc创建的时候设置环境变量来指定用户

    ```python
    sparkConf.set("spark.executorEnv.USER_NAME", "gongjuntai")
    ```

    修改spark-py37:2.4.0镜像启动脚本文件/opt/entrypoint.sh

    ```shell
    
    ```

    

