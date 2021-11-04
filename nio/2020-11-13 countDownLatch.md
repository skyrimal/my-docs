#CountDownLatch



A synchronization aid that allows one or more threads to wait until a set of operations being performed in other threads completes.

一种允许一个或多个线程等待，直到在其他线程中执行的一组操作完成的同步帮助。

```java
public class CountDownLatchDemo {
    public static void main(String[] args) throws InterruptedException {
        CountDownLatch countDownLatch = new CountDownLatch(6);
        for (int i = 0; i < 6; i++) {
        new Thread((()->{
            System.out.println(Thread.currentThread().getName()+":\t离开教室");
            countDownLatch.countDown();
        }), "第"+i+"个同学").start();
    }
    countDownLatch.await();//计数完后会自动唤醒
    System.out.println("关门打狗");
}

private static void close() {
    for (int i = 0; i < 6; i++) {
        new Thread((()->{
            System.out.println(Thread.currentThread().getName()+":\t离开教室");
        }), "第"+i+"个同学").start();
    }
    System.out.println("关门打狗");
}
```
}

输出

第0个同学:	离开教室
第5个同学:	离开教室
第3个同学:	离开教室
第4个同学:	离开教室
第2个同学:	离开教室
第1个同学:	离开教室
关门打狗