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

     

2. 