# 汇集各种开发问题
### 1 开发egg.js 封装service层级的时候，引用层级搞错了对象。
**ctx.model.query 是关于database层的连接，而不是关于某个表的连接。**
<br/>
### 2 更改mysql绝host访问的问题
**授予user权限即可**
### 3 group by 查询 不了 数据库
**SQL 标准中不允许 SELECT 列表，HAVING 条件语句，或 ORDER BY 语句中出现 GROUP BY 中未列表的可聚合列。**
**而 MySQL 中有一个状态 ONLY_FULL_GROUP_BY 来标识是否遵从这一标准，默认为开启状态。**
**SET SESSION sql_mode=(SELECT REPLACE(@@sql_mode,'ONLY_FULL_GROUP_BY,','')); 关闭状态**

### 4 docker 启动 自定义镜像 ，闪退
**闪退的根本原因：进程中断或者进程生命周期结束。**
**解决方法：让进程不被关闭，一直开着。docker容器内部的进程需要启动，外部才能调度docker 容器这个进程。如果docker内部进程关闭，外部的容器也随着关闭**

### 5 mongodb 数据丢失
** 未知道原因 ，需要做持久化**

### 6 mongodb 连接 eggjs 配置config 问题
** 不需要 username 和 password **


