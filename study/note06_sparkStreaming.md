
#

spark sreaming 是 spqrk 核心api的一个扩展  实现高吞吐 具备容错机制的实时流数据的处理  


## 
第一步： 针对小数据快的RDD DAG的构建  
第二步： 连续Data的切片处理  


？？？？？ 疑问 ： 对每一批的处理结果处理完成之后 能 在存起来吗  怎么划分片 如果 需要前后两个或之前的第5个两个及国际继续处理
 算是实时分析吗  还是离线分析   
 
 
## DStream
 离散流  
 为什么要将连续的数据持久化，离散化 然后进行批量处理？  
 1 数据持久化 ：接受到的数据暂存  
 2 离散化： 按时间分片，形成处理单元  
 3 分片处理： 分批处理  
 
transformation ： 转换算子   
    map join flatmap reduceByKey
output 执行算子 输出算子   
    saveAsObject saveAsFile 
    
    
## Dstream Graph
 
 c = a.join(b) ,d  = c.flter()

 逻辑关系是 a/b -> c ,c->d  
 
 在sparkstreaming 中进行物理记录确实反向的  a/b <- c ,c <- d  
 
 
 ```
                    每个时间间隔会积累一定的瞬狙，这些数据可以看成由于event（假设kafka或者flume）
                    时间间隔是固定的，在时间间隔内的数据就是固定的，也就是有RDD是有一个时间间隔内所有的
                    数据构成。时间维度的不同，导致酶促处理的数据量和内容概不同
  Y轴 
  
  空间维度：代表
  RDD的依赖关系
  构成的具体的业务
  逻辑的处理步骤                                                            时间维度：batchInterval
  用DstreamGraoh                                                    X轴    为时间间隔不断生成job实例
  表示                                                                     并且在集群上运行
  
                  随着时间的流逝，基于DstreamGraph不断的申城Rdd Graph 也就是
                  DAG的方式生产job 并通过jobScheduler的线程池提交给Spark
                  Cluster不断的执行
 

 
```


# spark Stream架构

## 模块
1 Master  记录Dstream 之间的依赖关系学院关系 调度
2 Worker  从网络接数据 执行机选
3 client  向 spark stream 中灌输数据   


## 作业提交  

Network Input Tracker   
Job Schedluer  
Job Manager  


## streaming 窗口操作  
滑动长度 
滑动时间间隔

## streaming 容错分析  
启动WAL持久化日志配置  
1 给streamingContext 设置checkpoint目录 
2 spark。streaming。receive。writeAlheadLog.enable 设置为 true  


 

# SPARK STREAMING 接受数据的两种方式   

## receiver-based Approach : offset存储在zookeeper 有Receiver 维护，Spark获取书记处存入executor中 调用kafka高阶api 

## Direct Approach（No Receiver） offset 自己存储和维护 由spark维护，且可以从每一个分区去取数据，调用kafka 的低阶api







































 