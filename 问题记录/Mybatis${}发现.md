m(String n,T m)

传入多个参数时${} 不要求封装调用get方法，原因不明

下例Mapper没有报需要getter的错

```java
List<RepayPathDO> selectRepayDataByTaskIdAndTaskInfoDO(String tableName,
                                                       DarailedTaskInfoDO taskInfoDO);
```