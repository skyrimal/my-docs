# SemaphoreDemo

制定一个限定容积的满容器队列，当使用时释放或添加容器

当容器空了时，则锁住线程

```java
public class SemaphoreDemo {
    public static void main(String[] args) {
        Semaphore semaphore = new Semaphore(3);//模拟资源类，资源有3个
        for (int i = 0; i < 6; i++) {
            new Thread((()->{
                try {
                    semaphore.acquire();
                    System.out.println(Thread.currentThread().getName()+"\t获取了车位");
                }catch (Exception e){
                    e.printStackTrace();
                }finally {
                    System.out.println(Thread.currentThread().getName()+"\t交停车费走了");
                    semaphore.release();
                }
            }),"第"+i+"辆车").start();
        }
    }
}
```

输出

第0辆车	获取了车位
第2辆车	获取了车位
第2辆车	交停车费走了
第1辆车	获取了车位
第1辆车	交停车费走了
第0辆车	交停车费走了
第4辆车	获取了车位
第4辆车	交停车费走了
第5辆车	获取了车位
第3辆车	获取了车位
第3辆车	交停车费走了
第5辆车	交停车费走了