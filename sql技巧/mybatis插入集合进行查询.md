不要插入list，即使是list也要放进map后再进行插入

```
注意 你可以传递一个 List 实例或者数组作为参数对象传给 MyBatis。当你这么做的时 候,MyBatis 会自动将它包装在一个 Map 中,用名称在作为键。List 实例将会以“list” 作为键,而数组实例将会以“array”作为键。
```



例如：

```java
    public int getStudentCount(List<String> studentNameList){
    	//把参数手动封装在Map中
    	Map<String, Object> map = new HashMap<String, Object>();
    	map.put("studentNameList", studentNameList);
    	return super.count("getStudentCount", map);
    }
```





第一步在你的mapper写上:

```java
 List<WeixinUserLocationList> findweixinUserLocations(@Param("params") Map<String, Object> map);
```

注意就是注解@param 这个，是mybatis的

然后在xml中这样写:

```xml
	    <if test="params.accountId!=null">
            and a.accountid=#{params.accountId}
        </if>
        <if test="params.nickname!=null and params.nickname !=''">
            and a.nickname like '%${params.nickname}%'
        </if>
        <if test="params.beginDate!=null and params.beginDate!=''">
            and date_format(a.createtime,'%Y-%m-%d')>=${params.beginDate}
        </if>
        <if test="params.endDate!=null and params.endDate!=''">
             <![CDATA[    and date_format(a.createtime,'%Y-%m-%d')<=${params.endDate}  ]]>     
        </if>
```

${params.nickname}这种写法参数默认是传字符串，
\#{params.accountId}可以取Long，Integer之类的。x