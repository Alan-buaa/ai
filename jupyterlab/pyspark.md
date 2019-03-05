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