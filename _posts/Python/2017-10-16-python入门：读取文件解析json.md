---
layout: post
title: python入门：读取文件解析json
categories: Python
description: python入门：读取文件解析json
---
## 使用场景
在linux上将spark实时生成的json的经纬度字段解析出来，然后封装成高德热力图api可用的json样式。思考了一圈，用shell对json解析能力比较弱，用java太重，还是用python方便。
## 原始数据格式

```
{"transPriceType":"xxx","useLat":"39.915394","passengerType":"GP","useLocale":"xxx","isVoice":"xxx","orderTime":"2017-09-22 13:59:40","adminCode":"xxx","bespeakType":"xxx","passengerDemand":"","destination":"xxx","orderNo":"10001","passengerName":"xxx","operateFlag":"xxx","voiceUrl":"","destLat":"39.906896","useLon":"116.415065","destLon":"116.429038","useTime":"2017-09-22 14:40:46","passengerTel":"136xxx"}
{"transPriceType":"xxx","useLat":"39.915394","passengerType":"GP","useLocale":"xxx","isVoice":"xxx","orderTime":"2017-09-22 13:59:40","adminCode":"xxx","bespeakType":"xxx","passengerDemand":"","destination":"xxx","orderNo":"10001","passengerName":"xxx","operateFlag":"xxx","voiceUrl":"","destLat":"39.906896","useLon":"116.415065","destLon":"116.429038","useTime":"2017-09-22 14:40:46","passengerTel":"136xxx"}
{"transPriceType":"xxx","useLat":"39.915394","passengerType":"GP","useLocale":"xxx","isVoice":"xxx","orderTime":"2017-09-22 13:59:40","adminCode":"xxx","bespeakType":"xxx","passengerDemand":"","destination":"xxx","orderNo":"10001","passengerName":"xxx","operateFlag":"xxx","voiceUrl":"","destLat":"39.906896","useLon":"116.415065","destLon":"116.429038","useTime":"2017-09-22 14:40:46","passengerTel":"136xxx"}
```

## python解析代码

```
#! /usr/bin/env python  
#coding:utf-8  
import json
file = open("201710091400_15.json")
hitmap={}
while 1:
    line = file.readline()
    if not line:
        break
    json_dict = json.loads(line)
    key=json_dict["useLon"]+"_"+json_dict["useLat"]
    if key in hitmap.keys():
	value=hitmap[key]+1
        hitmap[key]=value
    else:
       hitmap[key]=1
filename ='heatmapData.js'
with open(filename, 'a') as file_object:
    file_object.write("var heatmapData = [{\n")
    dict_len=len(hitmap)
    index=1
    for key,value in hitmap.items():
    	file_object.write("\t\"lng\":"+ key.split("_")[0]+",\n")
    	file_object.write("\t\"lat\":"+key.split("_")[1]+",\n")
    	file_object.write("\t\"count\":"+str(value)+"\n")
	if index != dict_len:
         	file_object.write("}, {\n")
        else:
                file_object.write("}];\n")
        index+=1
```

## 生成后数据格式
生成后的数据如下，可以直接放在高德热力图的中进行显示

```
[{
	"lng":116.416812,
	"lat":39.909065,
	"count":1
}, {
	"lng":116.335648,
	"lat":39.993144,
	"count":1
}, {
	"lng":116.475383,
	"lat":39.884503,
	"count":2
}, {
	"lng":116.389868,
	"lat":39.887871,
	"count":1
}, {
	"lng":116.457450,
	"lat":39.917896,
	"count":1
}]
```

