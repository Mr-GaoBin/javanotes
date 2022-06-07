# Java 23种设计模式

## 目录

#### [1.1创建型模式-------->]

###### 1.1.1 工厂方法

###### 1.1.2 抽象工厂

###### 1.1.3 建造者模式	

###### 1.1.4 单态模式	

单例模式（Singleton Pattern）是 Java 中最简单的设计模式之一。这种类型的设计模式属于创建型模式，它提供了一种创建对象的最佳方式。

这种模式涉及到一个单一的类，该类负责创建自己的对象，同时确保只有单个对象被创建。这个类提供了一种访问其唯一的对象的方式，可以直接访问，不需要实例化该类的对象。

1、单例类只能有一个实例。

2、单例类必须自己创建自己的唯一实例。

3、单例类必须给所有其他对象提供这一实例。

>饿汉就是类一旦加载，就把单例初始化完成，保证getInstance的时候，单例是已经存在的了。
>
>而懒汉比较懒，只有当调用getInstance的时候，才回去初始化这个单例。



```java
DCL饿汉式
//饿汉式单例     构造器私有
pub1ic class Hungry {
	private Hungry(){
}
private final static Hungry HUNGRY = new Hungry();
    public static Hungry getInstance(){
    return HUNGRY ;
}

DCL懒汉式
private volatile static LazyMan LazyMan;
//双重检测锁模式的懒汉式单例DCL懒汉式
public static LazyMan getInstance(){
    if (LazyMan==null){
    	synchronized (LazyMan. class){
        if (LazyMan==nu1l){
        LazyMan = new LazyMan(); //不是一个原子性操作
        }
    }
    return LazyMan;
}
    /* 1.分配内存空间
       2、执行构造方法，初始化对象.
       3、把这个对象指向这个空间*/
//反射
public static void main(String[] args) throws Exception {
    LazyMan instance = LazyMan.getInstance();
    Constructor<LazyMan> declaredConstructor = LazyMan.class.getDeclaredConstructor(null);
    declaredConstructor.setAccessible(true);//无视构造器私有
    LazyMan instance2 = declaredConstructor.newInstance();
}
   
```

破坏私有关键字

```apl
method.setAccessible(true)
此对象的 accessible 标志设置为指示的布尔值。值为 true 则指示反射的对象在使用时应该取消 Java 语言访问检查。值为 false 则指示反射的对象应该实施 Java 语言访问检查;实际上setAccessible是启用和禁用访问安全检查的开关,并不是为true就能访问为false就不能访问 ；

由于JDK的安全检查耗时较多.所以通过setAccessible(true)的方式关闭安全检查就可以达到提升反射速度的目的 
```





###### 1.1.5 原型模式	

#### [1.2 结构型模式-------->]

###### 1.2.1 适配器模式	

###### 1.2.2 桥接模式	

###### 1.2.3 组合模式	

###### 1.2.4 装饰模式	

###### 1.2.5 外观模式	

###### 1.2.6 享元模式	

###### 1.2.7 代理模式	

#### [1.3 行为型模式-------->]

###### 1.3.1 责任链模式	

###### 1.3.2 命令模式	

###### 1.3.3 解释器模式	

###### 1.3.4 迭代器模式	

###### 1.3.5 中介者模式	

###### 1.3.6 备忘录模式	

###### 1.3.7 观察者模式	

###### 1.3.8 状态模式	

###### 1.3.9 策略模式	

###### 1.3.10 模板方法	

###### 1.3.11 访问者模式	