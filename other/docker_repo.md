# docker私服搭建

* 安装docker

* 下载registry镜像

* 启动私服

  ```shell
  docker run -d -p 8000:5000  --name dockerRegistry --restart=always -v /export/App/docker_registry:/export/docker/registry 10.220.54.6:8000/registry
  ```

* 验证服务  

  ```shell
  curl http://10.220.54.6:8000/v2/_catalog
  ```

  

* 创建web服务

  ```shell
  docker run -d -p 8001:8080 --name registry-web --link dockerRegistry --restart=always -e REGISTRY_URL=http://10.220.54.6:8000/v2 -e REGISTRY_NAME=localhost:8000 10.220.54.6:8000/docker-registry-web 
  ```

  

