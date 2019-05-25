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

特点： 
1 消息持久化：通过O(1)的磁盘数据结构提供  
2 高吞吐：partition  
3 分布式   
4 多客户端  
5 实时性  

组件：  
broker ： 每个机器就是一个broker  
product ： flume    
consumer  ： spark streaming  
topic  ： 不同额消费者去指定的topic 中读 就像是一个旗帜  不同的人 可以根据旗帜 找到自己要去的地方    
partition ： 在topic 的基础上进一步的区分   


