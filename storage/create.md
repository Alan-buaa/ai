# 云盘创建

### 前提条件

首先得创建cephfs

### 创建流程

1. 创建存储池

   ```shell
   [root@YZ-25-65-49 quotatest]# ceph osd pool create gaofengbin 128
   pool 'gaofengbin' created
   
   #查看存储池详情
   [root@YZ-25-65-49 quotatest]# ceph osd pool ls detail
   pool 10 'gaofengbin' replicated size 3 min_size 2 crush_rule 0 object_hash rjenkins pg_num 128 pgp_num 128 last_change 854 flags hashpspool stripe_width 0
   
   ```

   

2. 将以创建的存储池添加到元数据服务器（mds）

   ```shell
   [root@YZ-25-65-49 quotatest]# ceph fs ls
   name: kuaicephfs, metadata pool: cephfs_metadata, data pools: [cephfs_data ]
   [root@YZ-25-65-49 quotatest]# 
   [root@YZ-25-65-49 quotatest]# ceph mds add_data_pool gaofengbin
   added data pool 10 to fsmap
   [root@YZ-25-65-49 quotatest]# ceph fs ls
   name: kuaicephfs, metadata pool: cephfs_metadata, data pools: [cephfs_data gaofengbin ]
   ```

   

3. 在cephfs  /notebook下创建用户对应的目录

   

   <!--*目录只有经过定制才会有显式的布局，如果从没更改过，那么读取其布局时就会失败：这表明它会继承父目录的显式布局设置。*-->

   ```shell
   [root@YZ-25-65-49 notebook]# mkdir gaofengbin
   [root@YZ-25-65-49 notebook]# getfattr -n ceph.dir.layout gaofengbin/
   gaofengbin/: ceph.dir.layout: No such attribute
   [root@YZ-25-65-49 notebook]# setfattr -n ceph.dir.layout.stripe_count -v 2 gaofengbin/
   [root@YZ-25-65-49 notebook]# getfattr -n ceph.dir.layout gaofengbin/                  
   # file: gaofengbin/
   ceph.dir.layout="stripe_unit=4194304 stripe_count=2 object_size=4194304 pool=cephfs_data"
   ```

   

4. 修改该用户目录所属的存储池

   <!--读取布局时，存储池通常是以名字标识的。然而在极少数情况下，如存储池刚创建时，可能会输出 ID 。-->

   ```shell
   [root@YZ-25-65-49 notebook]# setfattr -n ceph.dir.layout.pool -v gaofengbin gaofengbin/
   [root@YZ-25-65-49 notebook]# getfattr -n ceph.dir.layout gaofengbin/                   
   # file: gaofengbin/
   ceph.dir.layout="stripe_unit=4194304 stripe_count=2 object_size=4194304 pool=10"
   
   ```

   

pool配额的设置对cephfs不生效，该功能主要支持radosgw 。该方式用来Cephfs多用户隔离

<https://www.jianshu.com/p/96a34485f0fc>

https://ceph-users.ceph.narkive.com/rolGXq1V/osd-pool-set-quota-behaviour-with-cephfs