# 注册逻辑

服务将自身信息以key-value形式注册到Eureka中，key=服务名 value=服务地址

要实现注册首先要启动Eureka服务注册中心，然后启动要注册的应用。应用在启动时通过@EnableEurekaClient和配置文件注册到集群中

注册后，消费者再进行调用服务提供方时，会使用服务方的服务名去==**注册中心获取RPC远程调用地址**==，后使用HttpClient实现远程调用接口

消费者获取到服务方RPC地址后会暂时储存到本地jvm，默认30秒刷新一次

# Eureka集群

## 互相注册，相互守望

当某一个Eureka服务器挂掉后，注册中心仍可继续运行

修改hosts文件，添加本地域名映射(修改不了先看是不是etc文件夹被设置成只读模式，之后再看hosts文件权限)

```properties
127.0.0.1 eureka7001.com
127.0.0.1 eureka7002.com
```



yml配置，相互守望

```yml
#7001yml
server:
  port: 7001

eureka:
  instance:
    hostname: eureka7001.com #eureka服务器实例名  对应本地127.0.0.1映射
  client:
    register-with-eureka: false #禁止自己当做服务注册 #服务注册中心不管理自身
    fetch-registry: false #屏蔽注册信息 #不检索（监听？？）没道理检索自己，要检索的是注册入服务器的应用微服务
    service-url:
      #注册eureka 将自己注册到7001中
      defaultZone: http://eureka7002.com:7002/eureka
#7002yml
server:
  port: 7002

eureka:
  instance:
    hostname: eureka7002.com #eureka服务器实例名  对应本地127.0.0.1映射
  client:
    register-with-eureka: false #禁止自己当做服务注册 #服务注册中心不管理自身
    fetch-registry: false #屏蔽注册信息 #不检索（监听？？）没道理检索自己，要检索的是注册入服务器的应用微服务
    service-url:
            #注册eureka 将自己注册到7002中
      defaultZone: http://eureka7001.com:7001/eureka
```



## 启动后

7001

![image-20200630220040426](../%E5%9B%BE%E7%89%87/7001%E5%90%AF%E5%8A%A8%E9%9B%86%E7%BE%A4.png)

7002

![image-20200630220004478](../%E5%9B%BE%E7%89%87/7002%E5%90%AF%E5%8A%A8%E9%9B%86%E7%BE%A4.png)

## 应用注册到Eureka集群上

payment8001修改yml

```yml
#进行注册的配置
eureka:
  client:
    #是否注册进服务注册中心==默认true
    register-with-eureka: true
    #注册后服务注册中心是否监控==默认true
    fetch-registry: true
    service-url:
      #注册到==服务器注册中心开发的注册渠道
      #Eureka单机版
      #defaultZone: http://localhost:7001/eureka
      #Eureka集群版
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka
```

oder80修改yml

```yml
eureka:
  client:
    service-url:
      #Eureka单机版
      #defaultZone: http://localhost:7001/eureka
      #Eureka集群版
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka
```

修改启动后

eureka7001

![image-20200630220925822](../%E5%9B%BE%E7%89%87/%E5%BA%94%E7%94%A8%E6%B3%A8%E5%86%8C%E9%9B%86%E7%BE%A4%E5%90%8E7001.png)

eureka7002

![image-20200630221022245](../%E5%9B%BE%E7%89%87/%E5%BA%94%E7%94%A8%E6%B3%A8%E5%86%8C%E9%9B%86%E7%BE%A4%E5%90%8E7002.png)

## payment服务方集群注册到Eureka集群中

创建微服务payment8002（只有yml端口和启动类名不同）

controller添加功能

```java
    //获取端口后打印
	@Value("${server.port}")
    private String serverPort;
```

启动后8001、8002分别获取数据

```json
{
    "code": 200,
    "message": "查询成功serverPort8001",
    "data": {
        "id": 41,
        "serial": "尝试从order插入数据"
    }
}
{
    "code": 200,
    "message": "查询成功serverPort8002",
    "data": {
        "id": 31,
        "serial": "skyrimal学习spring-boot"
    }
}
```

oder80修改controller

```java

    //不再写死获取数据的地址
    //private static final String PAYMENT_URL= "http://localhost:8001/payment";
    //通过注册进注册中心的服务名来调用服务提供方的接口
    private static final String PAYMENT_URL= "http://CLOUD-PAYMENT-SERVICE/";
```

**更换后必须启动负载均衡功能，否则无法从服务方集群调用正确接口，进而抛出异常**

![image-20200630224439956](../%E5%9B%BE%E7%89%87/%E4%B8%8D%E9%85%8D%E7%BD%AE%E8%B4%9F%E8%BD%BD%E5%9D%87%E8%A1%A1%E5%BC%82%E5%B8%B8.png)

**负载均衡功能是添加到RestTemplate上的，springcloud集成eureka时默认使用Ribbon进行负载均衡**

![image-20200630224913179](../%E5%9B%BE%E7%89%87/spingcloud%E9%9B%86%E6%88%90eureka%EF%BC%8C%E9%BB%98%E8%AE%A4%E9%9B%86%E6%88%90%E4%BA%86Ribbon%E7%94%A8%E6%9D%A5%E8%BF%9B%E8%A1%8C%E8%B4%9F%E8%BD%BD%E5%9D%87%E8%A1%A1.png)

```java
/**
 * @Description: TODO
 * @author: skyrimal
 * @date: 2020年06月18日 23:08
 */
@Configuration
public class ApplicationConfig {

    @Bean
    @LoadBalanced//生成bean时使用注解启动Ribbon负载均衡功能,默认轮询
    public RestTemplate restTemplate(RestTemplateBuilder builder) {

        return builder.build();

    }
}
```



## 主机名称映射

yml修改

```yml
eureka:
  #主机名称映射
  instance:
    instance-id: payment8001
    #显示ip地址
    prefer-ip-address: true
```

# **补充：：Spring Boot Actuator**

```xml
		<!-- 图像处理图形化展示和坐标监控 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
```



Spring Boot Actuator可以帮助你监控和管理Spring Boot应用，比如健康检查、审计、统计和HTTP追踪等。所有的这些特性可以通过JMX或者HTTP endpoints来获得

| 路径         | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| /autoconfig  | 提供了一份自动配置报告，记录哪些自动配置条件通过了，哪些没通过 |
| /beans       | 描述应用程序上下文里全部的Bean，以及它们的关系               |
| /env         | 获取全部环境属性                                             |
| /configprops | 描述配置属性(包含默认值)如何注入Bean                         |
| /dump        | 获取线程活动的快照                                           |
| /health      | 报告应用程序的健康指标，这些值由HealthIndicator的实现类提供  |
| /info        | 获取应用程序的定制信息，这些信息由info打头的属性提供         |
| /mappings    | 描述全部的URI路径，以及它们和控制器(包含Actuator端点)的映射关系 |
| /metrics     | 报告各种应用程序度量信息，比如内存用量和HTTP请求计数         |
| /shutdown    | 关闭应用程序，要求endpoints.shutdown.enabled设置为true       |
| /trace       | 提供基本的HTTP请求跟踪信息(时间戳、HTTP头等)                 |

### 安全措施

如果上述请求接口不做任何安全限制，安全隐患显而易见。实际上Spring Boot也提供了安全限制功能。比如要禁用/env接口，则可设置如下：

```
endpoints.env.enabled= false
```

如果只想打开一两个接口，那就先禁用全部接口，然后启用需要的接口：

```
endpoints.enabled = false
endpoints.health.enabled = true
```

另外也可以引入spring-boot-starter-security依赖

```
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

在application.properties中指定actuator的端口以及开启security功能，配置访问权限验证，这时再访问actuator功能时就会弹出登录窗口，需要输入账号密码验证后才允许访问。

```
management.port=8099
management.security.enabled=true
security.user.name=admin
security.user.password=admin
```

actuator暴露的health接口权限是由两个配置： `management.security.enabled` 和 `endpoints.health.sensitive`组合的结果进行返回的。

| management.security.enabled | endpoints.health.sensitive | Unauthenticated | Authenticated |
| --------------------------- | -------------------------- | --------------- | ------------- |
| false                       | **false**                  | Full content    | Full content  |
| false                       | true                       | Status only     | Full content  |
| true                        | **false**                  | Status only     | Full content  |
| true                        | true                       | No content      | Full content  |



### 安全建议

在使用Actuator时，不正确的使用或者一些不经意的疏忽，就会造成严重的信息泄露等安全隐患。在代码审计时如果是springboot项目并且遇到actuator依赖，则有必要对安全依赖及配置进行复查。也可作为一条规则添加到黑盒扫描器中进一步把控。
 安全的做法是一定要引入security依赖，打开安全限制并进行身份验证。同时设置单独的Actuator管理端口并配置不对外网开放。