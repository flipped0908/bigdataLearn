
# 业务需求  


# 架构设计 

ODW层  源数据 历史增量 存量  平台层处理   

DW层
ETL  清洗 干净 脱敏 重新编码    
明细层：细粒度                  窄表
汇总层：明细层的数据 join 一下    宽表

应用层
  数据来源于DW层

# 规范设计  
降低沟通成本 

## 命名规范  
dws_sls_item_m
dws - 业务部门 - 维度自定义标签 - 时间标签

存量 T+1
增量 实时


## 开发规范  

降低风险

代码管理 

代码规范

最佳实践  比如秒还是毫秒  （机器还是人点击间隔）


## 流程规范    

需求分析 可行性评估 需求细化 工作量评估 需求排期 进入设计开发 （不停的问，大部分产品都是不知道，所以要一直问）  

开始 需求 评审 设计开发 开发环境测试 数据初步验证 验证通过  发布  配置质量监控 生产测试 数据验收 验收通过 


## 安全规范  

## 质量规范  


## 案例


商品维度表 dim_fr_item  
             
|   |     |
| ------  | -----    |
| 基本属性 <br>外键关联  |  item_id  <br> item_title <br> shop_id   |
| 时间 | ins <br> upd | 
| 对于层级关系 星形  扁平化冗余 | level1 <br>  l2_id <br> l3_id |
| 行为维度冗余 | item_sales_count_7days <br> item_sales_count_30days |
| 不适合桥接的字段 | shop_content1 <br> shop_contetn2 |
| 大字段，用于扩展的 | item_json_arrs <br> item_keyvalue_attrs |
| 分区字段分库分表  | day |


销售事实表






# 数据湖 
1 数据湖存放所有数据  

2 Schema-On-Read
 
3 更容易适应变化  

4 更快洞察超能力  

