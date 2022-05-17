# 线程池概述
一种线程使用模式，线程过多会带来调度开销，进而影响缓存局部性和整体性能。而线程池维护着多个线程，等待监督管理者分配可并发执行的任务。避免了在处理短时间任务时创建与销毁线程的代价。  
控制运行的线程数量，如果线程数量超过了最大数量，超出数量的线程排队等候，等其他线程执行完毕后，再从队列中取出任务来执行。  
降低资源消耗，提高响应速度，提高线程的可管理性  

#线程池的使用方式
1. 一池N线程

```java
public class ThreadPoolDemo1 {
    public static void main(String[] args) {
        //一池五线程
        ExecutorService threadPool1 = Executors.newFixedThreadPool(5);
        //10个任务
        try {
            for (int i = 1; i <= 10; i++) {
                threadPool1.execute(() -> {
                    System.out.println(Thread.currentThread().getName + "办理业务");
                });
            }
            
        } finally {
            threadPool1.shutdown();
        }
    }
}
```
2. 一个任务一个任务执行，一池一线程
```java
public class ThreadPoolDemo1 {
    public static void main(String[] args) {
        //一池一线程
        ExecutorService threadPool2 = Executors.newSingleThreadExecutor();
        //10个任务
        try {
            for (int i = 1; i <= 10; i++) {
                threadPool2.execute(() -> {
                    System.out.println(Thread.currentThread().getName + "办理业务");
                });
            }
            
        } finally {
            threadPool2.shutdown();
        }
    }
}
```

3. 线程池根据需求创建线程，可扩容
```java
public class ThreadPoolDemo1 {
    public static void main(String[] args) {
        //可扩容线程
        ExecutorService threadPool3 = Executors.newCachedThreadPool();
        //10个任务
        try {
            for (int i = 1; i <= 10; i++) {
                threadPool3.execute(() -> {
                    System.out.println(Thread.currentThread().getName + "办理业务");
                });
            }
            
        } finally {
            threadPool3.shutdown();
        }
    }
}
```

## 三种方法底层都是创建ThreadPoolExecutor


# ThreadPoolExecutor
+ int corePoolSize:常驻线程数量
+ int maximumPoolSize:最大线程数量
+ long keepAliveTime:线程存活时间
+ TimeUnit unit:线程存活时间
+ BlockingQueue<Runnable> workQueue:阻塞队列
+ ThreadFactory threadFactory:线程工厂
+ RejectedExecutionHandler handler:拒绝策略


#自定义线程池
```java
public class ThreadPoolDemo2{
    public static void main(String[] args) {
        ExecutoService threadPool = new ThreadPoolExecutor(
                2,
                5,
                2L,
                TimeUnit.SECONDS,
                new ArrayBlockingQueue<>(3),
                Executors.defaultThreadFactory(),
                new ThreadPoolExecutor.AbortPolicy()
                
        );
        try {
            for (int i = 1; i <= 10; i++) {
                threadPool.execute(() -> {
                    System.out.println(Thread.currentThread().getName + "办理业务");
                });
            }

        } finally {
            threadPool.shutdown();
        }
    }
}
```


