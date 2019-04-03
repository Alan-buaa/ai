# ceph yum源设置

```shell
# ceph_release
# 12.2.10 luminous
# distro:CentOS 7是el7
ceph_release=luminous
distro=el7
sys_ver=centos7

cat > /etc/yum.repos.d/ceph.repo <<EOF
[ceph]
name=Ceph packages for \$basearch
baseurl=https://download.ceph.com/rpm-${ceph_release}/${distro}/\$basearch
enabled=1
gpgcheck=1
type=rpm-md
gpgkey=https://ceph.com/git/?p=ceph.git;a=blob_plain;f=keys/release.asc
[ceph-noarch]
name=Ceph noarch packages
baseurl=https://download.ceph.com/rpm-${ceph_release}/${distro}/noarch
enabled=1
gpgcheck=1
type=rpm-md
gpgkey=https://ceph.com/git/?p=ceph.git;a=blob_plain;f=keys/release.asc
[ceph-source]
name=Ceph source packages
baseurl=https://download.ceph.com/rpm-${ceph_release}/${distro}/SRPMS
enabled=0
gpgcheck=1
type=rpm-md
gpgkey=https://ceph.com/git/?p=ceph.git;a=blob_plain;f=keys/release.asc
EOF
```

