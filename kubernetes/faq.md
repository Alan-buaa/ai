# FAQ

##### kubernetes pull镜像问题

当拉取私有镜像仓库的镜像时需要账户名密码。

解决方案：

1. 首先在namespace下创建secret

   ```shell
   kubectl create secret docker-registry regsecret \
       --docker-server=10.220.54.6:8088 \
       --docker-username=admin \
       --docker-password=KuAI2019 \
       --docker-email=admin@example.com   -n kubeflow
   ```

2. 然后在创建容器的时候使用该secret

   * 将该secret patch到此namespace下的serviceaccout上

     ```shell
     kubectl patch serviceaccount jupyter-notebook -p '{"imagePullSecrets": [{"name": "regsecret"}]}' -n kubeflow
     ```

   * 创建pod时指定 .withImagePullSecrets(secrets)

##### kubectl exec连接容器空闲超时时间问题

https://github.com/kubernetes/kubernetes/issues/66661

这个空闲超时时间由kubelet的参数--streaming-connection-idle-timeout设置默认为4h

<https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/>

linux系统的TMOUT也会影响空闲连接时长

还有kubectl和API server之间的代理会导致此问题。

我们系统使用的是haproxy

修改/etc/haproxy/haproxy.cfg里的 timeout client和timeout server参数

![](D:\document\book\ai\kubernetes\hacfg.png)