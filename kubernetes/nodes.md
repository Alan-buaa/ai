# 节点管理

### 删除物理节点

1. 设置节点不可调度

   ```shell
   kubectl drain <node-name> --ignore-daemonsets --delete-local-data
   ```

   

2. 删除节点

   ```shell
   kubectl delete node <node-name>
   ```

   

### GPU节点的加入

注意：nvidia drivers和linux kernel 版本5兼容有问题，多卡跑任务会导致机器崩溃

1. 物理节点上安装nvidia drivers

   [^1]: <https://github.com/NVIDIA/nvidia-docker/wiki/CUDA#requirements>

   <https://www.cnblogs.com/mar-q/p/7482720.html>

   * <https://centos.pkgs.org/> 下载nvidia-detect rpm包，放在本地yum源上

   * 在物理节点上安装nvidia-detect包

   * 检测硬件版本

     ```shell
     [root@TX-220-54-25 script]# nvidia-detect -v
     Probing for supported NVIDIA devices...
     [10de:1db4] NVIDIA Corporation Device 1db4
     This device requires the current 418.43 NVIDIA driver kmod-nvidia
     [10de:1db4] NVIDIA Corporation Device 1db4
     This device requires the current 418.43 NVIDIA driver kmod-nvidia
     [10de:1db4] NVIDIA Corporation Device 1db4
     This device requires the current 418.43 NVIDIA driver kmod-nvidia
     [10de:1db4] NVIDIA Corporation Device 1db4
     This device requires the current 418.43 NVIDIA driver kmod-nvidia
     [10de:1db4] NVIDIA Corporation Device 1db4
     This device requires the current 418.43 NVIDIA driver kmod-nvidia
     [1a03:2000] ASPEED Technology, Inc. ASPEED Graphics Family
     [10de:1db4] NVIDIA Corporation Device 1db4
     This device requires the current 418.43 NVIDIA driver kmod-nvidia
     [10de:1db4] NVIDIA Corporation Device 1db4
     This device requires the current 418.43 NVIDIA driver kmod-nvidia
     [10de:1db4] NVIDIA Corporation Device 1db4
     This device requires the current 418.43 NVIDIA driver kmod-nvidia
    WARNING: Xorg log file /var/log/Xorg.0.log does not exist
     WARNING: Unable to determine Xorg ABI compatibility
     WARNING: The driver for this device does not support the current Xorg version
     ```

   * nvidia官网下载驱动

   * 驱动测试 docker run --runtime=nvidia --rm 10.220.54.6:8000/kuai-miniconda-gpu  nvidia-smi

   * `驱动卸载 sudo /usr/bin/nvidia-uninstall`

2. 安装nvidia-docker

   <https://github.com/NVIDIA/nvidia-docker>

   ![](D:\document\book\ai\kubernetes\nvidia_docker.png)
   
   在有外网权限机器上下载
   
   通过 yum search --showduplicates nvidia-docker 查找需要的版本

### 获取节点可分配资源

https://medium.com/@gorenje/kubernetes-api-allocatable-node-resources-6ea88f828f0

https://gist.github.com/gorenje/dff508489c3c8a460433ad709f14b7db