# 注册AnnotationAwareAspectJAutoProxyCreator

aop的核心逻辑就是给切点添加增强器，当调用切点时，代理器跟着就调用切点对应的增强器以实现切面代码织入切点

## 1. 调用

 解析BeanDefinition时，增强或者自定义的则走parseCustomElement，会拿到url然后去决定是哪个NamespaceHandler对应的BeanDefinitionParser，再调用他的parse方法。

### 在 **AspectJAutoProxyBeanDefinitionParser**中调用其注册方法

```java
@Override
@Nullable
public BeanDefinition parse(Element element, ParserContext parserContext) {
   //尝试注册AspectJAnnotationAutoProxyCreator
   AopNamespaceUtils.registerAspectJAnnotationAutoProxyCreatorIfNecessary(parserContext, element);
   //对注解中子类进行处理
   extendBeanDefinition(element, parserContext);
   return null;
}
```

### 注册步骤

解析出beanName为 ' org.springframework.aop.config.internalAutoProxyCreator '的beanDefinition并将其注册为组件

```java
public static void registerAspectJAnnotationAutoProxyCreatorIfNecessary(
      ParserContext parserContext, Element sourceElement) {

   //注册或升级AutoProxyCreator，
   //定义beanName为 ' org.springframework.aop.config.internalAutoProxyCreator ' 的beanDefinition
   //如果容器中有重复Bean，那么会按照优先级选择是否覆盖
   BeanDefinition beanDefinition = AopConfigUtils.registerAspectJAnnotationAutoProxyCreatorIfNecessary(
         parserContext.getRegistry(), parserContext.extractSource(sourceElement));
   //在上下文中对prosy-target-class 和 expose-proxy 属性的处理
   useClassProxyingIfNecessary(parserContext.getRegistry(), sourceElement);
   //将AOP组件注册进上下文并通知，便于监听器做进一步处理
   //beanDefinition的className为AnnotationAwareAspectJAutoProxyCreator
   //AOP的实现基本是靠AnnotationAwareAspectJAutoProxyCreator实现的
   registerComponentIfNecessary(beanDefinition, parserContext);
}
```

## AnnotationAwareAspectJAutoProxyCreator类图

![image-20210824092831770](1%20%E6%B3%A8%E5%86%8CAnnotationAwareAspectJAutoProxyCreator.assets/image-20210824092831770.png)