# 集群管理

### 分配pod到指定物理节点

1. 给指定物理节点增加标签

   ```shell
   kubectl label nodes <node-name> <label-key>=<label-value>
   ```

   修改标签

   ```shell
   kubectl label nodes <node-name> <label-key>=<label-value> --overwrite
   ```

   删除标签

   ```shell
   kubectl label nodes <node-name> <label-key>-
   ```

   

   验证

   ```shell
   kubectl get nodes --show-labels
   kubectl describe node "nodename"
   ```

   

2. pod增加nodeselector

   ```shell
   apiVersion: v1
   kind: Pod
   metadata:
     name: nginx
     labels:
       env: test
   spec:
     containers:
     - name: nginx
       image: nginx
       imagePullPolicy: IfNotPresent
     nodeSelector:
       disktype: ssd
   ```

   

### 设置主节点可调度

1. 可调度：kubectl taint node k8s-master node-role.kubernetes.io/master-
2. 不可调度：kubectl taint node k8s-master node-role.kubernetes.io/master=""

### 节点kubelet修改参数

修改日志级别，修改/etc/systemd/system/kubelet.service.d/10-kubeadm.conf

```shell
# Note: This dropin only works with kubeadm and kubelet v1.11+
[Service]
Environment="KUBELET_KUBECONFIG_ARGS=--bootstrap-kubeconfig=/etc/kubernetes/bootstrap-kubelet.conf --kubeconfig=/etc/kubernetes/kubelet.conf"
Environment="KUBELET_CONFIG_ARGS=--config=/var/lib/kubelet/config.yaml"
# This is a file that "kubeadm init" and "kubeadm join" generates at runtime, populating the KUBELET_KUBEADM_ARGS variable dynamically
EnvironmentFile=-/var/lib/kubelet/kubeadm-flags.env
# This is a file that the user can use for overrides of the kubelet args as a last resort. Preferably, the user should use
# the .NodeRegistration.KubeletExtraArgs object in the configuration files instead. KUBELET_EXTRA_ARGS should be sourced from this file.
EnvironmentFile=-/etc/sysconfig/kubelet
ExecStart=
ExecStart=/usr/bin/kubelet -v 4 $KUBELET_KUBECONFIG_ARGS $KUBELET_CONFIG_ARGS $KUBELET_KUBEADM_ARGS $KUBELET_EXTRA_ARGS
```

最后一行里的 -v 4代表日志级别，然后重启服务

```shell
 systemctl daemon-reload && systemctl restart kubelet
```

