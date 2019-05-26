## 系统架构  

Namespace   Namenode  BlockManagement

Block Stroage Datanode  PhysicalStroage  


Master:
NameNode SecondaryNameNode 
Workwer:
DataNode  

## NameNode

管理文件系统的命名空间    
    维护文件系统树及树种的所有文件和目录  
存储元数据  
    保存各种元信息       
        目录和层级关系  
        权限  
        文件块的名字 和 文件有哪些块组成  
元数据保存在内存中 1.文件名到block 2 block到datanode  
    NameNOde元信息并不包含每个块的位置信息  
保存文件 block datanode 之间的映射关系  


#### Hadoop更倾向存储大文件原因 

        一般来说，一条元信息记录会占用200byte内存空间。假设块大
        小为64MB ，备份数量是3 ，那么一个1GB大小的文件将占用
        16*3=48个文件块。如果现在有1000个1MB大小的文件，则会占
        用1000*3=3000个文件块(1000个文件不能放到一个块中)。我 们可以发现，
        如果文件越小，存储同等大小文件所需要的元信息
        就越多，所以Hadoop更喜欢大文件。     
        
        
元信息持久化   
在NameNode中存放元信息的文件是 fsimage。在系统运行期间 所有对元信息的操作都保存在内存中并被持久化到另一个文件
    edits中。并且edits文件和fsimage文件会被SecondaryNameNode周期性的合并

运行NameNode会占用大量内存和I/O资源，一般NameNode不会存储用户数据或执行MapReduce任务 


#### 全Hadoop系统只有一个NameNode   

导致单点问题   

将hadoop元数据写入到本地文件系统的同时再实时同步到一个远程挂载的网络文件系统(NFS)。   

运行一个secondary NameNode，它的作用是与NameNode进行交互， 定期通过编辑日志文件edits logs合并命名空间镜像fsimage


## DataNode   

负责存储数据块，负责为系统客户端提供数据块的读写服务  

根据NameNode的指示进行创建、删除和复制等操作  

心跳机制，定期报告文件块列表信息

DataNode之间进行通信，块的副本处理  


## 数据块——磁盘读写的基本单位

HDFS默认数据块大小64MB

磁盘块一般为512B

原因:块增大可以减少寻址时间，降低寻址时间/文件传输时间，若寻址
时间为10ms，磁盘传输速率为100MB/s，那么该比例仅为1%

数据块过大也不好 因为一个MapReduce通常以一个块作为输入 ，块大会导致整体任务数量过小，降低作业处理速度


## SecondNameNode

命名不好，容易误解，SecondNameNode并不是NameNode的备份

用来保存HDFS的元数据信息，比如命名空间信息、块信息等，由于这些信息是在内存的，
SecondNameNode是为了考虑持久化到磁盘

Secondary NameNode所做的不过是在文件系统 中设置一个检查点来帮助NameNode更好的工作。 它不是要取代掉NameNode也不是NameNode的


## Block副本放置策略  

第一个副本，在客户端相同的节点(如果客户端是集群外的一台机器，就随机算节 点，但是系统会避免挑选太满或者太忙的节点)

第二个副本，放在不同机架(随机选择)的节点

第三个副本，放在与第二个副本同机架但是不同节点上



## 数据完整性校验  

HDFS 会对写入的数据计算校验和，并在读取数据时验证校验和 


校验和  
• 检测损坏数据的常用方法是在第一次进行系统时计算数据的校验和，在通道传输过程中，如果新生成的校验和 不完全匹配原始的校验和，那么数据就会被认为是被损坏的。

数据块检测程序DataBlockScanner  
在DataNode节点上开启一个后台线程，来定期验证存储在它上所有块，这个是防止物理介质出现损减情况而造成的数据损坏  

## 容错 

一个名字节点和多个数据节点

数据复制(冗余机制)

故障检测  
数据节点  心跳包(检测是否宕机)  块报告(安全模式下检测)  数据完整性检测(校验和比较)    
名字节点(日志文件，镜像文件)   

空间回收机制   Trash目录


## 应用  

能做什么  
– 存储并管理PB级数据  
– 处理非结构化数据  
– 注重数据处理的吞吐量(延迟不敏感)T+1  
– 应用模式:write-once-read-many存取模式(无数据一致性问题)  

不适合做  
– 存储小文件(不建议)  
– 大量随机读(不建议)  
– 需要对文件修改(不支持)  
– 多用户写入(不支持)   



# HDFS 2.0 

## 新特性  

  NameNode HA  
• NameNode Federation  
• HDFS 快照  
• HDFS 缓存  
• HDFS ACL  
• 异构层级存储结构   
 

##   NameNode HA  

什么问题:Hadoop 1.0中NameNode在整个HDFS中只有一个，存在单点故障 风险，一旦NameNode挂掉，整个集群无法使用 

解决方法:HDFS的高可用性将通过在同一个集群中运行两个NameNode ( active NameNode & standby NameNode )来解决 

在任何时间，只有一台机器处于Active状态;另一台机器是处于Standby状态

Active NN负责集群中所有客户端的操作;

Standby NN主要用于备用，它主要维持足够的状态，如果必要，可以提供快速 的故障恢复。
 
 
####  同步问题:

需要依赖JournalNodes守护进程，完成元数据的一致性

####

快速的故障恢复:心跳保证，Standby NN也需要保存集群中各个文件块的存储 位置

避免分歧:任何情况下，NameNode只有一个Active状态，否则导致数据的丢 失及其它不正确的结果

####节点分配:  

NameNodemachines:运行ActiveNN和StandbyNN的机器需要相同的硬件配置  

JournalNodemachines:也就是运行JN的机器。JN守护进程相对来说比较轻量，所以这些 守护进程可以可其他守护线程(比如NN，YARN ResourceManager)运行在同一台机器上  
• 在一个集群中，最少要运行3个JN守护进程，这将使得系统有一定的容错能力

在HA集群中，Standby NN也执行namespace状态的checkpoints，所以不必 要运行Secondary NN、CheckpointNode和BackupNode;事实上，运行这 些守护进程是错误的。 


####

在HA集群中，Standby NN也执行namespace状态的checkpoints，所以不必 要运行Secondary NN、CheckpointNode和BackupNode;事实上，运行这 些守护进程是错误的。


好处:实现NameNode的横向扩展,使得Hadoop集群的规模可以达到上万台




# • HDFS 快照  

  HDFS快照是一个只读的基于时间点文件系统拷贝  
• 快照可以是整个文件系统的也可以是一部分。  
• 常用来作为数据备份，防止用户错误操作和容灾恢复。  
• Snapshot 并不会影响HDFS 的正常操作:修改会按照时间的反序记录，这样可 以直接读取到最新的数据。  
• 快照数据是当前数据减去修改的部分计算出来的。  
• 快照会存储在snapshottable的目录下。  
 


# HDFS 缓存 

• 允许用户指定要缓存的HDFS路径  
• 明确的锁定可以阻止频繁使用的数据被从内存中清除  
• 集中化缓存管理对于重复访问的文件很有用   
• 可以换成目录或文件，但目录是非递归的  


# HDFS ACL 

Hadoop从2.4.0开始支持

• 目前HDFS的权限控制与Linux一致，包括用户、用户组、其他用户组三类权限，这种方
式有很大局限性


# 异构层级存储结构 

Hadoop2.6.0开始支持 

一个集群中，有多种不同的存储介质，比如有SSD、RAM等  

由于一个Hadoop集群中有多种类型的任务，不同的任务对数据访问速度要求不 一样，于是通过相应配置，控制目录和文件写到什么样的介质上去，并且具备配
 额控制要求
 
• 目前支持不够完整













   