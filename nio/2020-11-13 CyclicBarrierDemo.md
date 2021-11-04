# CyclicBarrierDemo

给线程定下个数，当一定个数的都执行到指定位置后才会再进行执行

##实例代码

```java
public class CyclicBarrierDemo {
    public static void main(String[] args) {
        CyclicBarrier cyclicBarrier = new CyclicBarrier(7,() -> System.out.println("比克复活了"));
        for (int i = 1; i < 8; i++) {
            final int _temp = i;
            new Thread((()->{
                System.out.println(Thread.currentThread().getName()+"\t搜集："+_temp+"星龙珠");
                try {
                    cyclicBarrier.await();
                }catch (Exception e){
                    e.printStackTrace();
                }finally {
                }
                System.out.println(Thread.currentThread().getName()+"\t丢了："+_temp+"星龙珠");
            }), "杂兵"+i).start();
        }
    }
}
```

输出

杂兵1	搜集：1星龙珠
杂兵5	搜集：5星龙珠
杂兵4	搜集：4星龙珠
杂兵3	搜集：3星龙珠
杂兵2	搜集：2星龙珠
杂兵7	搜集：7星龙珠
杂兵6	搜集：6星龙珠
比克复活了
杂兵4	丢了：4星龙珠
杂兵5	丢了：5星龙珠
杂兵1	丢了：1星龙珠
杂兵6	丢了：6星龙珠
杂兵7	丢了：7星龙珠
杂兵2	丢了：2星龙珠
杂兵3	丢了：3星龙珠
