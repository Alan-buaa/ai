# 集群搭建

### 物理机要求

#### 软件

1. ceph-fuse(k8s会检测该节点是否支持用户态mount，否则会kernel mount)

### 物理机安装

1. 升级内核

   * 首先在下面网站查看最新版的内核

     http://mirror.rc.usf.edu/compute_lock/elrepo/kernel/el7/x86_64/RPMS/

   * 然后通过yum安装

     yum -y install kernel-ml-xxx

   * 修改启动配置并重启

     ```shell
     sed -i '/GRUB_DEFAULT/s/saved/0/' /etc/default/grub && grub2-mkconfig -o /boot/grub2/grub.cfg && shutdown -r now
     ```

2. kubeadm, kubelet, kubectl安装

   在有外网环境里设置kubernetes 阿里yum源

   ```shell
   cat <<EOF > /etc/yum.repos.d/kubernetes.repo
   [kubernetes]
   name=Kubernetes
   baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/
   enabled=1
   gpgcheck=1
   repo_gpgcheck=1
   gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
   EOF
   ```

   使用yum安装命令 只下载相关包，然后将相关包放入本地yum源里

   ```shell
   yum install --downloadonly --downloaddir=/home/gaofengbin/k8s kubelet kubeadm kubectl
   ```

   

3. 降级内核

   ```shell
   rpm -qa |grep kernel
   rpm -e kernel-ml-5.0.7-1.el7.elrepo.x86_64
   rpm -qa |grep kernel
   ```

   