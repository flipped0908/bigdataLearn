# Spark

Application  -> 
one driver program   and   many job  ->
many Stage ->
one stage  - one taskset ->
task

分布式的并行计算框架  
MR 调度慢 计算慢 一项任务需要多轮mr


#### Driver Program
spark核心组件
构建SparkContext
job转化DAG图 数据处理流程图
DAG划分stage和tasks
根据tash想RM 申请资源

#### Executor
真正执行task的单元 一个WorkNode上可以有多个Executor


### 什么是spark 

executor都是装载在container里运行，container默认内存是1G    
executor分配的内存是executor-memory，想yarn申请的内存是（executor-memory + 1）*num -executors。  
AM在Spark中叫做driver AM向RM 申请的是executor 资源 当资源分配完后，executor启动后，有spark的am 向executor分配 task
分配多少task分配到哪个executor有AM决定，可以理解为spark也有一个调度过程，这些task都运行在executor的坑里  
executor有线程池多线程管理这些坑内的task    

Executor伴随整个app的生命周期    
线程池模型，省去进程频繁启停的开销  


#### spark解决了什么问题
最大化利用内存
中间结果放内存加速迭代  
某结果集放入内存，加速后续查询和处理，解决运行慢的问题  

提供了更丰富的api  

完整的作业描述  


#### spark 核心

基于弹性分布式数据集 RDD 模型 具有良好的通用，容错 ，并行处理  

RDD Resilient Distributed Dataset : 弹性分布式数据集，本质是数据集描述（只读的，可分区的分布式数据集），而不是数据本身


RDD 的关键特性  

将计算结果显式的保存在内存中，控制数据的划分。使用更丰富的操作来处理，只读（由一个RDD到另一个RDD，但是不能对本身的RDD修改）   
记录数据的变换而不是数据本身 容错  
懒操作   
瞬时性   


> 既然不能修改元数据 修改的rdd是怎么在内存中保存中间结果的，那么rdd是否是内存中的部分数据的数据集


Spark允许从四个方面构建RDD   
1 共享文件系统中 hdfs  
2 现有的rdd转换
3 定义一个scala数组  
4 有一个已经存在的RDD通过持久化操作生成  



Spark针对Rdd提供的两类操作  
transformations  
action  


每个RDD 包含了数据分块分区 partition 的集合  每个partition是不可分割的     


与父rdd的依赖关系  rddA => rddB    
宽依赖 ： b的每个partition依赖于a的所有partition       全部
    groupByKey、reduceByKey、join......，
窄依赖 ： b的每一个partition依赖于a的常数个partition    部分
    map、filter、union......

 

rdd的依赖 
> 如果让你自己设计，你怎么设计

``` 
从后边往前，将宽依赖的边 删掉，联通分量及其在原图中所有依赖的RDD ，构成一个stage

DAG是在计算过程中不断扩展的，在action后才会启动计算 

每一个stage内部，尽可能的包含一组具有窄依赖关系的转换 ，并将它们流水线并行化


```



若一个stage包含的其他stage中的任务已经全部完成，这个stage中的任务才会被加入调度   

> 遵循数据局部性原则，使得**数据传输代价最小**  

– 如果一个任务需要的数据在某个节点的内存中，这个任务就会被分配至那个节点    
– 需要的数据在某个节点的文件系统中，就分配至那个节点




容错：  
是一个递归过程，会一直追本溯源，甚至直到最初的输入数据     

如果task依赖的上层partition数据已经失效了，会先将其依赖的partition计算任务重新计算一遍   
 
宽依赖中被依赖partition，可以将数据保存HDFS，以便快速重构(checkpoint)  
– 窄依赖只依赖上层一个partition，恢复代价较少;宽依赖依赖上层所有partition，如果数据丢 失，上层所有partiton要重算  

可以指定保存一个RDD的数据至节点的cache中，如果内存不够，会LRU释放一部分，仍有重构的可能   



### spark 作业运行原理  

driver -》RM 申请资源 分配资源 -》 executor 执行task 

executor 内存分三块 
1 让task执行代码
2 task通过shuffle拉去stage的task
3 让rdd持久化使用  

Task的执行速度和每个executor进程的CPU Core数量有直接关系，一个CPU Core同一
时间只能执行一个线程，每个executor进程上分配到的多个task，都是以task一条线程的
方式，多线程并发运行的。如果CPU Core数量比较充足，而且分配到的task数量比较合
理，那么可以比较快速和高效地执行完这些task线程






### spark 调优

rdd的调优

资源的利用

参数的设置

### spark 技术栈

hadoop 的 hdfs  

spark on yarn 依赖于yarn的计算框架  

sparkcore  rdd DGA  

sparksql  hive

sparkstreaming  

mlib 机器学习

graphx 图形并行操作库





































