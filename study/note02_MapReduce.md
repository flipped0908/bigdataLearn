
#### File
文件要存储在HDFS中，每个文件切分 成多个一定大小(默认64M)的Block (默认3个备份)存储在多个节点 (DataNode)上  

#### InputFormate
MR的基础类之一  数据分割 （Data Splits） 记录读取器（Record Reader）

#### splite
实际上每个split包含一个Block中开头部分的数据， 解决记录快Block的问题

#### RR  RecordRead
每次读取一条记录 调用一次map函数

####  map
滴啊用一次map 

#### shuffle
Partition， Sort， Spill ， Merger ，Combiner ， Copy， Memery ，Disk 。。。  
神奇的地方 性能优化的重点区域

1 partition:  
    决定数据由那个Reducer 处理   
2 MemoryBuffer:  
   内存缓冲区，每个map的结果和partition处理的 key value结果都保存在缓存中     
3 Spill:
   内存缓冲区达到阈值时，溢写spill线程锁住这80M 的缓冲区，开始将数据写出到本地磁盘中，然后释 放内存。  
4 Sort:  
    缓冲区数据按照key进行排序
5 Combiner：
    合并数据
6 spill sort：
     和map一样，内存缓冲满时，也通过sort和 combiner，将数据溢写到硬盘文件中  
7 Reduce：
    多个reduce任务输入的数据都属于不同的 partition，因此结果数据的key不会重复。  
    合并reduce的输出文件即可得到最终的结 果。                 


## Hadoop Streaming
   MapReduce和HDFS采用Java实现，默认提供Java编程接口
   • Streaming框架允许任何语言来实现能在Hadoop MapReduce使用的程
   序
   • Streaming方便已有程序向Hadoop平台移植
   
   
### 应用

1 数据统计   

2 数据过滤     

3 过滤加统计  

4 数据join  

5 全局排序 容错框架 


   