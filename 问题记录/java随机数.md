第一种：new Random() 

第二种：Math.random()==>double 0-1

第三种：currentTimeMillis()



```java
        //创建Random类对象
        Random random = new Random();              
         
        //产生随机数
        int number = random.nextInt(END - START + 1) + START;
```

