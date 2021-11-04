# springboot文件优先级



**springboot读取外部配置文件的方法，如下优先级：**

第一种是在执行命令的目录下建config文件夹。（在jar包的同一目录下建config文件夹，执行命令需要在jar包目录下才行），然后把配置文件放到这个文件夹下。

第二种是直接把配置文件放到jar包的同级目录。

第三种在classpath下建一个config文件夹，然后把配置文件放进去。

第四种是在classpath下直接放配置文件。

springboot默认是优先读取它本身同级目录下的一个config/application.properties 文件的。

在src/main/resources 文件夹下创建的application.properties 文件的优先级是最低的