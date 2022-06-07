# java SE

java源文件后缀是 .java

经源文件编译后的二进制文件后缀是.Class

jar可以理解成一个可执行文件 类似exe 只不过需要java虚拟机执行 本质上是个压缩包，里面包含了运行这个程序所需要的文件和资源以及配置文件



## 第一个java程序HelloWord

源文件

```java
public class HelloWord{
	/*
	*@author sunny
	*@param args
	*@version 1.0	
	*/
	public static void main(String[] args){
		System.out.println("Holle Word");
	}
}
```

编译

```
HelloWord.java 
```

执行

```
java HelloWord 
```

生成帮助文档

```
C:\Users\74056\Desktop\物流需>javadoc -encoding UTF-8 -charset UTF-8 HelloWord.java                                   
正在加载源文件HelloWord.java...                                                                                        
正在构造 Javadoc 信息...                                                                                                
标准 Doclet 版本 1.8.0_102                                                                                              
正在构建所有程序包和类的树...                                                                                           
正在生成.\HelloWord.html...                                                                                             
正在生成.\package-frame.html...                                                                                        
正在生成.\package-summary.html...                                                                                       
正在生成.\package-tree.html...                                                                                          
正在生成.\constant-values.html...                                                                                       
正在构建所有程序包和类的索引...                                                                                        
正在生成.\overview-tree.html...                                                                                         
正在生成.\index-all.html...                                                                                             
正在生成.\deprecated-list.html...                                                                                       
正在构建所有类的索引...                                                                                                 
正在生成.\allclasses-frame.html...                                                                                      
正在生成.\allclasses-noframe.html...                                                                                    
正在生成.\index.html...                                                                                                 
正在生成.\help-doc.html...                                                                                                                 
```

## **JVM的基础概念**

>JVM的中文名称叫Java虚拟机，它是由软件技术模拟出计算机运行的一个虚拟的计算机。
>
>我们都知道Java的程序需要经过编译后，产生.Class文件，JVM才能识别并运行它，JVM针对每个操作系统开发其对应的解释器，所以只要其操作系统有对应版本的JVM，那么这份Java编译后的代码就能够运行起来，这就是Java能一次编译，到处运行的原因。

## **JVM的生命周期**

>JVM在Java程序开始执行的时候，它才运行，程序结束的时它就停止。
>
>一个Java程序会开启一个JVM进程，如果一台机器上运行三个程序，那么就会有三个运行中的JVM进程。
>
>JVM中的线程分为两种：守护线程和普通线程
>
>守护线程是JVM自己使用的线程，比如垃圾回收（GC）就是一个守护线程。
>
>普通线程一般是Java程序的线程，只要JVM中有普通线程在执行，那么JVM就不会停止。
>
>权限足够的话，可以调用exit()方法终止程序。

## **JVM的装入环境和配置**

>JDK是面向开发人员使用的SDK，它提供了Java的开发环境和运行环境，JDK中包含了JRE。
>
>JRE是Java的运行环境，是面向所有Java程序的使用者，包括开发者。





## 机器码(machine code)

```
机器码(machine code)，学名机器语言指令，有时也被称为原生码（Native Code），是电脑的CPU可直接解读的数据(计算机只认识0和1)。
```

## [字节码](https://so.csdn.net/so/search?q=字节码&spm=1001.2101.3001.7020)(byte code)

```
字节码的典型应用为Java bytecode。
字节码在运行时通过JVM（JAVA虚拟机）做一次转换生成机器指令，因此能够更好的跨平台运行。
字节码是一种中间状态（中间码）的二进制代码（文件）。需要直译器转译后才能成为机器码。
```

## **Class文件**

```
Class文件由Java编译器生成，我们创建的.Java文件在经过编译器后，会变成.Class的文件，这样才能被JVM所识别并运行。
```

## **类加载子系统**

类加载子系统也可以称之为类加载器，JVM默认提供三个类加载器：

**1、BootStrap ClassLoader** ：称之为启动类加载器，是最顶层的类加载器，**负责加载JDK中的核心类库，如 rt.jar、resources.jar、charsets.jar等**。

**2、Extension ClassLoader**：称之为扩展类加载器，负责加载Java的扩展类库，默认加载$JAVA_HOME中jre/lib/*.jar 或 -Djava.ext.dirs指定目录下的jar包。

**3、App ClassLoader**：称之为系统类加载器，负责加载应用程序classpath目录下所有jar和class文件。

除了Java默认提供的三个ClassLoader（加载器）之外，我们还可以根据自身需要自定义ClassLoader，自定义ClassLoader必须继承java.lang.ClassLoader 类。**除了BootStrap ClassLoader 之外**的另外两个默认加载器都是继承自java.lang.ClassLoader 。BootStrap ClassLoader 不是一个普通的Java类，它底层由C++编写，已嵌入到了JVM的内核当中，当JVM启动后，BootStrap ClassLoader 也随之启动，负责加载完核心类库后，并构造Extension ClassLoader 和App ClassLoader 类加载器。



类加载器子系统不仅仅负责定位并加载类文件，它还严格按照以下步骤做了很多事情：

```text
1、加载：寻找并导入Class文件的二进制信息
2、连接：进行验证、准备和解析
     1）验证：确保导入类型的正确性
     2）准备：为类型分配内存并初始化为默认值
     3）解析：将字符引用解析为直接引用
3、初始化：调用Java代码，初始化类变量为指定初始值
```

详细请参考另一篇文章：[Java类加载机制 - 知乎专栏](https://zhuanlan.zhihu.com/p/25228545)





## **方法区（Method Area）**

在JVM中，**类型信息**和**类静态变量**都保存在方法区中，类型信息是由类加载器在类加载的过程中从类文件中提取出来的信息。

需要注意的一点是，**常量池也存放于方法区中**。

程序中所有的线程共享一个方法区，所以访问方法区的信息必须确保线程是安全的。如果有两个线程同时去加载一个类，那么只能有一个线程被允许去加载这个类，另一个必须等待。

在程序运行时，方法区的大小是可以改变的，程序在运行时可以扩展。

方法区也可以被垃圾回收，但条件非常严苛，必须在该类没有任何引用的情况下，详情可以参考另一篇文章：[Java性能优化之JVM GC（垃圾回收机制） - 知乎专栏](https://zhuanlan.zhihu.com/p/25539690)

**类型信息包括什么？**

```text
1、类型的全名（The fully qualified name of the type）

2、类型的父类型全名（除非没有父类型，或者父类型是java.lang.Object）（The fully qualified name of the typeís direct superclass）

3、该类型是一个类还是接口（class or an interface）（Whether or not the type is a class ）

4、类型的修饰符（public，private，protected，static，final，volatile，transient等）（The typeís modifiers）

5、所有父接口全名的列表（An ordered list of the fully qualified names of any direct superinterfaces）

6、类型的字段信息（Field information）

7、类型的方法信息（Method information）

8、所有静态类变量（非常量）信息（All class (static) variables declared in the type, except constants）

9、一个指向类加载器的引用（A reference to class ClassLoader）

10、一个指向Class类的引用（A reference to class Class）

11、基本类型的常量池（The constant pool for the type）
```



**方法列表（Method Tables）**

为了更高效的访问所有保存在方法区中的数据，在方法区中，除了保存上边的这些类型信息之外，还有一个为了加快存取速度而设计的数据结构：方法列表。每一个被加载的非抽象类，Java虚拟机都会为他们产生一个方法列表，这个列表中保存了这个类可能调用的所有实例方法的引用，保存那些父类中调用的方法。





## **Java堆（JVM堆**、**Heap）**

当Java**创建一个类的实例对象或者数组时，都在堆中为新的对象分配内存**。

虚拟机中只有一个堆，程序中所有的线程都共享它。

堆占用的内存空间是最多的。

堆的存取类型为管道类型，先进先出。

在程序运行中，可以动态的分配堆的内存大小。

堆的内存资源回收是交给JVM GC进行管理的，详情请参考：[Java性能优化之JVM GC（垃圾回收机制） - 知乎专栏](https://zhuanlan.zhihu.com/p/25539690)





## **Java栈（JVM栈、Stack）**

在Java栈中**只保存基础数据类型**（**参考：**[Java 基本数据类型 - 四类八种 - 知乎专栏](https://zhuanlan.zhihu.com/p/25439066)）和自定义对象的**引用**，**注意只是对象的引用而不是对象本身哦**，对象是保存在堆区中的。



**拓展知识：像String、Integer、Byte、Short、Long、Character、Boolean这六个属于包装类型，它们是存放于堆中的。**

栈的存取类型为类似于水杯，先进后出。

栈内的数据在超出其作用域后，会被自动释放掉，**它不由JVM GC管理**。

每一个线程都包含一个栈区，每个栈中的数据都是私有的，其他栈不能访问。

每个线程都会建立一个操作栈，每个栈又包含了若干个栈帧，每个栈帧对应着每个方法的每次调用，每个栈帧包含了三部分：

局部变量区（方法内基本类型变量、变量对象指针）

操作数栈区（存放方法执行过程中产生的中间结果）

运行环境区（动态连接、正确的方法返回相关信息、异常捕捉）





## **本地方法栈**

本地方法栈的功能和JVM栈非常类似，用于存储本地方法的局部变量表，本地方法的操作数栈等信息。

栈的存取类型为类似于水杯，先进后出。

栈内的数据在超出其作用域后，会被自动释放掉，**它不由JVM GC管理。**

每一个线程都包含一个栈区，每个栈中的数据都是私有的，其他栈不能访问。

本地方法栈是在程序调用或JVM调用**本地方法接口（Native）**时候启用。

本地方法都不是使用Java语言编写的，比如使用C语言编写的本地方法，本地方法也不由JVM去运行，所以本地方法的运行不受JVM管理。

HotSpot VM将本地方法栈和JVM栈合并了。





## **程序计数器**

在JVM的概念模型里，字节码解释器工作时就是通过改变这个计数器的值来选取下一条需要执行的字节码指令。分支、循环、跳转、异常处理、线程恢复等基础功能都需要依赖这个计数器来完成。

JVM的多线程是通过线程轮流切换并分配处理器执行时间的方式来实现的，为了各条线程之间的切换后计数器能恢复到正确的执行位置，所以**每条线程都会有一个独立的程序计数器**。

程序计数器仅占很小的一块内存空间。

当线程正在执行一个Java方法，程序计数器记录的是正在执行的JVM字节码指令的地址。如果正在执行的是一个Natvie（本地方法），那么这个计数器的值则为空（Underfined）。

程序计数器这个内存区域是唯一一个在JVM规范中没有规定任何OutOfMemoryError（内存不足错误）的区域。





## **JVM执行引擎**

Java虚拟机相当于一台虚拟的“物理机”，这两种机器都有代码执行能力，其区别主要是物理机的执行引擎是直接建立在处理器、硬件、指令集和操作系统层面上的。而JVM的执行引擎是自己实现的，因此程序员可以自行制定指令集和执行引擎的结构体系，因此能够执行那些不被硬件直接支持的指令集格式。

在JVM规范中制定了虚拟机字节码执行引擎的概念模型，这个模型称之为JVM执行引擎的统一外观。JVM实现中，可能会有两种的执行方式：解释执行（通过解释器执行）和编译执行（通过即时编译器产生本地代码）。有些虚拟机只采用一种执行方式，有些则可能同时采用两种，甚至有可能包含几个不同级别的编译器执行引擎。

**输入的是字节码文件、处理过程是等效字节码解析过程、输出的是执行结果**。在这三点上每个JVM执行引擎都是一致的。





## **本地方法接口（JNI）**

JNI是Java Native Interface的缩写，它提供了若干的API实现了Java和其他语言的通信（主要是C和C++）。

**JNI的适用场景**

当我们有一些旧的库，已经使用C语言编写好了，如果要移植到Java上来，非常浪费时间，而JNI可以支持Java程序与C语言编写的库进行交互，这样就不必要进行移植了。或者是与硬件、操作系统进行交互、提高程序的性能等，都可以使用JNI。需要注意的一点是需要保证本地代码能工作在任何Java虚拟机环境。

**JNI的副作用**

一旦使用JNI，Java程序将丢失了Java平台的两个优点：

1、程序不再跨平台，要想跨平台，必须在不同的系统环境下程序编译配置本地语言部分。

2、程序不再是绝对安全的，本地代码的使用不当可能会导致整个程序崩溃。一个通用规则是，调用本地方法应该集中在少数的几个类当中，这样就降低了Java和其他语言之间的耦合。





## **JVM GC（垃圾回收机制）**

详情请参考我的另外一篇文章：[Java性能优化之JVM GC（垃圾回收机制） - 知乎专栏](https://zhuanlan.zhihu.com/p/25539690)





## **JVM 常量池**

JVM常量池也称之为运行时常量池，**它是方法区（Method Area）的一部分**。**用于存放编译期间生成的各种字面量和符号引用。运行时常量池不要求一定只有在编译器产生的才能进入，运行期间也可以将新的常量放入池中，这种特性被开发人员利用比较多的就是String.intern()方法。**

由**“****用于存放编译期间生成的各种字面量和符号引用”**这句话可见，**常量池中存储的是对象的引用而不是对象的本身**。



**常量池的好处**

常量池是为了避免频繁的创建和销毁对象而影响系统性能，它也实现了对象的共享。

例如字符串常量池：在编译阶段就把所有字符串文字放到一个常量池中。

1、节省内存空间：常量池中如果有对应的字符串，那么则返回该对象的引用，从而不必再次创建一个新对象。

2、节省运行时间：比较字符串时，==比equals()快。对于两个引用变量，==判断引用是否相等，也就可以判断实际值是否相等。



**双等号（==）的含义**

基本数据类型之间使用双等号，比较的是数值。

复合数据类型（类）之间使用双等号，比较的是对象的引用地址是否相等。

### Exception和Error

IllegalArgumentException不合法的参数异常







## =======================================《《常用》》======================================

### 数组的使用

```java
//定义方式1  
dataType [] a= new dataType [length];
//定义方式2
dataType b[]={value0, value1, ..., valuek};

// 打印所有数组元素For循环
for (int i = 0; i < a.length; i++) {
    System.out.println(myList[i] + " ");
}
// 打印所有数组元素For循环
for(type element: a)
{
    System.out.println(element);
}

// 计算所有元素的总和
double total = 0;
for (int i = 0; i < a.length; i++) {
    total += myList[i];
}
System.out.println("Total is " + total);

// 查找最大元素
double max = myList[0];
for (int i = 1; i < a.length; i++) {
    if (myList[i] > max) max = myList[i];
}
System.out.println("Max is " + max);
 


```

### 二维数组

```java
//定义二维数组
type[][] a typeName = new type[typeLength1][typeLength2];
//静态初始化（直接后面大括号赋值）
int [][]scores;//定义一个int类型的二维数组
scores = new int[][]{undefined{1,2,3},{4,5},{6,7}};
//动态初始化：
int [][]i = new int[3][4];//动态初始化方式一
int [][]i= new int[3][];//动态初始化方式二
i[0] = new int[4];
i[1] = new int[4];
i[2] = new int[4];


//初始化二维数组
int [][]i= new int[3][];
i[0] = new int[]{1,2,3};
i[1] = new int[]{4,5,6};
i[2] = new int[]{7,8,9};
//二维数组的总长度（行的长度）
System.out.println(i.length);
//数组中第一个数组值的长度（列的长度）
System.out.println(i[0].length);
//二维数组的遍历
for (int j = 0; j <i.length ; j++) {
    for (int k = 0; k < i[j].length; k++) {
        System.out.println(i[j][k]);
    }
}
```

### 8种基本数据类型

#### 基本数据类型分为三类：

1. 数值型：数值型又分为整数型和浮点型；
2. 字符型（char)
3. 布尔型（boolean)

#### 为什么会有基本数据类型？

因为，在java中`new`一个对象是存储在堆里的，对于我们经常操作的数据类型，每次创建对象这样太消耗资源，因此java提供了8个基本数据类型，存储在栈里。用起来更方便。

计算机中都是使用**二进制的补码**进行运算的，首先，`1字节=8bit`，那么8bit可以表示的数字范围是[-128,127]。注意首位的1表示负数，0表示正数。

> 1000 0000：-128或-2^7-1
>
> 0111 1111:127或2^7-1

| 类型    | 占用存储空间 | 表数范围               | 默认值          |
| ------- | ------------ | ---------------------- | --------------- |
| byte    | 1字节        | -128 ~ 127             | 0               |
| short   | 2字节        | -2^15 ~ 2^15 -1        | 0               |
| int     | 4字节        | -2^31 ~ 2^31 -1        | 0               |
| long    | 8字节        | -2^63 ~ 2^63 -1        | 0，或者0L或者0l |
| float   | 4字节        | -3.403E38 ~ 3.403E38   | 0.0F或0.0f      |
| double  | 8字节        | -1.798E308 ~ 1.798E308 | 0.0D或0.0d      |
| char    | 2字节        | 0~2^16 -1              | '\u0000'        |
| boolean | 1字节        | true/false             | false           |

### 包装类

基本数据类型，使用起来非常方便，但是没有对应的方法来操作这些基本类型的数据，可以使用一个类，把基本数据类型的数据装起来，这个类叫做包装类（wrapper）。这样我们可以调用类中的方法。

开发中，用的最多的是字符串变为基本数据类型。

8种基本数据类型对应包装类：可以看到除了int和char类型，其它都是首字母大写。

| 基本数据类型 |    包装类     |
| :----------: | :-----------: |
|     byte     |     Byte      |
|    short     |     Short     |
|   **int**    |  **Integer**  |
|     long     |     Long      |
|    float     |     Float     |
|    double    |    Double     |
|   **char**   | **Character** |
|   boolean    |    Boolean    |



