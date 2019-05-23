
###

1.0
jobTracker    master    
taskTracker   selver   

2.0
Resource Manager      master   
ApplicationManager    selver   

```
1 client 到 RM  
2 RM 分配一个selver 启动一个 AM
3 AM 在去 RM 要资源
4 RM 分配多个selver 
5 AM 控制多个分配到的selver 

```


容错能力 
1. RM挂掉:单点故障，新版本可以基于Zookeeper实现HA高可用集群，可通过 配置进行设置准备RM，主提供服务，备同步主的信息，一旦主挂掉，备立即做 切换接替进行服务
2. NM挂掉:不止一个，当一个挂了，会通过心跳方式通知RM，RM将情况通知对 应AM，AM作进一步处理
3. AM挂掉:若挂掉，RM负责重启，其实RM上有一个RMApplicationManager ，是AMaster的AManager，上面保存已经完成的task，若重启AM，无需重新
运行已经完成的task



