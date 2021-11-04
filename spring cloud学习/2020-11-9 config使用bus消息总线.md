# 安装rabbitmq消息中间件到windows

## 1.安装前置环境erlang

[erlang下载地址](https://www.erlang.org/downloads)

配置环境变量 类似java

![image-20201109145213868](../image-20201109145213868.png)

然后path添加

![image-20201109145240677](../image-20201109145240677.png)

## 2.下载安装rabbitmq

下载安装后进入通过管理员打开powerShell 进入sbin文件夹

rabbitmqctl status 查询rabbitmq状态

rabbitmq-plugins.bat enable rabbitmq_management 启动rabbitmq

rabbitmq-server.bat 启动rabbitmq网页服务器

net stop RabbitMq && net start RabbitMq 关闭/启动rabbitmq



## 3.cloud config使用rabbitMQ通知服务

给3344、3355、3366都添加pom、yml对rabbitmq的依赖

```xml
<!--消息总线-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-bus-amqp</artifactId>
</dependency>
```

```yml
spring:
  # spring.rabbitmq
  # rabiitmq配置
  rabbitmq:
    host: localhost
    port: 5672
    username: guest
    password: guest
```



3344服务器端暴露下方刷新接口

```yml
management:
  endpoints:
    web:
      exposure:
        # 暴露bus刷新端点
        include: 'bus-refresh'
```



向总线发送post刷新指令

==**curl -X POST **==**"http://localhost:3344/actuator/bus-refresh"**