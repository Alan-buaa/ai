# spark

### no port number

![](D:\document\book\ai\faq\no port num.png)

```shell
2019-05-05 11:58:12 WARN  MemoryStore:66 - Not enough space to cache rdd_17_52 in memory! (computed 34.5 MB so far)
2019-05-05 11:58:12 INFO  MemoryStore:54 - Memory use = 378.8 MB (blocks) + 34.4 MB (scratch space shared across 1 tasks(s)) = 413.2 MB. Storage limit = 413.9 MB.
2019-05-05 11:58:12 INFO  BlockManager:54 - Found block rdd_17_52 locally
2019-05-05 11:58:13 INFO  Executor:54 - Finished task 52.0 in stage 814.0 (TID 25633). 2364 bytes result sent to driver
2019-05-05 11:58:13 INFO  CoarseGrainedExecutorBackend:54 - Got assigned task 25635
2019-05-05 11:58:13 INFO  Executor:54 - Running task 54.0 in stage 814.0 (TID 25635)
2019-05-05 11:58:13 INFO  BlockManager:54 - Found block rdd_17_54 locally
2019-05-05 11:58:13 INFO  Executor:54 - Finished task 54.0 in stage 814.0 (TID 25635). 2321 bytes result sent to driver
2019-05-05 11:58:13 INFO  CoarseGrainedExecutorBackend:54 - Got assigned task 25637
2019-05-05 11:58:13 INFO  Executor:54 - Running task 1.0 in stage 815.0 (TID 25637)
2019-05-05 11:58:13 INFO  MapOutputTrackerWorker:54 - Updating epoch to 407 and clearing cache
2019-05-05 11:58:13 INFO  TorrentBroadcast:54 - Started reading broadcast variable 1223
2019-05-05 11:58:13 INFO  MemoryStore:54 - Block broadcast_1223_piece0 stored as bytes in memory (estimated size 104.8 KB, free 35.1 MB)
2019-05-05 11:58:13 INFO  TorrentBroadcast:54 - Reading broadcast variable 1223 took 6 ms
2019-05-05 11:58:13 INFO  MemoryStore:54 - Block broadcast_1223 stored as values in memory (estimated size 341.8 KB, free 34.7 MB)
2019-05-05 11:58:13 INFO  MapOutputTrackerWorker:54 - Don't have map outputs for shuffle 406, fetching them
2019-05-05 11:58:13 INFO  MapOutputTrackerWorker:54 - Doing the fetch; tracker endpoint = NettyRpcEndpointRef(spark://MapOutputTracker@pangweidong-114-scmaturityloss.kubeflow:44765)
2019-05-05 11:58:13 INFO  MapOutputTrackerWorker:54 - Got the output locations
2019-05-05 11:58:13 INFO  ShuffleBlockFetcherIterator:54 - Getting 8 non-empty blocks including 4 local blocks and 4 remote blocks
2019-05-05 11:58:13 INFO  ShuffleBlockFetcherIterator:54 - Started 1 remote fetches in 0 ms
2019-05-05 11:58:13 INFO  Executor:54 - Finished task 1.0 in stage 815.0 (TID 25637). 7527 bytes result sent to driver
2019-05-05 11:58:13 INFO  CoarseGrainedExecutorBackend:54 - Got assigned task 25638
2019-05-05 11:58:13 INFO  Executor:54 - Running task 2.0 in stage 815.0 (TID 25638)
2019-05-05 11:58:13 INFO  ShuffleBlockFetcherIterator:54 - Getting 8 non-empty blocks including 4 local blocks and 4 remote blocks
2019-05-05 11:58:13 INFO  ShuffleBlockFetcherIterator:54 - Started 1 remote fetches in 0 ms
2019-05-05 11:58:13 INFO  Executor:54 - Finished task 2.0 in stage 815.0 (TID 25638). 7527 bytes result sent to driver
2019-05-05 11:58:13 INFO  CoarseGrainedExecutorBackend:54 - Got assigned task 25640
2019-05-05 11:58:13 INFO  Executor:54 - Running task 4.0 in stage 815.0 (TID 25640)
2019-05-05 11:58:13 INFO  ShuffleBlockFetcherIterator:54 - Getting 8 non-empty blocks including 4 local blocks and 4 remote blocks
2019-05-05 11:58:13 INFO  ShuffleBlockFetcherIterator:54 - Started 1 remote fetches in 0 ms
2019-05-05 11:58:13 INFO  Executor:54 - Finished task 4.0 in stage 815.0 (TID 25640). 7527 bytes result sent to driver
2019-05-05 11:58:13 INFO  BlockManager:54 - Removing RDD 17
2019-05-05 11:58:15 INFO  CoarseGrainedExecutorBackend:54 - Got assigned task 25643
2019-05-05 11:58:15 INFO  Executor:54 - Running task 0.0 in stage 816.0 (TID 25643)
2019-05-05 11:58:15 INFO  TorrentBroadcast:54 - Started reading broadcast variable 1224
2019-05-05 11:58:15 INFO  MemoryStore:54 - Block broadcast_1224_piece0 stored as bytes in memory (estimated size 114.2 KB, free 413.8 MB)
2019-05-05 11:58:15 INFO  TorrentBroadcast:54 - Reading broadcast variable 1224 took 5 ms
2019-05-05 11:58:15 INFO  MemoryStore:54 - Block broadcast_1224 stored as values in memory (estimated size 354.6 KB, free 413.5 MB)
2019-05-05 11:58:15 INFO  HadoopRDD:54 - Input split: hdfs://hdfs-namenode.hdfs:8020/tmp/testdata_scan.txt:0+134217728
2019-05-05 11:58:15 INFO  MemoryStore:54 - Block broadcast_0 stored as values in memory (estimated size 343.9 KB, free 413.1 MB)
2019-05-05 11:58:15 WARN  PythonWorkerFactory:87 - Failed to open socket to Python daemon:
java.net.ConnectException: Connection refused (Connection refused)
	at java.net.PlainSocketImpl.socketConnect(Native Method)
	at java.net.AbstractPlainSocketImpl.doConnect(AbstractPlainSocketImpl.java:350)
	at java.net.AbstractPlainSocketImpl.connectToAddress(AbstractPlainSocketImpl.java:206)
	at java.net.AbstractPlainSocketImpl.connect(AbstractPlainSocketImpl.java:188)
	at java.net.SocksSocketImpl.connect(SocksSocketImpl.java:392)
	at java.net.Socket.connect(Socket.java:589)
	at java.net.Socket.connect(Socket.java:538)
	at java.net.Socket.<init>(Socket.java:434)
	at java.net.Socket.<init>(Socket.java:244)
	at org.apache.spark.api.python.PythonWorkerFactory.createSocket$1(PythonWorkerFactory.scala:109)
	at org.apache.spark.api.python.PythonWorkerFactory.liftedTree1$1(PythonWorkerFactory.scala:126)
	at org.apache.spark.api.python.PythonWorkerFactory.createThroughDaemon(PythonWorkerFactory.scala:125)
	at org.apache.spark.api.python.PythonWorkerFactory.create(PythonWorkerFactory.scala:95)
	at org.apache.spark.SparkEnv.createPythonWorker(SparkEnv.scala:117)
	at org.apache.spark.api.python.BasePythonRunner.compute(PythonRunner.scala:108)
	at org.apache.spark.api.python.PythonRDD.compute(PythonRDD.scala:65)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:324)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:288)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:52)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:324)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:288)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:52)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:324)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:288)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:52)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:324)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:288)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:52)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:324)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:288)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:52)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:324)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:288)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:52)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:324)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:288)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:52)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:324)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:288)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:52)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:324)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:288)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:52)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:324)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:288)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:52)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:324)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:288)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:52)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:324)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:288)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:52)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:324)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:288)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:52)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:324)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:288)
	at org.apache.spark.scheduler.ShuffleMapTask.runTask(ShuffleMapTask.scala:99)
	at org.apache.spark.scheduler.ShuffleMapTask.runTask(ShuffleMapTask.scala:55)
	at org.apache.spark.scheduler.Task.run(Task.scala:121)
	at org.apache.spark.executor.Executor$TaskRunner$$anonfun$10.apply(Executor.scala:402)
	at org.apache.spark.util.Utils$.tryWithSafeFinally(Utils.scala:1360)
	at org.apache.spark.executor.Executor$TaskRunner.run(Executor.scala:408)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
	at java.lang.Thread.run(Thread.java:748)
2019-05-05 11:58:15 WARN  PythonWorkerFactory:66 - Assuming that daemon unexpectedly quit, attempting to restart
2019-05-05 11:58:15 ERROR Executor:91 - Exception in task 0.0 in stage 816.0 (TID 25643)
org.apache.spark.SparkException: No port number in pyspark.daemon's stdout
	at org.apache.spark.api.python.PythonWorkerFactory.startDaemon(PythonWorkerFactory.scala:204)
	at org.apache.spark.api.python.PythonWorkerFactory.liftedTree1$1(PythonWorkerFactory.scala:132)
	at org.apache.spark.api.python.PythonWorkerFactory.createThroughDaemon(PythonWorkerFactory.scala:125)
	at org.apache.spark.api.python.PythonWorkerFactory.create(PythonWorkerFactory.scala:95)
	at org.apache.spark.SparkEnv.createPythonWorker(SparkEnv.scala:117)
	at org.apache.spark.api.python.BasePythonRunner.compute(PythonRunner.scala:108)
	at org.apache.spark.api.python.PythonRDD.compute(PythonRDD.scala:65)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:324)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:288)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:52)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:324)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:288)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:52)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:324)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:288)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:52)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:324)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:288)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:52)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:324)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:288)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:52)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:324)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:288)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:52)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:324)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:288)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:52)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:324)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:288)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:52)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:324)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:288)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:52)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:324)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:288)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:52)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:324)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:288)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:52)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:324)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:288)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:52)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:324)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:288)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:52)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:324)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:288)
	at org.apache.spark.scheduler.ShuffleMapTask.runTask(ShuffleMapTask.scala:99)
	at org.apache.spark.scheduler.ShuffleMapTask.runTask(ShuffleMapTask.scala:55)
	at org.apache.spark.scheduler.Task.run(Task.scala:121)
	at org.apache.spark.executor.Executor$TaskRunner$$anonfun$10.apply(Executor.scala:402)
	at org.apache.spark.util.Utils$.tryWithSafeFinally(Utils.scala:1360)
	at org.apache.spark.executor.Executor$TaskRunner.run(Executor.scala:408)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
	at java.lang.Thread.run(Thread.java:748)
2019-05-05 11:58:15 INFO  CoarseGrainedExecutorBackend:54 - Got assigned task 25645
2019-05-05 11:58:15 INFO  Executor:54 - Running task 2.0 in stage 816.0 (TID 25645)
2019-05-05 11:58:15 INFO  HadoopRDD:54 - Input split: hdfs://hdfs-namenode.hdfs:8020/tmp/testdata_scan.txt:268435456+134217728
2019-05-05 11:58:15 ERROR Executor:91 - Exception in task 2.0 in stage 816.0 (TID 25645)
```

日志中显示***MemoryStore:66 - Not enough space to cache rdd_17_52 in memory! (computed 34.5 MB so far)***没有内存空间。然后就发生了异常，查看系统进程

![](D:\document\book\ai\faq\no_pyspark_daemon.png)



![](D:\document\book\ai\faq\pyspark_daemon.png)

第一张图中pyspark.daemon已经挂掉。



启动spark环境的时候注意设置足够的内存空间