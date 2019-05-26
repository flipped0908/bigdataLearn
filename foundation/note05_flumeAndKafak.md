# flume

分布式，搞笑收集 汇聚 和 移动  大规模日志信息  

提供fliter 过滤功能  

数据源 cpnsole rpc text tail syslog exec  


## flume 事件
 Event ： 专差数据的字节数数组 + 可选头部  
 

## flume agent 
每个agent 是一个独立的守护进程 jvm container  

agent  : source channel sink    

channel : 被动存储形式   
Memory Channel ： 内存存储食物 吞吐高 存在丢数据风险  
File Channel ： WAL 实现 不会丢数据 


agent interceptor   用于source的一组拦截器  
对日志过滤和包装  

agent selector 选择放入到不同的channel  

## flume 可靠性  

## flume 支持复杂的流动  








# kafka
linkedin 2010 年 开源的

##特点： 
1 消息持久化：通过O(1)的磁盘数据结构提供  
2 高吞吐：partition  
3 分布式   
4 多客户端  
5 实时性  

##组件：  
broker ： 每个机器就是一个broker  
product ： flume    
consumer  ： spark streaming  
topic  ： 不同额消费者去指定的topic 中读 就像是一个旗帜  不同的人 可以根据旗帜 找到自己要去的地方    
partition ： 在topic 的基础上进一步的区分   




## 持久化
一段时间后 执行性真正的文件 flush  
逻辑偏移量  

## 传输效率
分批传输 减少网络请求的数量 
无缓存技术 避免双重缓存 消息只缓存了一份在页缓存中  
zore-copy 

## 无状态borker
消费者维护自己消费的状态信息offset  

## 交付保证
默认 at least once  


## 副本管理  
保证数据不丢失   

## 分布式协调  
不同分区只能被消费组中的一个消费者消费  

kafka 使用 zookeeper   
探测broker和consumer的添加或移除  
当1发生的时候出发每一个消费之进程重新负载  
维护消费关系和追踪消费者在分区消费消息的offset   


## 和其他消息服务对比 
生产者测试  activemq ratbbitmq 
消费者测试  

kafka 性能好很多  
原因 1 kafka不等待 2 搞笑存储格式 kafka 每个信息 9字节 amq 144字节 amq 大部分时间都在存取B-Tree维护消息元数据和状态   

kafka 存储格式 字节少  amq rmq 维护消息的传输状态 






















