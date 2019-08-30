# scala

使用almond来支持scala，https://almond.sh/

### 构建安装文件

需查看spark对应的scala版本

```shell
(base) [a@spark]$ spark-shell
2019-07-26 11:26:13 WARN  NativeCodeLoader:62 - Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Setting default log level to "WARN".
To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
Spark context Web UI available at http://xxx:4040
Spark context available as 'sc' (master = local[*], app id = local-1564111578790).
Spark session available as 'spark'.
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /___/ .__/\_,_/_/ /_/\_\   version 2.4.0
      /_/

Using Scala version 2.11.12 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_191)
Type in expressions to have them evaluated.
Type :help for more information.

scala>
```

在可以连接外网的环境里执行如下命令，如果环境不联网需指定本地的maven库

```shell
SCALA_VERSION=2.11.12 ALMOND_VERSION=0.6.0
curl -Lo coursier https://git.io/coursier-cli
chmod +x coursier
./coursier bootstrap \
  -r jitpack \
  -i user -I user:sh.almond:scala-kernel-api_$SCALA_VERSION:$ALMOND_VERSION \
  sh.almond:scala-kernel_$SCALA_VERSION:$ALMOND_VERSION \
  -o almond-scala-2.11.12
```

然后打包/root/.cache/目录下的coursier，将其解压到要安装的环境中

```shell

```

