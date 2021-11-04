## redis序列化出错

> Could not read JSON: Unexpected token (VALUE_NUMBER_FLOAT), expected VALUE_STRING: need JSON String that contains type id (for subtype of java.lang.Object)

###1. 出现原因推测，在静态方法中调用==redisTemplate.opsForHash().putAll(key, map);==导致序列化是出错。

解决方法1：将静态方法改为普通方法进行尝试 --失败

> 发现错误，在地图模块数据使用了相同的处理方法，但是没有出错

###2. 根据出错代码和成功代码对比，发现生成Key的方法不一样。出错代码Key是由静态方法生成

解决方法，同步Key生成方法 --失败

### 3. value使用了json序列化，导致上面直接设置的value字符串序列化时报解析json异常；

> 即，数字外没有引号

#### 发现redis的om.enableDefaultTyping(DefaultTyping.NON_FINAL);

根据下列描述，绝大多数基础数据类型（“自然”类型）都不在redis处理范围内

```java
		/**
         * Value that means that default typing will be used for
         * all non-final types, with exception of small number of
         * "natural" types (String, Boolean, Integer, Double), which
         * can be correctly inferred from JSON; as well as for
         * all arrays of non-final types.
         *<p>
         * Since 2.4, this does NOT apply to {@link TreeNode} and its subtypes.
         */
        NON_FINAL
//翻译
/*
值，该值表示将对
所有非最终类型，除了少数
“自然”类型（String、Boolean、Integer、Double），其中
可以从JSON中正确推断；以及
所有非final类型的数组。
从2.4开始，这不适用于{@linktreenode}及其子类型。 */
```

解决方法1：注释掉改设置，能够跑通获取方法，但是感觉不好





==**扩展：：**==设置为JAVA_LANG_OBJECT、NON_CONCRETE_AND_ARRAYS、OBJECT_AND_NON_CONCRETE时，保存的数据只是单纯的jjson字符串而没有转换为2进制，所以取出进行反序列化时会出错；

只有使用NON_FINAL时，会默认转换为2进制，但对String, Boolean, Integer, Double外的基础数据类型（int、double、long等）无效。

NOTE: use of Default Typing can be a potential security risk if incoming  content comes from untrusted sources, and it is recommended that this is either not done, or, if enabled, use {@link #setDefaultTyping} passing a custom {@link TypeResolverBuilder} implementation that white-lists  legal types to use.



==***ObjectMap.enableDefaultTyping上有提示：：***==

 注意：如果传入的内容来自不受信任的源，则使用默认类型可能是一个潜在的安全风险，建议不要这样做，或者，如果启用，请使用{@link#setDefaultTyping}传递一个自定义的{@link TypeResolverBuilder}实现，该实现列出了要使用的合法类型。 

## 扩展：：RedisTemplate初始化

```java
/**
 * RedisTemplate配置
 *
 * @param lettuceConnectionFactory
 * @return
 */
@Bean
public RedisTemplate<String, Object> redisTemplate(LettuceConnectionFactory lettuceConnectionFactory) {
   // 设置序列化
   Jackson2JsonRedisSerializer<Object> jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer<Object>(Object.class);
   ObjectMapper om = new ObjectMapper();
   om.setVisibility(PropertyAccessor.ALL, Visibility.ANY);
   /**
    *  使用后会限制序列化最底层成员的数据类型，只能使用“自然”类型（String、Boolean、Integer、Double）
    *  即非final类型 例：int,double等都会序列化出错
    */
   //om.enableDefaultTyping(DefaultTyping.NON_FINAL);
   jackson2JsonRedisSerializer.setObjectMapper(om);
   // 配置redisTemplate
   RedisTemplate<String, Object> redisTemplate = new RedisTemplate<String, Object>();
   redisTemplate.setConnectionFactory(lettuceConnectionFactory);
   RedisSerializer<?> stringSerializer = new StringRedisSerializer();
   redisTemplate.setKeySerializer(stringSerializer);// key序列化
   redisTemplate.setValueSerializer(jackson2JsonRedisSerializer);// value序列化
   redisTemplate.setHashKeySerializer(stringSerializer);// Hash key序列化
   redisTemplate.setHashValueSerializer(jackson2JsonRedisSerializer);// Hash value序列化
   redisTemplate.afterPropertiesSet();
   return redisTemplate;
}
```

