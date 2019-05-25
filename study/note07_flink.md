##

storm  延迟低 吞吐高偏低  
spark streaming  吞吐大 延迟高

flink 低延迟 高吞吐  


### flink 技术栈

flink core  
分布式流处理引擎  
为上层api提供基础服务  

flink API  
实现面向stream的流处理和面向batch的批处理api  

DataSet  
对静态数据进行批处理 将静态数据抽象成分布式数据集  

DataStream:   
对数据流进行流处理操作，将流式的数据抽象成分布式数据流    

Table API   
对结构化数据进行查询操作，将结构化数据抽象成关系表   



数据集:    
- 无界数据集:持续不断，不停流入数据(交易日志、网站点击日志)     扶梯  
- 有界数据集:批次的，类似MapReduce处理的数据集               电梯
 

数据处理模型:  
- 流处理:实时任务，任务一直运行，处理无界数据  
- 批处理:批处理任务，处理有界数据，任务运行完释放资源  


Flink:将有界数据集当做无界数据集的一种特例  
Spark Streaming:把无界数据集分割成有界，通过微批的方式对待流计算   


#### flink 数据处理组件
 
 Stream  -  Source  - Transformation  - Sink   
 
 StreamGraph   
 
 ##### 执行任务 
 runtime层一JobGraph形式接收程序
 DateStream DateSet  API 都会单独编译 JobGraph
 
 
 ##### streaming dataflow
 Flink 映射到数据流 Streaming dataflow 由流和转化符组成  
 
 stream 类似 rdd   
 transformation 类似 spark算子  
 
 ##### 流的重分布
 一对一流 保持元素的分区和排序  
 redistributing流 重分布 每个算子任务根据所选的转换 向不同的目标子任务发送数据  一对多  
 
 
 ##### 时间操作
 事件时间   event time
 采集时间   ingestion time
 处理时间   processing timee
 
 
 ##### window操作
 时间窗口       
        翻滚窗口  
        滑动窗口  
 事件窗口  
 会话窗口  
 
 
#### Flink API  

##### 低层级的抽象
通过 process function 嵌入DataStream Api 中  
实现复杂的程序计算  容错 回调事件  

##### 核心API         
实际开发中 主要是对核心api的编程  

多形式的 Transformation  
连接 join  
聚合  aggregation  
窗口 window  
状态 state   


##### Table API
以表为中心的 
程序声明式的定义了什么逻辑应该执行 而不是操作代码 写了就执行  
类似 spark sql 和 rdd 的混合使用   


##### SQL 层
最高级层级的抽象是sql   


##### Source 组件
读取  
基于文件     readTextFile（path）；
基于socket   socketTextStream：  
基于集合  


##### transormation 组件
map   flatmap   fliter  keyby  reduce flod  aggregation  window  windowall 
windowapply windowreduce  
windowfold aggregationOnWindow union windowjoin connect  splite iterate extract  



##### sink组件  
writeAsText()  
writeAsCsv()
print()/printToErr()  
writeUsingOutputFormat()  



### Flink 架构  

#### 分布式Runtime环境
FlinkProgram  
Dataflow  
Client ActorSystem  


Job Managers:  (master)
Scheduler  
Checkpoint Coordainator  
Actor System


TaskManager: worker  
TaskSlot  
Network Manager  
ActorSystem  
Memory&I/O manager  

#### Task的Slots 和 资源
TaskManager 进程  一个JVM    
slot 线程  
slot 只分配内存 不分配cpu **（这和 sparkStream不同）**


#### 状态的最终存储  
快照  
（比如你想存一栋大楼，你可以存它的照片，但是你如果真的赋值一个大楼要多大空间 ，一张照片空间就很小）   


#### Flink on yarn  

（不太理解）？？？？ 



## Flink 容错  


sparkstreaming  WLA  checkpoint

flink 分布式数据流 和 快照   


barrier 组标记栏   
barier 被定期的插入 每一个barrier都带有快照 id  不会干扰数据流  

#### 对齐 
Operator 接受多个数据需要对数据流做排列对齐  
放入缓存中先到的  

##### checkpoint
保证流处理系统失败的时候 能够正确的回复数据  

##### 水位线  
保证处理时间 》= 事件时间  

##### 窗口机制  

按分割标准划分 time count

按窗口行为划分

基本操作
window 创建 
trigger  触发  
evictor 驱动   过滤  
apply 自定义 function 

##### 撤回 
retract sql中 撤回 之前的计算结果  重新计算  

#### 反压机制  限流  
storm zookeeper stop

sparkstreaming 自动反压  

flink 不需要设置  每个最贱都有对应的分布式阻塞队列   相当于有一个消息队列中间件   


















        
 


































































