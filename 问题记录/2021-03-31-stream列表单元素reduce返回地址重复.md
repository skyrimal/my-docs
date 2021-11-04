## reduce 返回地址问题 

#### 问题

Java stream 单元素reduce处理返回地址为list唯一的元素的地址

#### 实例

上述问题在下例单列表汇总时出现

```java
List<VTChangeWithOperatorDO> dos = vtChangeWithOperatorMapper.selectVTChangeWithOperatorDataByRangeMonthDate(startDate, endDate, operator);

if(dos.size()==1) return dos;

//汇总
VTChangeWithOperatorDO sum = dos.stream().reduce((d1, d2) -> {
    VTChangeWithOperatorDO _sum = new VTChangeWithOperatorDO();
    _sum.setTrafficVolume(BigDecimalUtils.stringAddition(d1.getTrafficVolume(),d2.getTrafficVolume()));
    _sum.setVtBiggerNumber(BigDecimalUtils.stringAddition(d1.getVtBiggerNumber(),d2.getVtBiggerNumber()));
    _sum.setVtSmallerNumber(BigDecimalUtils.stringAddition(d1.getVtSmallerNumber(),d2.getVtSmallerNumber()));
    return _sum;
}).orElse(new VTChangeWithOperatorDO());
```

#### 分析

list.stream().reduce((d1, d2)->{})

因为reduce操作的是复数元素