# prometheus

#### grafana无法显示namespace及pod

![1569399336494](D:\document\book\ai\prometheus\kube-state-metrics-error.png)

其中8443那个端口state会显示DOWN状态，Error列会显示context deadline exceeded



查看pod状态正常。

https://github.com/kubernetes/kube-state-metrics/issues/257

修改

![1569399573808](D:\document\book\ai\prometheus\deploy.png)

上述配置中limit 原来为cpu 20m，memory 40Mi。后改为cpu 200m，memory 400Mi。然后服务正常