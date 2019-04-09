# 存储

### 问题调研

#### cephFS配额管理

1. 创建用户对应的osd pool，然后设置pool的配额大小，然后指定用户目录为该pool。***但是该配额对cephfs不生效***

#### cephfs-provisioner部署

<https://github.com/kubernetes-incubator/external-storage>    代码下载

https://github.com/kubernetes-incubator/external-storage/issues/1120#issuecomment-460684760  根据该评论修改代码，否则生成的镜像运行时会报错

```shell
[root@667a0ae61bf9 bin]# ./cephfs-provisioner 
./cephfs-provisioner flag redefined: log_dir
panic: ./cephfs-provisioner flag redefined: log_dir

goroutine 1 [running]:
flag.(*FlagSet).Var(0xc000078180, 0x1461500, 0x1e3f830, 0x12edf51, 0x7, 0x131684e, 0x2f)
        /usr/lib/golang/src/flag/flag.go:805 +0x529
flag.(*FlagSet).StringVar(0xc000078180, 0x1e3f830, 0x12edf51, 0x7, 0x0, 0x0, 0x131684e, 0x2f)
        /usr/lib/golang/src/flag/flag.go:708 +0x8a
k8s.io/klog.InitFlags(0xc000078180)
        /home/gaofengbin/external-storage-master/vendor/src/k8s.io/klog/klog.go:411 +0x7b
main.main()
        /home/gaofengbin/external-storage-master/ceph/cephfs/cephfs-provisioner.go:388 +0x3f
```

#### ceph-fuse挂载

```shell
ceph-fuse -m 172.25.65.49:6789,172.25.65.50:6789,172.25.65.51:6789  -r /notebook subcephtest/
```

开机自动挂载cephfs，/etc/fstab中添加

```shell
none      /ceph_fs   fuse.ceph   ceph.id=admin,ceph.conf=/etc/ceph/ceph.conf,ceph.client_mountpoint=/YZ-25-65-49,_netdev,defaults  0 0
```

