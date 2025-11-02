---
layout: post
title: Spark Streaming 实时join
categories: [spark]
description: Spark Sttreaming join
keywords: spark streaming,join
---

# 1.介绍
很多时候流上报过来的数据只有id，这时需要我们join维度表，这里有三种思路：

1.在数据源端进行join，如在kafka写入时进行join，不过这需要数据源端配合。
2.在计算引擎上join，如spark streaming/flink。
3.在结果端，如结果写入es/mysql，可在此时根据key join。

这三种方式大家按实际情况进行取舍，各自有自己的适用场景。我这里主要介绍在计算引擎端进行join。
# 2.计算引擎端join
## 2.1 流与完全静态数据Join
流与完全静态数据Join有两种方式，一种是RDD join，另一种是broadcast join。
不过这种方式，我觉得应该是很少用的，很少有完全不变的数据。而且这种方式完
全可以用后面提到的方式进行替代。

业务中有个场景就是kafka中的订单表要查询司机信息，我们的做法是把对应城市的活跃司机信息保存为文件然后设置为broadcast广播变量与订单表实时流join，如果关联不上就通过接口查询司机信息，其中查询司机信息时会设置一个缓存，防止频繁请求。
```$xslt
LoadingCache<String, DriverInfo> localCache  = CacheBuilder.newBuilder().maximumSize(30000).expireAfterWrite(2, TimeUnit.HOURS)
            .build(new CacheLoader<String, DriverInfo>(){
                public DriverInfo load(String driver_id){
                    DriverInfo driverInfo = OrderHelper.getDriverInfoFromservice(driver_id);
                    return driverInfo;
                }
            });
```
RDD join
```$xslt
val dataset: RDD[String, String] = ...
val kafkaDStream: DStream[String, String] = ...
val joinedStream = kafkaDStream.transform { rdd => rdd.join(dataset) }
//val joinedStream = kafkaDStream.foreachRDD(_.join(dataset)

```
broadcast join
```
val dataset: RDD[String, String] = ...
val broadcastData=ssc.sparkContext.broadcast(dataset.collect())
val kafkaDStream: DStream[String, String] = ...
val joinedStream = kafkaDStream.mapPartitions(part=>{
      val dataset = broadcastData.value
      part.map(row=>{
        ...
    })
```
## 2.2 流与半静态数据Join
半静态数据指的是放在redis等的数据，会被外部更新。思路是，根据redis分区策略，
key进行repartition，然后每个partition去对应redis机器上取得数据，然后组合。
这里的话我想的是如果redis的分区策略和kafka的分区策略一致，就不用repar了。
这里外部的缓存还可以根据key的热度进行缓存，不用缓存全部数据，数据量大的时候
也不用担心。
```
val kafkaDStream: DStream[String, String] = ...
val result=kafkaDStream.mapPartitions(part=>{
      val redisCli=connToRedis("localhost",6111,1000,10)
      part.map(row=>{
        ...
      })
    })
```
## 2.3 流与流join
流与流join，就是两个实时流进行join。我们之前说过流与静态数据join有更好的替代方案，这里我们将维度数据以流的方式查出来，并且batch时间设置比较长的时间，
那么这个流就可以作为维度流与事实流进行join，并且可以按照我们设置的batch时间来更新数据。如果我们维度表比较小可以考虑这种方式，维度表比较大的话更建议
第二种方式。

前段时间写Spark Streaming实时程序时遇到个这样一种场景，kafka 订单表topic中某些某些字段不全，需要与订单扩展表实时join获取订单扩展信息。如果批次比较小时，有可能会出现关联不上的情况。一般采取的方式是如果关联不上就降级从接口中查询订单扩展表信息。
```
val kafkaDStream1: DStream[String, String] = ...
val kafkaDStream2: DStream[String, String] = ...
val joinedStream = kafkaDStream1.join(kafkaDStream2)

```
# 3.参考
[spark streaming join 维度表解决方案](https://riverzzz.github.io/2019/05/13/spark-streaming-join-%E7%BB%B4%E5%BA%A6%E8%A1%A8%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88/)

