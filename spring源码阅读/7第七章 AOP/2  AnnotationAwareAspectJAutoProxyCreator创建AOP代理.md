# 创建AOP代理

## 1. AnnotationAwareAspectJAutoProxyCreator类分析

AnnotationAwareAspectJAutoProxyCreator类图

![image-20210824092831770](2%20%20AnnotationAwareAspectJAutoProxyCreator%E5%88%9B%E5%BB%BAAOP%E4%BB%A3%E7%90%86.assets/image-20210824092831770.png)

如图可见，自动代理创造器本质上是一个**==BeanPostProcessor==**，即，会在类创建后加工类

>BeanPostProcessor在初始化上下文时就已经加载
>
>每次调用时都是从上下文中获取后处理器并调用

AnnotationAwareAspectJAutoProxyCreator在每个使用了AOP的bean实例化时都会被加载，然后执行，生成对应bean的增强器并交由其代理持有

其postProcessBeforeInstantiation()方法由其父类的父类AbstractAutoProxyCreator持有