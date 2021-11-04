# 安装nacos

[Nacos使用文档](https://nacos.io/zh-cn/docs/quick-start-spring-cloud.html)

下载1.3.2

解压

启动单机版 cmd startup.cmd -m standalone

## 集群版启动

conf下application.properties添加mysql连接

```properties
spring.datasource.platform=mysql
db.num=1
db.url.0=jdbc:mysql://${server}:${port}/nacos?useUnicode=true&useJDBCCompliantTimezoneShift=true&useLegacyDatetimeCode=false&serverTimezone=UTC
db.user=${user}
db.password=${password}
```

拷贝cluster.conf.example为cluster.conf，修改ip

127.0.0.1:8847
127.0.0.1
127.0.0.1



# Nacos概念

## 命名空间

用于进行租户粒度的配置隔离。不同的命名空间下，可以存在相同的 Group 或 Data ID 的配置。Namespace 的常用场景之一是不同环境的配置的区分隔离，例如开发测试环境和生产环境的资源（如配置、服务）隔离等。

![img](nacos%E6%A6%82%E5%BF%B5)

# 服务注册进Nacos

开启nacos注册的注解：：@EnableDiscoveryClient

> nocos服务提供方和消费方的配置方法相同
>
> 引入的nacos依赖自带有ribbin负载均衡

##1.POM

###父POM

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
      		<groupId>com.alibaba.cloud</groupId>
      		<artifactId>spring-cloud-alibaba-dependencies</artifactId>
      		<version>${alibaba.cloud}</version>
      		<type>pom</type>
      		<scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

### 服务实例POM

```xml
    <!--整合nacos cloud alibaba-->
    <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
    </dependency>
```

### YML

> 下方是官网提供的基础配置

```yml
server:
  port: 9002
spring:
  application:
    name: nacos-payment-provider
  cloud:
    nacos:
      discovery:
        server-addr: 127.0.0.1:8848
management:
  endpoints:
    web:
      exposure:
        include: '*'
```

以下显示了Nacos Discovery启动器的其他配置：

| 组态          | 键                                               | 默认值                       | 描述                                                         |
| ------------- | ------------------------------------------------ | ---------------------------- | ------------------------------------------------------------ |
| 服务器地址    | `spring.cloud.nacos.discovery.server-addr`       |                              | Nacos服务器侦听器的IP和端口                                  |
| 服务名称      | `spring.cloud.nacos.discovery.service`           | `${spring.application.name}` | 命名当前服务                                                 |
| 重量          | `spring.cloud.nacos.discovery.weight`            | `1`                          | 值范围：1到100。值越大，重量越大。                           |
| 网卡名称      | `spring.cloud.nacos.discovery.network-interface` |                              | 如果未指定IP地址，则注册的IP地址是网卡的IP地址。如果也未指定，默认情况下将使用第一个网卡的IP地址。 |
| 注册IP地址    | `spring.cloud.nacos.discovery.ip`                |                              | 最高优先级                                                   |
| 注册端口      | `spring.cloud.nacos.discovery.port`              | `-1`                         | 默认情况下将自动检测。不需要配置。                           |
| 命名空间      | `spring.cloud.nacos.discovery.namespace`         |                              | 一个典型的场景是隔离针对不同环境的服务注册，例如测试和生产环境之间的资源（配置，服务等）隔离 |
| 快捷键        | `spring.cloud.nacos.discovery.access-key`        |                              | 阿里云帐户访问密钥                                           |
| 密钥          | `spring.cloud.nacos.discovery.secret-key`        |                              | 阿里云账户密钥                                               |
| 元数据        | `spring.cloud.nacos.discovery.metadata`          |                              | 您可以使用地图格式为服务定义一些元数据                       |
| 日志文件名    | `spring.cloud.nacos.discovery.log-name`          |                              |                                                              |
| 集群名称      | `spring.cloud.nacos.discovery.cluster-name`      | `DEFAULT`                    | Nacos的群集名称                                              |
| 终点          | `spring.cloud.nacos.discovery.endpoint`          |                              | 特定服务在特定区域中的域名。您可以使用该域名动态检索服务器地址 |
| 是否集成色带  | `ribbon.nacos.enabled`                           | `true`                       | 在大多数情况下设置为true                                     |
| 启用Nacos手表 | `spring.cloud.nacos.discovery.watch.enabled`     | `true`                       | 设置为false以关闭手表                                        |