# 公平锁与非公平锁  
+ 公平锁效率高，线程饿死
+ 非公平锁阳光普照，效率低  

# 可重入锁
synchronized和lock都是可重入锁  

#死锁
两个或两个以上的进程在执行过程中，因为争夺资源而造成互相等待的现象，如果没有外力干涉，他们无法再执行下去

1. 系统资源不足
2. 进程运行推进顺序不合适
3. 资源分配不当

```java
public class DeadLock{
    static Object a = new Object();
    static Object b = new Object();

    public static void main(String[] args) {
        new Thread(()->{
            synchronized (a){
                System.out.println(Thread.currentThread().getName()+"持有锁a，试图获取锁b");
                TimeUnit.SECONDS.sleep(1);
                synchronized (b){
                    System.out.println(Thread.currentThread().getName()+"获取锁b");
                }
            }
        },"a").start();

        new Thread(()->{
            synchronized (b){
                System.out.println(Thread.currentThread().getName()+"持有锁b，试图获取锁a");
                TimeUnit.SECONDS.sleep(1);
                synchronized (a){
                    System.out.println(Thread.currentThread().getName()+"获取锁a");
                }
            }
        },"b").start();
    }
}
```