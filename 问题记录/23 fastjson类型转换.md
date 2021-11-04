## java.util.LinkedHashMap cannot be cast to com.alibaba.fastjson.JSONObject

原因

JSONArray里面是

解决代码

```java
JSONArray term = json.getJSONArray(info.getTerms());
for (JSONObject o : term.toJavaList(JSONObject.class)) {
    System.out.println(o.toJSONString());
}
```