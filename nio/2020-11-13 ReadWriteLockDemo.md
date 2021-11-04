# ReadWriteLockDemo

读写锁

多线程可以共享同步读取的资源

但是当有写入操作时，则锁住资源防止多次写入

```java
class MyCache {
    private volatile Map<String, Object> map = new HashMap<>();
    private ReadWriteLock readWriteLock = new ReentrantReadWriteLock();

    public void put(String key, Object value)  {
        System.out.println(Thread.currentThread().getName()+"\t写入开始");
        readWriteLock.writeLock().lock();
        try {
            //模拟延时
            try {
                TimeUnit.SECONDS.sleep(1);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            map.put(key, value);
            System.out.println(Thread.currentThread().getName()+"\t写入成功");
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            readWriteLock.writeLock().unlock();
        }

    }

    public void get(String key)  {
        System.out.println(Thread.currentThread().getName()+"\t读取开始");
        readWriteLock.readLock().lock();//读取锁在没有写入时不会锁住
        try {
            try {
                TimeUnit.SECONDS.sleep(1);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            Object result = map.get(key);
            System.out.println(Thread.currentThread().getName()+"\t读取成功"+ result.toString());
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            readWriteLock.readLock().unlock();
        }
    }
}

/**
 * @Description: TODO
 * @author: skyrimal
 * @date: 2020年11月13日 15:34
 */
public class ReadWriteLockDemo {
    public static void main(String[] args) {
        MyCache myCache = new MyCache();

        for (int i = 0; i < 5; i++) {
            final int finalI = i;
            new Thread((()->{
                myCache.put("第"+ finalI +"个", "黄瓜");
            }), "第"+finalI+"个写入线程").start();
        }
        for (int i = 0; i < 5; i++) {
            final int finalI = i;
            new Thread((()->{
                myCache.get("第"+ finalI +"个");
            }), "第"+finalI+"个读取线程").start();
        }
    }
}
```