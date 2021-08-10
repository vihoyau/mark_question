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

### 7 删除 mongodb重复的数据
```
db.getCollection("iot_log_money").aggregate([
    {
        $group:{_id:{request_id:'$request_id'},count:{$sum:1},dups:{$addToSet:'$_id'}}
    },
    {
        $match:{count:{$gt:1}}
    }

    ], { allowDiskUse: true }).forEach(function(it){

         it.dups.shift();
            db.getCollection("iot_log_money").remove({_id: {$in: it.dups}});

    });
```

### 8 mongodb查询慢.建立联合索引
```
db.getCollection("iot_log").ensureIndex({productKey:-1,deviceName:-1,status:-1})
db.getCollection("iot_log").ensureIndex({productKey:-1,status:-1})
```
### 9.docker hub 提交镜像 

```bash
$ docker login
$ docker build -t wyho/device_server_manager:1.0.1 .
$ docker commit -a "xxx" -m "xxx" 651a8541a47d myubuntu:v1
$ docker push wyho/device_server_manager:1.0.3
# $ docker run -d --name device_server_manager wyho/device_server_manager:1.0.3 -p 9003:7001
$ docker run -d -p 9002:7001 215e8e5cc3bf
$ docker update 9331e4c37ed5 --restart=always
$ docker run -d --name node/koa-server -p 9003:7001
```
### 10，mongodb构建表
```
1,npx sequelize migration:generate  --name=init-users  
2,在database/migrations 目录下 构建users表models
3,npx sequelize db:migrate 执行
```
### 11，mysql 备份
```
1,备份网络问题，公司路由器拦截，每天自动备份可能下线 
2,备份的脚本语句，需要加--single-transaction --default-character-set=utf8mb4 避免编码问题
3,恢复方式，从手动excute改成命令方式。
4,使用--single-transaction保证dump文件的有效性
5,恢复脚本shell，需要拷贝本地sql进入docker，然后drop table，再执行sql。使用source xx.sql 
```
