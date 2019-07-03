# conda

### 安装时动态链接库的问题

<https://blog.51cto.com/wutengfei/2095715>

```shell
[root@jupyter-zhangjinsheng7-aa5nzwx9 python]# python setup.py build                             
running build
running build_py
running build_ext
building '_CRFPP' extension
gcc -pthread -B /opt/conda/compiler_compat -Wl,--sysroot=/ -Wsign-compare -DNDEBUG -g -fwrapv -O3 -Wall -Wstrict-prototypes -fPIC -I/opt/conda/include/python3.6m -c CRFPP_wrap.cxx -o build/temp.linux-x86_64-3.6/CRFPP_wrap.o
cc1plus: warning: command line option '-Wstrict-prototypes' is valid for C/ObjC but not for C++
CRFPP_wrap.cxx: In function 'PyObject* PyInit__CRFPP()':
CRFPP_wrap.cxx:5872:21: warning: variable 'md' set but not used [-Wunused-but-set-variable]
   PyObject *m, *d, *md;
                     ^~
g++ -pthread -shared -B /opt/conda/compiler_compat -L/opt/conda/lib -Wl,-rpath=/opt/conda/lib -Wl,--no-as-needed -Wl,--sysroot=/ build/temp.linux-x86_64-3.6/CRFPP_wrap.o -lcrfpp -lpthread -o build/lib.linux-x86_64-3.6/_CRFPP.cpython-36m-x86_64-linux-gnu.so
/opt/conda/compiler_compat/ld: cannot find -lcrfpp
collect2: error: ld returned 1 exit status
error: command 'g++' failed with exit status 1
[root@jupyter-zhangjinsheng7-aa5nzwx9 python]# 
```

报/opt/conda/compiler_compat/ld: cannot find -lcrfpp 找不到动态库。  

其中-rpath=/opt/conda/lib为conda的lib库，将相应的so文件链接到该库里