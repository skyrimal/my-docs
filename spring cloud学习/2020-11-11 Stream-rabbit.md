#sStream

Spring Cloud 整合Stream进行消息中间件的适配

![点击查看源网页](../cloud)

## 1. 对RabbitMQ==发送消息

### POM

```xml
        <!--spring-cloud整合stream-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-stream-rabbit</artifactId>
        </dependency>

        <!--eureka 服务注册客户端 netflix==豪猪哥-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
```

### YML

```yml
server:
  port: 8801

spring:
  application:
    name: cloud-payment-service #服务名，当注册入服务管理中心时显示
  cloud:
    stream:
      binders: #使用的服务信息
        defaultRabbit: #整合rabbitmq
          type: rabbit
          envieonment: #设置rabbitmq环境变量
            sping:
              rabbitmq:
                host: localhost
                port: 5672
                username: guest
                password: guest
      bindings:
        output: #输出通道
          destination: outputExchange1 # Exchange的名字
          content-type: application/json # 设置消息类型
          binder: defaultRabbit # 设置要绑定的服务配置


#进行注册的配置
eureka:
  #主机名称映射
  instance:
    instance-id: stream-send8801
    prefer-ip-address: true
    #修改向Eureka发布心跳时间（默认30秒）
    lease-renewal-interval-in-seconds: 1
    #Eureka注册中心收到最后一次心跳后的等待时间（默认90秒）
    lease-expiration-duration-in-seconds: 3
  client:
    service-url:
      #Eureka单机版
      #defaultZone: http://localhost:7001/eureka
      #Eureka集群版
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka
```

###创建消息发送通道

```java
@Slf4j
@EnableBinding(Source.class)//定义输出管道
public class IMessageProviderImpl implements IMessageProvider {

    @Resource
    private MessageChannel output;

    @Override
    public String sendMSG() {
        String serial = UUID.randomUUID().toString();
        output.send(MessageBuilder.withPayload(serial).build());
        log.info("******"+serial+"******");
        return null;
    }
}
```

## 2. RabbitMQ==消息接收

与消息发送方不同处在于

### 1. YML在binding下的通道是Input，而不再是output

###2. 对于通道的代码

```java
/**
 * @Description: 此处之间连接通道，不用进行访问
 * @author: skyrimal
 * @date: 2020年11月11日 15:07
 */
@Component
@EnableBinding(Sink.class)//指定通道为接收通道
public class ReceiveMessageListenerController {

    @StreamListener(Sink.INPUT)
    public void getMessage(Message<String> message){
        System.out.println("消费者8002"+message.getPayload());
    }
}
```

## 3. 消息分组==防止重复消费

在YML中binding下添加group组

```yml
spring:
    cloud:
        stream:
            bindings:
              input: #接收通道
                destination: outputExchange1 # Exchange的名字
                content-type: application/json # 设置消息类型
                binder: defaultRabbit # 设置要绑定的服务配置
                group: g1
```

> 同组数据不会重复消费、同组分布式默认轮询
>
> 不同组数据会重复消费
>
> 不设置分组会随机到不同的组，同时消息不会持久化，即接收通道下线后再上线不会接收到之间发送的消息
>
> 设置分组后消息会持久化，（启动消息接收后会接收到未启动时发送的消息）

