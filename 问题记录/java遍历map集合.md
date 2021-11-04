### 方法一：通过Map.keySet，遍历key和value

```java
Map<String, Object> map = new HashMap<>();
for (String key : map.keySet()) {
    System.out.println("key=" + key + ", value=" + map.get(key));
}
```

### 方法二：通过Map.values()，遍历所有的value，但不能遍历key

```java
Map<String, Object> map = new HashMap<>();
for (Object value : map.values()) {
    System.out.println("value=" + value);
}
```

### 方法三：通过Map.entrySet，遍历key和value

```java
Map<String, Object> map = new HashMap<>();
for (Map.Entry<String, Object> entry : map.entrySet()) {
    System.out.println("key=" + entry.getKey() + ", value=" + entry.getValue());
}
```

### 方法四：通过Map.entrySet，使用iterator遍历key和value

```java

Map<String, Object> map = new HashMap<>();
Iterator<Map.Entry<String, Object>> iterator = map.entrySet().iterator();
while (iterator.hasNext()) {
    Map.Entry entry = iterator.next();
    System.out.println("key=" + entry.getKey() + ", value=" + entry.getValue());
}
```