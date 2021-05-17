# 汇集各种开发问题
### 1 开发egg.js 封装service层级的时候，引用层级搞错了对象。
**ctx.model.query 是关于database层的连接，而不是关于某个表的连接。**
<br/>
### 2 更改mysql绝host访问的问题
**授予user权限即可**
### 3 group by 查询 不了 数据库
**SQL 标准中不允许 SELECT 列表，HAVING 条件语句，或 ORDER BY 语句中出现 GROUP BY 中未列表的可聚合列。而 MySQL 中有一个状态 ONLY_FULL_GROUP_BY 来标识是否遵从这一标准，默认为开启状态。**
**SET SESSION sql_mode=(SELECT REPLACE(@@sql_mode,'ONLY_FULL_GROUP_BY,','')); 关闭状态**
