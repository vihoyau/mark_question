# 汇集各种开发问题
### 1 开发egg.js 封装service层级的时候，引用层级搞错了对象。
**ctx.model.query 是关于database层的连接，而不是关于某个表的连接。**
<br/>
### 2 更改mysql绝host访问的问题
**授予user权限即可**
