# yum源搭建

1. apache服务器搭建

   * 安装httpd    yum install -y httpd

   * 修改httpd配置    /etc/httpd/conf/httpd.conf

     ```shell
     DocumentRoot "/export/grid/01/yumrepo"   修改DocumentRoot为放rpm包的目录， 并且如下directory的配置要有访问权限
     
     <Directory "/export/grid/01/yumrepo">
         #
         # Possible values for the Options directive are "None", "All",
         # or any combination of:
         #   Indexes Includes FollowSymLinks SymLinksifOwnerMatch ExecCGI MultiViews
         #
         # Note that "MultiViews" must be named *explicitly* --- "Options All"
         # doesn't give it to you.
         #
         # The Options directive is both complicated and important.  Please see
         # http://httpd.apache.org/docs/2.4/mod/core.html#options
         # for more information.
         #
         Options Indexes FollowSymLinks
     
         #
         # AllowOverride controls what directives may be placed in .htaccess files.
         # It can be "All", "None", or any combination of the keywords:
         #   Options FileInfo AuthConfig Limit
         #
         AllowOverride None
     
         #
         # Controls who can get stuff from this server.
         #
         Require all granted
     </Directory>
     ```

     

   * 启动httpd服务，

     ```shell
     systemctl start httpd.service 
     systemctl enable httpd
     ```

     

2. 挂载镜像

   * 创建目录，放包

     在/export/grid/01/yumrepo目录下创建centos目录，在centos目录下，创建extra，update，x86_64三个平行目录，将要管理的包文件放在对应目录下

   * 依次对三个目录进行createrepo操作，目的是生成repodata目录，自动创建索引信息

     ```shell
     createrepo -pdo /export/grid/01/yumrepo/centos/el7/x86_64 /export/grid/01/yumrepo/centos/el7/x86_64
     ```

     

3. 客户端访问yum源服务器

   在客户端/etc/yum.repos.d创建kuai.repo文件

   ```shell
   [base]
   name=local repo
   baseurl=http://10.220.54.1/centos/el7/x86_64
   enabled=1
   gpgcheck=0
   
   [noarch]
   name=local noarch
   baseurl=http://10.220.54.1/centos/el7/noarch
   enabled=1
   gpgcheck=0
   ```

   

**如果有新的包添加进了x86_64、extra、updates的任意一个目录中，都需要createrepo --update dir来更新yum源服务器的索引。客户端也需要yum makecache一下。**