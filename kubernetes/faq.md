# FAQ

##### kubectl exec连接容器空闲超时时间问题

https://github.com/kubernetes/kubernetes/issues/66661

这个空闲超时时间由kubelet的参数--streaming-connection-idle-timeout设置默认为4h

<https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/>

linux系统的TMOUT也会影响空闲连接时长

还有kubectl和API server之间的代理会导致此问题。

我们系统使用的是haproxy

修改/etc/haproxy/haproxy.cfg里的 timeout client和timeout server参数

![](D:\document\book\ai\kubernetes\hacfg.png)