# maven私服搭建

<https://www.jianshu.com/p/3d65558997af>

1. 镜像下载docker pull sonatype/nexus3

2. 启动服务

   ```shell
   docker run --restart=always -d --privileged=true -p 8081:8081 --name nexus -v /export/data/maven_repo:/var/nexus-data 10.220.54.6:8000/nexus3
   ```

3. 点击右上方的Sign in进行登录，初始账号密码为`admin/admin123`.请登录后修改密码