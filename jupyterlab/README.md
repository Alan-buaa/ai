# jupyterlab

安装完插件，需修改/opt/conda/share/jupyter/lab/static/vendors~main.753bbb78d3f75d02408d.js文件中的按钮，与已修改历史文件对比



### 新kernel安装

```shell
source activate envname
unset PIP_CONFIG_FILE
pip install --upgrade pip
pip install tornado==5.1.1
pip install ipython==5.8.0
pip install ipykernel==4.10.0
python -m ipykernel install --user --name envname --display-name xbg0.6
```

