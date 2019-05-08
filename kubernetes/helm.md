# helm

<https://www.jianshu.com/p/2bb1dfdadee8>

### helm 安装

1. 下载二进制文件

   <https://github.com/helm/helm/releases>

   解压并将helm复制到/usr/bin目录下

### httpd服务

helm init会 ***Adding stable repo with URL: https://kubernetes-charts.storage.googleapis.com***

```shell
[k8s@YZ-25-58-1 ~]$ helm init
Creating /home/k8s/.helm 
Creating /home/k8s/.helm/repository 
Creating /home/k8s/.helm/repository/cache 
Creating /home/k8s/.helm/repository/local 
Creating /home/k8s/.helm/plugins 
Creating /home/k8s/.helm/starters 
Creating /home/k8s/.helm/cache/archive 
Creating /home/k8s/.helm/repository/repositories.yaml 
Adding stable repo with URL: https://kubernetes-charts.storage.googleapis.com 
Error: Looks like "https://kubernetes-charts.storage.googleapis.com" is not a valid chart repository or cannot be reached: Get https://kubernetes-charts.storage.googleapis.com/index.yaml: dial tcp 172.217.160.80:443: connect: connection timed out
[k8s@YZ-25-58-1 ~]$
```

由于生产环境无法访问外网，故需自建httpd提供stable repo服务

修改httpd镜像中http.conf配置***ServerName localhost:80***否则启动会报***httpd: Could not reliably determine the server's fully qualified domain name***错误

```shell
docker run -dit -p 9000:80  --name k8s-charts-repo --restart=always -v /export/data/k8s_charts_repo:/var/www/html 10.220.54.6:8000/httpd httpd -DFOREGROUND
```

其中/export/data/k8s_charts_repo为物理节点上存放https://kubernetes.oss-cn-hangzhou.aliyuncs.com/charts/index.yaml的位置。 -DFOREGROUND参数保证httpd在前台运行，否则docker一启动就会变成Exited状态



### tiller 安装

本文采用将tiller安装到k8s集群的安装方式，既使用helm init

1. 利用helm init --dry-run --debug  查看安装生成的配置，主要查看需要的image文件

2. 通过gcr.io的国内代理下载镜像docker pull gcr.azk8s.cn/kubernetes-helm/tiller:v2.13.1

3. 创建 tiller service account， tiller.yaml

   参考<https://helm.sh/docs/using_helm/#role-based-access-control>

   ```shell
   apiVersion: v1
   kind: ServiceAccount
   metadata:
     name: tiller
     namespace: kube-system
   ---
   apiVersion: rbac.authorization.k8s.io/v1
   kind: ClusterRoleBinding
   metadata:
     name: tiller
   roleRef:
     apiGroup: rbac.authorization.k8s.io
     kind: ClusterRole
     name: cluster-admin
   subjects:
     - kind: ServiceAccount
       name: tiller
       namespace: kube-system
   ```

   

4. 初始化helm

   ```shell
   helm init --upgrade --service-account tiller --tiller-image 10.220.54.6:8000/kubernetes-helm/tiller:v2.13.1 --stable-repo-url http://10.220.54.4:9000/
   ```

   

   ```shell
   [k8s@TX-220-54-4 ~]$ helm init --upgrade --service-account tiller --tiller-image 10.220.54.6:8000/kubernetes-helm/tiller:v2.13.1 --stable-repo-url http://10.220.54.4:9000/
   Creating /home/k8s/.helm 
   Creating /home/k8s/.helm/repository 
   Creating /home/k8s/.helm/repository/cache 
   Creating /home/k8s/.helm/repository/local 
   Creating /home/k8s/.helm/plugins 
   Creating /home/k8s/.helm/starters 
   Creating /home/k8s/.helm/cache/archive 
   Creating /home/k8s/.helm/repository/repositories.yaml 
   Adding stable repo with URL: http://10.220.54.4:9000/ 
   Adding local repo with URL: http://127.0.0.1:8879/charts 
   $HELM_HOME has been configured at /home/k8s/.helm.
   
   Tiller (the Helm server-side component) has been installed into your Kubernetes Cluster.
   
   Please note: by default, Tiller is deployed with an insecure 'allow unauthenticated users' policy.
   To prevent this, run `helm init` with the --tiller-tls-verify flag.
   For more information on securing your installation see: https://docs.helm.sh/using_helm/#securing-your-helm-installation
   Happy Helming!
   ```

   

### 本地仓库开启

<https://www.jianshu.com/p/3fdb7ab391e0>

```shell
nohup helm serve --repo-path /export/data/local_charts_repo >> /export/data/local_charts_repo/logs/repo.log 2>&1 &
```

helm package会将打好的包添加到本地库中，所以需对路径做软链，否则安装的时候找不到包

```shell
ln -s /export/data/local_charts_repo/ local
```



### 离线chart包安装

1. 从<https://github.com/helm/charts>下载包放在TX-220-54-4节点/export/data/local_charts_repo目录下

并解压

2. 下载/export/data/local_charts_repo/charts/incubator/sparkoperator/values.yaml中的镜像文件并修改为本地镜像

3. 打包charts   helm package ../sparkoperator/，生成.tgz文件

   ```shell
   [k8s@TX-220-54-4 sparkoperator]$ helm package ../sparkoperator/
   Successfully packaged chart and saved it to: /export/data/local_charts_repo/charts/incubator/sparkoperator/sparkoperator-0.2.2.tgz
   [k8s@TX-220-54-4 sparkoperator]$ ll
   total 28
   -rw-rw-r-- 1 k8s k8s  311 May  7 05:41 Chart.yaml
   -rw-rw-r-- 1 k8s k8s   80 May  7 05:41 OWNERS
   -rw-rw-r-- 1 k8s k8s 3875 May  7 05:41 README.md
   -rw-rw-r-- 1 k8s k8s 4361 May  7 11:45 sparkoperator-0.2.2.tgz
   drwxrwxr-x 2 k8s k8s 4096 May  7 11:40 templates
   -rw-rw-r-- 1 k8s k8s  612 May  7 11:39 values.yaml
   [k8s@TX-220-54-4 sparkoperator]$ 
   ```

   查看本地仓库是否有sparkoperator的tgz包,

   再切换到本地仓库的目录下，发现也生成了**.tgz**文件，还有一个**index.yaml**文件，里面记录了一些仓库的信息。

   ```shell
   [k8s@TX-220-54-4 sparkoperator]$ helm search sparkoperator
   NAME                    CHART VERSION   APP VERSION             DESCRIPTION                                  
   local/sparkoperator     0.2.2           v2.4.0-v1beta1-0.8.2    A Helm chart for Spark on Kubernetes operator
   [k8s@TX-220-54-4 sparkoperator]$ ll /home/k8s/.helm/repository/local/
   index.yaml               sparkoperator-0.2.2.tgz  
   ```

   安装chart包

   ```shell
   [k8s@TX-220-54-4 repository]$ helm list
   [k8s@TX-220-54-4 repository]$ 
   [k8s@TX-220-54-4 repository]$ 
   [k8s@TX-220-54-4 repository]$ helm install local/sparkoperator --namespace spark-operator
   NAME:   alliterating-raccoon
   LAST DEPLOYED: Tue May  7 13:13:28 2019
   NAMESPACE: spark-operator
   STATUS: DEPLOYED
   
   RESOURCES:
   ==> v1/ClusterRole
   NAME                                   AGE
   alliterating-raccoon-sparkoperator-cr  21s
   
   ==> v1/ClusterRoleBinding
   NAME                                    AGE
   alliterating-raccoon-sparkoperator-crb  21s
   
   ==> v1/Deployment
   NAME                                READY  UP-TO-DATE  AVAILABLE  AGE
   alliterating-raccoon-sparkoperator  0/1    1           0          21s
   
   ==> v1/Pod(related)
   NAME                                                 READY  STATUS             RESTARTS  AGE
   alliterating-raccoon-sparkoperator-574fc7fb5d-xf2rj  0/1    ContainerCreating  0         21s
   
   ==> v1/Service
   NAME                          TYPE       CLUSTER-IP    EXTERNAL-IP  PORT(S)  AGE
   alliterating-raccoon-webhook  ClusterIP  10.100.37.69  <none>       443/TCP  21s
   
   ==> v1/ServiceAccount
   NAME                                SECRETS  AGE
   alliterating-raccoon-spark          1        21s
   alliterating-raccoon-sparkoperator  1        21s
   
   
   [k8s@TX-220-54-4 repository]$ helm list
   NAME                    REVISION        UPDATED                         STATUS          CHART                   APP VERSION             NAMESPACE     
   alliterating-raccoon    1               Tue May  7 13:13:28 2019        DEPLOYED        sparkoperator-0.2.2     v2.4.0-v1beta1-0.8.2    spark-operator
   [k8s@TX-220-54-4 repository]$ 
   ```

   

