首先，new Date（）的时间是获得java虚拟机jvm的时间。

所以问题所在就是jvm时间不正确。

修改jvm的时间：

 	方法一：在new Date（）前面加

```java


          //修改jvm时间
        TimeZone tz = TimeZone.getTimeZone("ETC/GMT-8");    
        TimeZone.setDefault(tz);
```
无效