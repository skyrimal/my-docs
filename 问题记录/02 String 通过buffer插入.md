##  金额分转换元

```java
public static void main(String[] args) {
    StringBuffer sb = new StringBuffer("120145");
    sb.insert(sb.length()-2, '.');
    System.out.println(sb.toString()+"元");
}
```

![image-20210202104454156](C:%5CUsers%5CAdministrator%5CDesktop%5C%E5%AD%A6%E4%B9%A0%E6%96%87%E6%A1%A3%5Cmd%5C%E5%9B%BE%E7%89%87%5Cimage-20210202104454156.png)