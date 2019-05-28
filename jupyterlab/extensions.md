# extensions

### 已有扩展

https://www.npmjs.com/search?q=keywords:jupyterlab-extension

https://github.com/topics/jupyterlab-extension

### treefile

https://www.npmjs.com/package/jupyterlab_filetree

https://github.com/youngthejames/jupyterlab_filetree

### 已安装扩展

<https://github.com/lckr/jupyterlab-variableInspector>

### tensorboard

<https://github.com/lspvic/jupyter_tensorboard>

<https://github.com/chaoleili/jupyterlab_tensorboard>

1. 先安装jupyter_tensorboard  

   ```shell
   pip install jupyter-tensorboard
   ```

   

2. 由于上述安装会将jupyter serverextension、nbextension安装到/root目录下，故需改变jupyter扩展配置

   ```shell
    jupyter serverextension enable jupyter_tensorboard  --py --sys-prefix && \
    jupyter nbextension enable jupyter_tensorboard  --py --sys-prefix
   ```

   

