1 监控平台

关系到公司的核心利益  

产生的数据， 生成的报表直接决定了上层管理层的决策


项目技术家架构 ： 

数据采集 ： 队列左缓存，  发送到 消息队列中，

每一个数据来源使用两个主机进行接收，保证 ，高可用，通过设置两台主机的优先级，指定接受策略。

分布式接收数据。

对于不同的数据来源，通过抽象，使用工厂模式，实现对厂商的增减， 保证了系统的可扩展性。

设计为无状态服务，可以更好的实现横向扩展


采用消息队列 ，实现异步存储 保证可高性能

采用分布式，消息队列，rabbitmq ， 和分布式存储 mongdo 保证了高可并发

消息队列， 根据不同的topic ，将存储业务和实时业务进行结构， 原始数据直接存入数据仓库，另一部分用来计算


对于数据的处理

数据仓库 
发电原始数据仓库  

业务数据仓库 


分为原始数据层 ODS层 
和DW层 经过清洗，转换，加载， 建模理论，生成的中间层

结合业务数据仓库进行 ETL 

产生的 明细层 和 汇总层 

供数据处理人员 ，生成报表和统计


我们的业务数据量 

怎么计算

每条数据有几个字段 每个字段有几个字节 
每天每个设备有统计多少次 例如5分钟一次
一共有多少用户 


30 * 30  * （24*12） *  10000 

100 * 300 * 1000

3 1000 1000 10 B

1GB 10^9 B




