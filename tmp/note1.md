

checklist
1 kafak 的原理 架构图 flume  和 rabbitmq redis rocketmq 的区别
  本质上都是个队列， 处理的是元数据还是数据本身
  
2 spark yarn client 和 cluest 两种模式的区别？

3 spart strming 架构中的各个节点怎么对应到spar yarn 中。



#### question：
spark 和 hadoop 的区别
#### answer：
区别之一： spark 一个进程中可以有多个task ，spark运行速度快， task可以快速启动，并处理内存中的数据。  
         hadoop 一个task就是一个进程， 是把数据拿到进程中处理， 然后在方放回去， 不在内存中处理数据的中间结果。（自己的理解，还需要验证）  
         hadoop 一个是没有在内存中处理数据的中间结果， 一是多个进程， 没有好好利用cpu，浪费了cpu的资源，进程很消耗资源， 还有没有好好利用内存， 而 spark 做到了  
         
但是saprk 又有缺点 每一个Applocation中拥有固定数量的executor和固定的内存， 也会造成资源浪费  
这时候又有了 yarn cluster 帮助 spark 灵活的运用executor 和 内存 解决了资源浪费的问题  

这时候又有了新的需求，spark 呢处理离线的大量数据特别快，处理实时的呢，  
这时候有了 storm 。sparkstreming ，flink，blink   
storm也盛极一时 但是很快就又被取代了  

这些都是一个问题的解决带来了新的问题，然后又产生了新的解决方案， 随着硬件技术的继续发展，有产生了新的解决方案  
所有都是交替的  

然而这些解决方案的逻辑，可以跟现实生活中人们处理事情的逻辑一致    
新的计算框架，新的架构， 以及错误补偿机制 ， 架构图， 都是人们逻辑的体现    

所以最重要的是逻辑   




