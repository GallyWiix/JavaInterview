# 线程间的通信  


## synchronized关键字实现

```java
class Share {
    //添加初始值
    private int number = 0;

    public synchronized void incr() throws InterruptedException {
        //2.判断
        while (number != 0) {
            this.wait();

        }
        //如果number为0
        number++;
        System.out.println(Thread.currentThread().getName() + "::" + number);
        //通知其他线程
        this.notifyAll();
    }

    public synchronized void decr() {
        while (number != 1) {
            this.wait(); 
        }
        number--;
        this.notifyAll();
    }
}

public class ThreadDemo1 {
    //3.创建多个线程
    public static void main(String[] args) {
        Share share = new Share();
        new Thread(() -> {
            for (int i = 1; i <= 10; i++) {
                try {
                    share.incr();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }

            }
        },"aa").start();

        new Thread(() -> {
            for (int i = 1; i <= 10; i++) {
                try {
                    share.decr();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }

            }
        },"bb").start();
    }
}
```

## Lock实现

```java
class Share{
    private int number = 0;
    private Lock lock = new ReentrantLock();
    
    private Condition condition = lock.newCondition();
    
    public void incr(){
        lock.lock();
        
        try{
            while(number != 0){
                condition.await();
            }
            number ++;
            System.out.println(Thread.currentThread().getName() + "::" + number);
            condition.signalAll();
        }finally {
            lock.unlock();
        }
    }

    public void decr(){
        lock.lock();

        try{
            while(number != 1){
                condition.await();
            }
            number --;
            System.out.println(Thread.currentThread().getName() + "::" + number);
            condition.signalAll();
        }finally {
            lock.unlock();
        }
    }
    
    
}
public class ThreadDemo2{
    public static void main(String[] args) {
        Share share = new Share();
        
        new Thread(()->{
            for (int i = 1; i <= 10 ; i++) {
                try{
                    share.incr();
                }catch (InterruptedException e){
                    e.printStackTrace();
                }
                
            }
        },"aa").start();

        new Thread(()->{
            for (int i = 1; i <= 10 ; i++) {
                try{
                    share.decr();
                }catch (InterruptedException e){
                    e.printStackTrace();
                }

            }
        },"bb").start();
    }
}
```
# 定制化通信

```java
//1.创建资源类
class ShareResource {
    //定义标志位
    private int flag = 1; // 1 aa 2 bb 3 cc
    //创建lock锁
    private Lock lock = new ReentrantLock();
    //创建三个condition
    private Condition c1 = locak.newCondition();
    private Condition c2 = locak.newCondition();
    private Condition c3 = locak.newCondition();

    //打印5次，参数第几轮
    public void print5(int loop) {
        //上锁
        lock.lock();
        try {
            //判断
            while (flag != 1) {
                //等待
                c1.await();
            }
            for (int i = 1; i <= 5; i++) {
                System.out.println(Thread.currentThread().getName() + "::" + i + ":轮" + loop);
            }
            flag = 2;
            c2.signal();
        } finally {
            lock.unlock();
        }
    }

    //打印10次，参数第几轮
    public void print10(int loop) {
        //上锁
        lock.lock();
        try {
            //判断
            while (flag != 2) {
                //等待
                c2.await();
            }
            for (int i = 1; i <= 10; i++) {
                System.out.println(Thread.currentThread().getName() + "::" + i + ":轮" + loop);
            }
            flag = 3;
            c3.signal();
        } finally {
            lock.unlock();
        }
    }

    //打印15次，参数第几轮
    public void print15(int loop) {
        //上锁
        lock.lock();
        try {
            //判断
            while (flag != 3) {
                //等待
                c3.await();
            }
            for (int i = 1; i <= 15; i++) {
                System.out.println(Thread.currentThread().getName() + "::" + i + ":轮" + loop);
            }
            flag = 1;
            c1.signal();
        } finally {
            lock.unlock();
        }
    }
}

public class ThreadDemo3 {
    public static void main(String[] args) {
        ShareResource shareResource = new ShareResource();
        new Thread(() -> {
            for (int i = 1; i <= 10; i++) {
                try {
                    shareResource.print5(i);
                } catch (java.lang.Exception e) {
                    e.printStackTrace();
                }
            }
        },"aa").start();

        new Thread(() -> {
            for (int i = 1; i <= 10; i++) {
                try {
                    shareResource.print10(i);
                } catch (java.lang.Exception e) {
                    e.printStackTrace();
                }
            }
        },"bb").start();

        new Thread(() -> {
            for (int i = 1; i <= 10; i++) {
                try {
                    shareResource.print15(i);
                } catch (java.lang.Exception e) {
                    e.printStackTrace();
                }
            }
        },"cc").start();
    }
}
```

# synchronized实现同步的基础
java中的每一个对象都可以作为锁。具体表现为：
+ 对于普通同步方法，锁是当前实例对象
+ 杜宇静态同步方法，锁是当前类的Class对象
+ 对于同步方法块，锁是synchronized括号里配置的对象  


