# JUC简介
java.util.concurrent工具包的简称。是一个处理线程的工具包，JDK1.5开始出现

#进程与线程
+ 进程：系统中正在运行的一个应用程序；程序一旦运行就是进程；进程--资源分配的最小单位
+ 线程：系统分配处理器时间资源的基本单元，或者说进程之内独立执行的一个单元执行流。是程序执行的最小单位

# 线程的状态
+ NEW
+ RUNNABLE
+ BLOCKED
+ WAITING
+ TIMED_WAITING
+ TERMINATED(终结)

# 并发与并行
并发：同一时刻多个线程访问同一个资源
并行：多项工作一起执行，之后再汇总

#管程
Monitor监视器，是一种同步机制，保证同一时间，只有一个线程能去访问被保护的数据或代码  

#用户线程和守护线程
+ 用户线程：自定义线程
+ 守护线程：运行后台，比如垃圾回收
```java
public class Main{
    public static void main(String[] args) {
        Thread aa = new Thread(()->{
            System.out.println(Thread.currentThread().getName() +"::" +Thread.currentThread().isDaemon());
            while (true){
                
            }
        },"aa");
        //设置守护线程
        aa.setDaemon(true);
        aa.start();

        System.out.println(Thread.currentThread().getName()+"over");
    }
}
```