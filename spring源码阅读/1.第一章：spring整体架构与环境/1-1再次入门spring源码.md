# 入门

时隔半年再再再次入坑

![image-20210218161836387](1-1%E5%86%8D%E6%AC%A1%E5%85%A5%E9%97%A8spring%E6%BA%90%E7%A0%81.assets/image-20210218161836387.png)

 

![image-20210218162341574](1-1%E5%86%8D%E6%AC%A1%E5%85%A5%E9%97%A8spring%E6%BA%90%E7%A0%81.assets/image-20210218162341574.png)



### BeanDefinitionReader::ioc读取配置（为了进行加工实例化相应的对象）

> 反射实现实例化

### 实例化：在堆中开辟空间

### 初始化：完成对象属性赋值（可以通过配置调用具体的初始化方法）

![image-20210218170632523](E:%5Cmd%5C%E5%90%8E%E7%AB%AF%5Cspring%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%90%5C2021spring%E6%BA%90%E7%A0%81%E5%AD%A6%E4%B9%A0%5C02%5C18%E5%86%8D%E6%AC%A1%E5%85%A5%E9%97%A8spring%E6%BA%90%E7%A0%81.assets%5Cimage-20210218170632523.png)

### PostProcessor：增强器/后置处理器

> 为了扩展实现

#### BeanFactoryPostcessor

> 可以通过继承BeanFactoryPostcessor在实例化前实现postProcessBeanFactory()来对BeanDefiftion进行再次加工

#### BeanPostProcesser

> 在实例化结束后初始化时加工对象

![image-20210218171316344](E:%5Cmd%5C%E5%90%8E%E7%AB%AF%5Cspring%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%90%5C2021spring%E6%BA%90%E7%A0%81%E5%AD%A6%E4%B9%A0%5C02%5C18%E5%86%8D%E6%AC%A1%E5%85%A5%E9%97%A8spring%E6%BA%90%E7%A0%81.assets%5Cimage-20210218171316344.png)



## spring 循环依赖

属性注入-自动注入

