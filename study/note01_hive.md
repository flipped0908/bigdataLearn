
## HIVE

Hive 基于一个统一的查询分析层， 通过sql语句的方式对hsfs得上的数据进行 查询 统计分析  

Hive sql解析引擎， 将sql 转译成 MR的Job 在hdoop上快读运行， 蛋刀快速开发的目的

HIVe 是纯逻辑表 元数据 本质是Haddoop的目录文件  实现了元数据和数据存储的分离

HIVe 本身不存储数据    

Hive 读多写少 不支持数据的删除和次改

```` 

select word ,count(*) from ( select explode(splite(sentence, ' ')) as word from  article ) t group by word

```` 


##### Hive   和 传统关系数据的 比较

1 存储的文件系统不一样  

2 hive使用的MR  传统的使用的是普通算法  查抄排序  

3 关系型 的 实时场景  hive 数据挖掘场景   

4 hive存储和扩展能力强  


#### hive 的架构

1 用户接口  CLI 启动一个hive副本 ，JDBC 客户端-hiveserver ， WUI 浏览器访问hive 

2 语句转换  解析， 语法分析吗， 优化， 生成 MR  

3 数据存贮  生成查询计划 有MR调用执行  


#### hive 的内部表 和外部表


#### hive 中的 partition 
table 拆分成 partition


#### hive 中的 bucket 
partition 拆分成 bucket


####  hive 中的数据类型 
1 原生类型  int float double timestamp 。。。  

2 复合类型  arrays


#### hive 中和sql  和 map reduce ， join 的优化  





























