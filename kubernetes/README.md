# kubernetes

### gcr.io镜像国内代理

<http://mirror.azure.cn/help/gcr-proxy-cache.html>

```sh
docker pull gcr.azk8s.cn/google_containers/<imagename>:<version>
```

### master节点初始化

```shell
kubeadm init --kubernetes-version=v1.14.1 --pod-network-cidr=10.244.0.0/16 --apiserver-advertise-address=172.25.58.1 --apiserver-bind-port=8043  --image-repository=172.25.58.1:8000
```

