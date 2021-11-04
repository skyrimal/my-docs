# shiro+pagehelper 赋权后运行时pagehelper配置失效

##2020.5.13

```xml
		<!--shiro-->
		<dependency>
		    <groupId>org.apache.shiro</groupId>
		    <artifactId>shiro-spring-boot-starter</artifactId>
		    <version>1.4.0</version>
		</dependency>
		<!-- shiro-redis -->
        <dependency>
            <groupId>org.crazycake</groupId>
            <artifactId>shiro-redis</artifactId>
            <version>3.1.0</version>
            <exclusions>
                <exclusion>
                    <groupId>org.apache.shiro</groupId>
                    <artifactId>shiro-core</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        
        <!-- 分页插件 -->
		<!-- https://mvnrepository.com/artifact/com.github.pagehelper/pagehelper -->
		<dependency>
			<groupId>com.github.pagehelper</groupId>
			<artifactId>pagehelper</artifactId>
			<version>4.2.1</version>
		</dependency>

```

当使用shiro给接口赋权以后，干扰了mabatis拼接sql的监听，pagehelper设置了helperDialect属性 就起不了作用，相反会强制按照mysql格式进行拼接sql

需要limit 

修改结果

初始化pagehelper时不要设置helperDialect，不设置的化pagehelper会默认加载匹配的helperDialect

```java
properties.setProperty("helperDialect","postgresql");
```



## 2020.5.14

不设置helperDialect后，多数据源情况下随缘加载方言，

可能上一次按mysql拼接limit ?,? ，下一次就有按postgresql拼接 limit ? offset ?

pagehelper更换5.1.11，推荐不设置方言

> 没有作用，依旧随缘加载方言

pagehelper更换4.1.4，方言属性设置为dialect

> 目前没有报错



