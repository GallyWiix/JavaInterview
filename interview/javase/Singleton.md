# 1.什么是单例模式
#### Singleton：在Java中即指单例设计模式，在软件开发汇总最常用的设计模式之一
#### 单：唯一  例:实例
#### 单例设计模式，即某个类在整个系统中只能有一个实例对象可被获取和使用的代码模式
# 2.要点
##### 某个类只能有一个实例（构造器私有化）
##### 他必须自行创建实例
##### 他必须自行向整个系统提供这个实例
## 饿汉式
```java
public class Singleton1{
    /**
     * 直接实例化饿汉式
     * 1.构造器私有化
     * 2.自行创建
     * 3.向外提供实例
     * 4.使用final，强调这是一个单例
     */
    public static final Singleton1 INSTANCE = new Singleton1();
    
    private Singleton1(){
        
    }
}
```
```java
public class Singleton2{
    /**
     * 静态代码块饿汉式
     */
    public static final Singleton2 INSTANCE;
    
    static {
        INSTANCE = new Singleton2();
        
    }
    private Singleton2(){
        
    }
}
```
```java
public enum Singleton3{
    /**
     * 枚举式
     */
    INSTANCE
}
```
## 懒汉式
```java
public class Singleton4{
    /**
     * 线程不安全
     * 1.构造器私有化
     * 2.用一个静态变量保存这一个为一个实例
     * 3.提供一个静态方法，获取这个实例对象
     */
    public static Singleton4 instance;
    
    private Singleton4(){
        
    }
    public static Singleton4 getInstance(){
        if(instance == null){
            instance = new Singleton4();
            
        }
        return instance;
    }
}
```
