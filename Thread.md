# Thread

#### 礼让线程

```java
//测试礼让线程
//礼让不一定成功，看cpu心情
public class YieldThread {
    public static void main(String[] args) {
        myYield myYield = new myYield();
        new Thread(myYield,"1").start();
        new Thread(myYield,"2").start();
        new Thread(myYield,"3").start();

    }
}
class myYield implements Runnable{
    @Override
    public void run() {
        System.out.println("线程"+Thread.currentThread().getName()+"开始执行");
        Thread.yield();//礼让线程
        System.out.println("线程"+Thread.currentThread().getName()+"停止执行");
    }
```

#### 插队线程

```java
  @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.println("插队--------线程"+i);
        }
    }

    public static void main(String[] args) throws InterruptedException {
        JoinThread joinThread = new JoinThread();
        Thread thread = new Thread(joinThread);
        thread.start();
        for (int i = 0; i < 500; i++) {
            if (i==200){
                thread.join();
            }
            System.out.println("main线程"+i);
        }
    }
```

#### 守护线程

```java

public class daemonThread {
    public static void main(String[] args) {
        mythread123 myth = new mythread123();
        daemonthread123 daemon = new daemonthread123();
        Thread thread2  = new Thread(daemon);
        thread2.setDaemon(true);
        //设置为守护线程。默认为fales
        thread2.start();
        new Thread(myth).start();
    }
}

class mythread123  implements Runnable{
    @Override
    public void run() {
        System.out.println("我是守护线程");
    }
}
class daemonthread123 implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 20; i++) {
            System.out.println("我是用户线程"+i);
        }
        System.out.println("用户线程结束");
    }
```

#### 线程优先级

```java
public class PriorityThread {
    public static void main(String[] args) {
        System.out.println("线程---》"+Thread.currentThread().getName()+"优先级----》"+Thread.currentThread().getPriority());
        myPriority mypriority = new myPriority();
        Thread thread1 = new Thread(mypriority);
        thread1.setPriority(10);
        thread1.start();
        Thread thread2 = new Thread(mypriority);
        thread2.setPriority(3);
        thread2.start();
        Thread thread3 = new Thread(mypriority);
        thread3.setPriority(5);
        thread3.start();
        Thread thread4 = new Thread(mypriority);
        thread4.setPriority(8);
        thread4.start();
        Thread thread5 = new Thread(mypriority);
        thread5.setPriority(1);
        thread5.start();
    }
}
class myPriority implements Runnable {
    @Override
    public void run() {
        System.out.println("线程---》"+Thread.currentThread().getName()+"
                           优先级----》"+Thread.currentThread().getPriority());
    }
}
```

#### 线程池

* 经常创建和销毁、使用量特别大的资源，比如并发情况下的线程，对性能影
* 提前创建好多个线程，放入线程池中，使用时直接获取， 使用完放回池中。
* 可以避免频繁创建销毁、实现重复利用。类似生活中的公共交通工具。
* 提高响应速度(减少了创建新线程的时间)
* 降低资源消耗(重复利用线程池中线程，不需要每次都创建)
* 便于线程管理
  --  rePoolSize: 核心池的大小
  --  maximumPoolSize:最大线程数
  --  keepAlive Time:线程没有任务时最多保持多长时间后会终止

```java
public class Threadpool {
    public static void main(String[] args) {
        //创建线程池                                             线程池大小(线程数)
        ExecutorService service = Executors .newFixedThreadPool( nThreads: 10) ;
        //执彷
        service. execute (new MyThread());
        service. execute (new MyThread());
        service. execute (new MyThread());
        service . execute (new MyThread()) ;
        //2.关闭链接
        service. shutdown() ;
    }
}
class MyThread impLements Runnable{
    @override
    public void run() {
        for(inti=0;i<100;i++){
            System.out.println(Thread . currentThread() .getName()+i) ;
        }
    }
```

### 产生死锁的四个必要条件: 

```
1.互斥条件:一个资源每次只能被一个进程使用。
2.请求与保持条件: 一个进程因请求资源而阻塞时，对已获得的资源保持不放。
3.不剥夺条件 :进程已获得的资源，在末使用完之前，不能强行剥夺。
4.循环等待条件:若干进程之间形成一种头尾相接的循环等待资源关系。
```

#### lock锁用法(明锁)

```java
private final Reentrantl ock lock = new ReenTrantLock();
try{
	lock.lock();//加锁
}catch{

}finally{
	lock.unlock();//解锁
}

```

#### synchronized锁用法(暗锁)

```java
synchronized{
	-- 方法
	-- 代码块
}
```

### 生产者消费者

##### 管程法

```
```



##### 信号灯法

```java
public class SignalLightsMethod {
    public static void main(String[] args) {
        Produc prod = new Produc();
        new player(prod).start();
        new watcher(prod).start();
    }
}
//生产
class player extends Thread{
    //生产品类构造方法
    Produc produc;
    public player(Produc pc){
        this.produc=pc;
    }

    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            if (i%2==0){
                this.produc.play("------------------>商品A");
            }
            this.produc.play("------------------>商品B");
        }

    }
}
//消费
class watcher extends Thread{
    //生产品类构造方法
    Produc produc;
    public watcher(Produc pc){
        this.produc=pc;
    }

    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            this.produc.watch();
        }
    }
}
//生产品
class Produc{
    String produc;  //商品名称
    boolean flag = true;   //生产消费状态

    //生产方法
    public synchronized void play(String p){
        if(!flag){
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        System.out.println("生产了"+p);
        //唤醒消费者消费
        this.notifyAll();
        //更新条件
        this.produc=p;
        this.flag=!this.flag;
    }
    //消费方法
    public synchronized void watch(){
        if(flag){
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        System.out.println("消费了<-----------------"+produc);
        //唤醒生产者生产
        this.notifyAll();
        //更新条件
        this.flag=!this.flag;
    }
}
```

