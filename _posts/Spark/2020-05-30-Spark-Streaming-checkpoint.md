---
layout: post
title: Spark Streaming checkpoint机制
categories: [Spark]
description: Spark Streaming checkpoint
keywords: spark streaming,checkpoint
---

## 1.checkpoint介绍

Checkpoint是一个在checkpoint时间间隔内将接收到的记录（通过输入dstream）写入高度可用的HDFS分布式存储的过程。它允许创建容错的流处理管道，因此当发生故障时，输入dstream可以恢复故障前的流状态并继续流处理。

DStreams可以在指定的时间间隔将输入数据保存在checkpoint中。
## 2.checkpoint的两种类型数据
### 2.1 Metadata checkpointing
将流式计算的信息保存到具备容错性的存储上比如HDFS，Metadata Checkpointing适用于当streaming应用程序Driver所在的节点出错时能够恢复。

元数据包括：
1. 配置信息：创建 Spark-Streaming 应用程序的配置信息，比如 SparkConf
2. DStream操作：在streaming应用程序中定义的DStreaming操作
3. 未处理完的 batch 信息：在队列中没有处理完的作业

### 2.2 Data checkpointing
将生成的RDD保存到外部可靠的存储当中，对于一些数据跨度为多个batch的有状态transforation（updateStateByKey和 reduceByKeyAndWindow）操作来说，checkpoint非常有必要，因为在这些transformation操作生成的RDD对前一RDD有依赖，随着时间的增加，依赖链可能非常长，checkpoint机制能够切断依赖链，将中间的RDD周期性地checkpoint到可靠存储当中，从而在出错时可以直接从checkpoint点恢复。

具体来说，metadata checkpointing主要还是从driver失败中恢复，而Data Checkpoint用于对有状态的transformation操作进行checkpointing
## 3.何时checkpoint
1. 使用了有状态转换，如果application中使用了updateStateByKey或者reduceByKeyAndWindow等stateful操作，必须提供checkpoint目录来允许定时的RDD checkpoint
2. 希望能从意外中恢复driver,如果streaming app没有stateful操作，也允许driver挂掉之后再次重启的进度丢失，就没有启动checkpoint的必要了。

## 4.如何checkpoint
streamingContext的创建要使用getOrCreate方法，主要需要将streaming的相关处理逻辑都放到该方法中。
1. 若application为首次重启，将创建一个新的StreamContext实例
2. 如果application是从失败中重启，将会从checkpoint目录导入checkpoint数据来重新创建StreamingContext实例。

示例如下:
```
val appName = "Recreating StreamingContext from Checkpoint"
val sc = new SparkContext("local[*]", appName, new SparkConf())

val checkpointDir = "_checkpoint"

def createSC(): StreamingContext = {
  val ssc = new StreamingContext(sc, batchDuration = Seconds(5))

  // NOTE: You have to create dstreams inside the method
  // See http://stackoverflow.com/q/35090180/1305344

  // Create constant input dstream with the RDD
  val rdd = sc.parallelize(0 to 9)
  import org.apache.spark.streaming.dstream.ConstantInputDStream
  val cis = new ConstantInputDStream(ssc, rdd)

  // Sample stream computation
  cis.print

  ssc.checkpoint(checkpointDir)
  ssc
}
val ssc = StreamingContext.getOrCreate(checkpointDir, createSC)

// Start streaming processing
ssc.start
```

## 5.checkpoint局限
### 5.1 代码更新后checkpoint数据不可用
checkpoint实现中将Scala/Java/Python objects序列化存储起来，恢复时会尝试反序列化这些objects。如果用修改过的class可能会导致错误。此时需要更换checkpoint目录或者删除checkpoint目录中的数据，程序才能起来。
### 5.2 spark1.6 中对dataframe的支持有限
在spark1.6中，对dataframe进行checkpoint可能会无法恢复。从spark2.1开始对dataframe checkpoint有好的支持见issuse：https://issues.apache.org/jira/browse/SPARK-11879

有一些trick的方法使用，可能会成功，详见：
https://stackoverflow.com/questions/33424445/how-to-checkpoint-dataframes/37014202#37014202对rdd进行checkpoint，再创建dataframe可以成功。
笔者在Stateful DStream中使用上述方法仍然失败。

## 6.总结
由于checkpoint天生的缺陷即代码变更后不能进行恢复，在生产环境中由于程序潜在的不稳定、程序的升级，checkpoint的缺陷都会造成一定风险。在这里不推荐使用checkpoint。

在不实用checkpoint时，比如数据来源是kafka，我们可以保存消费kafka的offset，当出现上述情况时，流重新拉起后，从上次的offset重新消费数据即可。

## 7.参考
[A Quick Guide On Apache Spark Streaming Checkpoint](https://techvidvan.com/tutorials/spark-streaming-checkpoint/)
[Checkpointing](https://jaceklaskowski.gitbooks.io/spark-streaming/spark-streaming-checkpointing.html)
[spark streaming checkpointing 踩坑记](https://www.jianshu.com/p/d7bee3d51863)
[Spark Streaming 容错机制](https://www.jianshu.com/p/cacb1e922c38)
