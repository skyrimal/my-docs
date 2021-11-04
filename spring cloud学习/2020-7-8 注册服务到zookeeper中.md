# 注册服务到zookeeper中



## 启动虚拟机zookeeper，并关闭虚拟机防火墙（linux可以设置允许访问的ip）

## 创建8004Payment应用

### pom

> ==注意：：==pom中spring-cloud-starter-zookeeper-discovery为zookeeper管理依赖，该包默认依赖zookeeper依赖包，但是依赖包版本可能错误，要判断匹配的zookeeper版本进行替换
>
> ==注意：：==替换zookeeper依赖时会与lombok默认SLF4J包下依赖冲突，原因是使用的底层日志框架冲突，建议去除新增的zookeeper中日志框架的依赖(谁新增去除谁)
>
> ```xml
> 			   <!--排除这个slf4j-log4j12-->
>                 <exclusion>
>                     <groupId>org.slf4j</groupId>
>                     <artifactId>slf4j-log4j12</artifactId>
>                 </exclusion>
> ```
>
> 
>
> 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>cloud2020</artifactId>
        <groupId>com.atguigu.springcloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-provider-payment8004</artifactId>


    <dependencies>
        <!-- spring-boot基础依赖 -->
        <!-- web依赖 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <exclusions>
                <!--排除SLF4J日志异常-->
                <!--排除这个slf4j-log4j12-->
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>slf4j-log4j12</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <!-- 图像处理图形化展示和坐标监控 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <!-- 跟cloud同组，默认同version ，一般为zookeeper注册中心处理应用-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-zookeeper-discovery</artifactId>
            <!-- 排除自带zookeeper -->
            <exclusions>
                <exclusion>
                    <groupId>org.apache.zookeeper</groupId>
                    <artifactId>zookeeper</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <!-- 添加对应版本的zookeeper -->
        <dependency>
            <groupId>org.apache.zookeeper</groupId>
            <artifactId>zookeeper</artifactId>
            <version>3.5.8</version>
            <exclusions>
                <!--排除SLF4J日志异常-->
                <!--排除这个slf4j-log4j12-->
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>slf4j-log4j12</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>com.atguigu.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
        </dependency>
    </dependencies>
</project>
```

### yml

```yml
server:
  port: 8004

#服务别名
spring:
  application:
    name: cloud-provider-payment
  cloud:
    zookeeper:
      #注册进的zookeeper地址
      connect-string: 192.168.42.25:2181
```

### 主启动类PaymentMain8004

```java
@SpringBootApplication
//注册到服务发现组件
//8004注册服务的注册中心是zookeeper
//配置服务发现，获取注册中心的基础信息
@EnableDiscoveryClient
public class PaymentMain8004 {
    public static void main(String[] args) {
        SpringApplication.run(PaymentMain8004.class,args);
    }
}
```

### 业务类9

```java
@RestController
@Slf4j
public class PaymentController {

    @Value("${server.port}")
    private String servicePort;

    @RequestMapping(value = "/payment/zk")
    public String paymentZookeeperInfo(){
        return "springcloud with zookeeper: "+servicePort+"\t"+ UUID.randomUUID().toString();
    }
}

```

### ==启动完成后的zookeeper节点和访问结果==

####zookeeper添加/services根节点，并在其下添加注册进的服务别名的子节点

![zookeeper节点](../%E5%9B%BE%E7%89%87/%E6%B3%A8%E5%86%8C%E5%85%A5zookeeper%E6%88%90%E5%8A%9F%E5%90%8E.png)

#### 根节点下有服务实例流水号节点，格式类似UUID

> 节点是临时性的，默认不持久

![服务流水号](../%E5%9B%BE%E7%89%87/%E6%9C%8D%E5%8A%A1%E6%B5%81%E6%B0%B4%E5%8F%B7.png)

##### 从流水号节点中可以获取注册的服务信息，json格式，节点

> 节点像Eureka心跳一样，在一定时间内会保存没有心跳的服务实例，在一段时间没有接收到心跳后自动删除节点

![获取流水号节点](../%E5%9B%BE%E7%89%87/%E8%8E%B7%E5%8F%96zookeeper%E6%B5%81%E6%B0%B4%E5%8F%B7%E8%8A%82%E7%82%B9%E7%9A%84%E6%95%B0%E6%8D%AE.png)

```json
{
    "name":"cloud-provider-payment",
    "id":"802ca903-2a79-478c-936f-5f3eff23a63f",
    "address":"localhost",
    "port":8004,
    "sslPort":null,
    "payload":{
        "@class":"org.springframework.cloud.zookeeper.discovery.ZookeeperInstance",
        "id":"application-1",
        "name":"cloud-provider-payment",
        "metadata":{

        }
    },
    "registrationTimeUTC":1594217217014,
    "serviceType":"DYNAMIC",
    "uriSpec":{
        "parts":[
            {
                "value":"scheme",
                "variable":true
            },
            {
                "value":"://",
                "variable":false
            },
            {
                "value":"address",
                "variable":true
            },
            {
                "value":":",
                "variable":false
            },
            {
                "value":"port",
                "variable":true
            }
        ]
    }
}
```

###访问注册后的应用服务的接口

![访问效果](../%E5%9B%BE%E7%89%87/%E8%8C%83%E5%9B%B4%E6%B3%A8%E5%86%8C%E5%88%B0zookeeper%E4%B8%8A%E7%9A%84%E5%BA%94%E7%94%A8%E6%9C%8D%E5%8A%A1.png)

## 创建cloud-consumerzk-order80

> 做注册入zookeeper的服务的消费应用

 ### pom

### yml

###主启动类

### ==基本跟8004服务相同==

### 写config

> @Bean，通过spring-boot框架注册进容器中的RestTemplateBuilder.build()返回restTemplate，必须加@LoadBalanced，除了负载均衡外，还有服务应用名转ip的作用

### 写controller

```java
@RestController
@Slf4j
public class OrderZkController {
    public static final String INVOKE_URL = "https://cloud-provider-payment";

    private RestTemplate restTemplate;

    @Autowired
    public void setRestTemplate(RestTemplate restTemplate) {
        this.restTemplate = restTemplate;
    }

    @RequestMapping(value = "/consumer/payment/zk")
    public String paymentInfo(){
        String result = restTemplate.getForObject(INVOKE_URL+"/payment/zk",String.class);
        return "orderZk80调用接口获取到payment8004的数据：\n"+result;
    }
}
```

### 启动后

![image-20200708230030141](../%E5%9B%BE%E7%89%87/order%E6%B3%A8%E5%86%8C%E5%85%A5zookeeper.png)

####访问报错

@loadBalance注解修饰的restTemplate才能实现服务名的调用，没有修饰的restTemplate是没有该功能的。@loadBalance是Netflix的ribbon中的一个负载均衡的注解，当我项目加了loadbalacnce的时候，就可以了。


至于为什么一定要该注解修饰，这里我大概讲一下。loadBalance这个注解加上之后，这个注解有3件事情要处理。

    第一件就是从负载均衡器中选一个对应的服务实例，那有的人就会问为什么从负载均衡器中挑选，原因很明显就是，所有的服务名实例都放在负载均衡器中的serverlist。
    
    第二件事情就是从第一件事情挑选的实例中去请求内容。
    
    第三件事情就是由服务名转为真正使用的ip地址

### ==restTemplate==只能访问http，要访问https要在config里添加配置

​	