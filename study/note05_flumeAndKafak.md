## kafka
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


