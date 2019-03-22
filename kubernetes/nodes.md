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

   

