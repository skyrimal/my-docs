nacos在今天单机版无法启动，仔细查看后发现日志报错有一个关于sql的报错。

但是今天之前没有配置数据库连接依旧能成功启动，官网明确说明单机版不需要连接数据库，重新安装过nacos后依旧无法重新启动，怀疑是笔记本系统的原因，原因不明

> java.sql.SQLTimeoutException: Login timeout exceeded.

最后在application.properties里添加数据库连接池配置后成功启动

```properties
### Count of DB:
db.num=1

### Connect URL of DB:
db.url.0=jdbc:mysql://127.0.0.1:3306/nacos?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true&useUnicode=true&useSSL=false&serverTimezone=UTC
db.user=root
db.password=toor
```



