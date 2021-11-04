# 使用nacos管理配置文件

## 1.POM

```xml
<!--整合nacos config cloud alibaba 配置信息管理功能-->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
</dependency>
```

##2. yml

> 使用SpringCloudConfig这种统一配置时Spring Boot 配置文件的**加载顺序**，
>
> 依次为 `bootstrap.properties` -> `bootstrap.yml` ->`application.properties` -> `application.yml`
>
> 并通过多次测试，连接config的代码如果不在bootstrap.yml文件中会直接导致项目驱动报错
>
> 可以使用“ spring.cloud.nacos.config.refresh.enabled = false”设置禁用自动刷新。
>
> 只有@RefreshScope注解下的类才能刷新获取配置信息

nacos发布的配置信息绑定服务的方法是通过DataId和GROUP来确认发布的服务

==*DatataId命名*== 规则：：{sing.name}-{spring.profiles.active}.{spring.cloud.nacos.config.file-extension}

### application.yml

```yml
spring:
  profiles:
    active: dev
```



### bootstrap.yml

```yml
server:
  port: 3377
spring:
  application:
    name: nacos-payment-provider
  cloud:
    nacos:
      discovery:
        server-addr: 127.0.0.1:8848
      config:
        server-addr: 127.0.0.1:8848
        file-extension: yaml
        #指明使用的配置文件位置，类似maven，三位一体
        #group: ***
        #namespace: ###
management:
  endpoints:
    web:
      exposure:
        include: '*'
```

### controler

```java
@RefreshScope
@RestController
public class NacosController {
    @Value("${config.info}")
    private String configInfo;

    @GetMapping("/echo/app-name")
    public String echoAppName(){
        return configInfo;
    }
}
```

