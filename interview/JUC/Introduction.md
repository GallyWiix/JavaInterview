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

#并发的三大特性
+ 原子性:指一个操作中cpu不可以一再中途暂停然后再调度，即不被中断，要不全部执行完成，要不都不执行
+ 可见性：当多个线程访问一个变量时，一个线程修改了这个变量的值，其他线程能够立即看到修改的值
+ 有序性：虚拟机在进行代码编译时，对于哪些改变顺序之后不会对最终结果造成影响的代码，虚拟机不一定会按照我们写的代码顺序来执行，有可能将他们重新排序