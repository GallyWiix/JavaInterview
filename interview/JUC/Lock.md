# synchronized关键字
 

# 多线程编程步骤
1. 创建资源类，在资源类创建属性和操作方法
2. 创建多个线程，调用资源类的操作方法

```java

//1.创建资源类，定义属性和操作方法
class Ticket {
    private int number = 30;

    private synchronized void sell() {
        //判断是否有余票
        if (number > 0) {
            System.out.println(Thread.currentThread().getName() + ":卖出:" + (number--) + "剩下:" + number);
        }
    }
}

public class SellTickets {
    //2.创建多个线程
    public static void main(String[] args) {
        //创建ticket对象
        Ticket ticket = new Ticket();
        
        new Thread(new Runnable(){
            @Override 
            public void run(){
                //调用卖票方法
                for (int i = 0; i < 40; i++) {
                    ticket.sell();
                }
            }
        },"aa").start();

        new Thread(new Runnable(){
            @Override
            public void run(){
                //调用卖票方法
                for (int i = 0; i < 40; i++) {
                    ticket.sell();
                }
            }
        },"bb").start();

        new Thread(new Runnable(){
            @Override
            public void run(){
                //调用卖票方法
                for (int i = 0; i < 40; i++) {
                    ticket.sell();
                }
            }
        },"cc").start();
    }
}

```

#Lock
lock锁实现提供了比使用同步方法和语句可以获得得更广泛的锁操作。它允许更灵活的结构，可能具有非常不同的属性，并且可能支持多个关联的条件对象。lock提供了比synchronized更多的功能

# lock与synchronized的区别

```java
//创建资源类，定义属性和操作方法
class LTicket {
    private int number = 30;
    private final ReentrantLock lock = new RenntrabtLock();

    public void sell() {
        lock.lock();
        try {
            if (number > 0) {
                System.out.println(Thread.currentThread().getName() + ":卖出:" + (number--) + "剩下:" + number);
            }
        } catch (java.lang.Exception e) {
            e.printStackTrace();
        }finally {
            lock.unlock();
        }
        
    }
}

public class LSellTicket {
    
    

    public static void main(String[] args) {
        LTicket ticket = new LTicket();
        
        new Thread(()->{
            for (int i = 0; i < 40; i++) {
                ticket.sell();
            }
        },"aa").start();

        new Thread(()->{
            for (int i = 0; i < 40; i++) {
                ticket.sell();
            }
        },"bb").start();

        new Thread(()->{
            for (int i = 0; i < 40; i++) {
                ticket.sell();
            }
        },"cc").start();
    }
    

}
```
1.Lock是一个接口，而synchronized是java中的关键字，synchronized是内置的语言实现；
2.synchronized在发生异常时，会自动释放线程占有的锁，因此不会导致死锁现象发生；而lock在发生异常时，如果没有主动通过unlock（）去释放锁