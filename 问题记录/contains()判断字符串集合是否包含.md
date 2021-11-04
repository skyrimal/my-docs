String类型有一个方法：contains（）,该方法是判断字符串中是否有子字符串。如果有则返回true，如果没有则返回false。

```java
if(map_string.contains("name")){
    System.out.println("找到了name的key");
}
if(map_string.contains("password")){
    System.out.println("找到了password的key");
}
```

2.list.contains(o)，比较list是否包含o

系统会对list中的每个元素e调用o.equals(e)，方法，加入list中有n个元素，那么会调用n次o.equals(e)，只要有一次o.equals(e)返回了true，那么list.contains(o)返回true，否则返回false。因此为了很好的使用contains()方法，我们需要重新定义下Student类的equals方法，根据我们的业务逻辑，如果两个Student对象的orderId相同，那么我们认为它们代表同一条记录 :

ArrayList的contains方法的实现：

```java
    public boolean contains(Object o) {
        return indexOf(o) >= 0;
    }
    public int indexOf(Object o) {
        if (o == null) {
            for (int i = 0; i < size; i++)
                if (elementData[i]==null)
                    return i;
        } else {
            for (int i = 0; i < size; i++)
                if (o.equals(elementData[i]))
                    return i;
        }
        return -1;
    }
```
