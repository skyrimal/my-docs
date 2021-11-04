#服务中心部署和配置获取

## 服务端

### GIT

```yml
config:
  message: 'something \n something'
  version: 1.0V
```



###POM

```xml
<!--配置中心-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-server</artifactId>
</dependency>
```

### YML  

==**注意：：**==消息配置后，下属文档读取的优先为bootstrap.yml

```yml
server:
  port: 3344

spring:
  application:
    name: cloud-config-server
  cloud:
    config:
      server:
        git:
          uri: https://github.com/skyrimal/spring-cloud-config-learn.git
          username: 3190540743@qq.com
          password: 1073135923yy
          # 搜索目录
          search-paths: config #文件夹名
      label: master #分支，也可以在git中设置默认分支，没有默认master
#由服务注册中心管理
eureka:
  client:
    service-url:
      defaultZone: http://localhost:7001/eureka
```

```java
@EnableConfigServer//启动消息中心服务器端
```

## 消费端

###POM

```xml
<!--配置中心消费端-->
<!-- spring cloud config 客户端包 -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
```

### YML 消息中心默认最高读取为bootstrap.yml

```yml
server:
  port: 3355

spring:
  application:
    name: cloud-config-client
  cloud:
    # config 客户端配置
    config:
      label: master # 分支名称 不配默认
      name: config # 资源名称
      profile: dev # 后缀名称
      uri: http://localhost:3344 # 配置中心地址
eureka:
  client:
    service-url:
      # 单机版
      defaultZone: http://localhost:7001/eureka
```

### Controller

```java
@RestController
public class ConfigClientController {

    @Value("${config.message}")
    private String massage;

    @Value("${config.version}")
    private String version;

    @GetMapping("configInfo")
    public String getConfigInfo() {
        return massage + ":" + version;
    }
}
```





# 修改使发送post既可以刷新配置

## 优化1：：3355端

### YML

添加如下配置暴露刷新接口

```yml
management:
  endpoints:
    web:
      exposure:
        include: "*"
```

### controller

使用==***@RefreshScope***==注解，使本区域的配置信息可以被刷新

使用==**curl -X POST **==**"http://localhost:3355/actuator/refresh"**

> 返回  ["config.version","config.client.version"]
>
> 本方法只能一个个的对服务发应用进行刷新

## 优化2：：3344配置中 心服务器端

[2020-11-9 config使用bus消息总线.md](2020-11-9 config使用bus消息总线.md) 