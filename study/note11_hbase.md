
非关系型

运行在hsfs上 容错地存储海量稀疏的数据 

## 数据模型

RowKey  ColumnFamily  Column  VersionNumber Value 


##  物理模型 

Hbase 多个 Hregion

Table Region  

表 HTable
按RowKey 范围分Region Hregion RegionServer
Hstore  memstore HHfiles  
Hfiles - HDFS

## 系统架构
Client  
Master  
Region Server  
Zookeeper  


## 容错  


## 特殊的表

root meta  

## 写入流程   

寻址 

hbase:meta表存储了所有用户HRegion的位置信息

当客户端发起一个Put请求时，首先它从hbase:meta表中查出该Put数据最终需要去的 HRegionServer。然后客户端将Put请求发送给相应的HRegionServer，在HRegionServer中 它首先会将该Put操作写入WAL日志(Flush到磁盘中)。

Memstore是一个写缓存，每一个Column Family有一个自己的MemStore

写完WAL日志文件后，HRegionServer根据Put中的TableName和RowKey找到对应的 HRegion，并根据Column Family找到对应的HStore，并将Put写入到该HStore的MemStore 中。此时写成功，并返回通知客户端。

MemStore是一个In Memory Sorted Buffer，在每个HStore中都有一个MemStore，即它是 一个HRegion的一个Column Family对应一个实例。它的排列顺序以RowKey、Column Family、Column的顺序以及Timestamp的倒序。

## 读取流程  
HBase中扫瞄的顺序依次是:BlockCache、MemStore、StoreFile(HFile)  

