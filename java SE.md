# java SE

## java的优势和特性

>简单性
>
>面向对象
>
>可移植性
>
>高性能
>
>分布式
>
>动态性
>
>多线程
>
>安全性(强类型语言)
>
>健壮性

* java源文件后缀是 .java

* 经源文件编译后的二进制文件后缀是.Class

* jar可以理解成一个可执行文件 类似exe 只不过需要java虚拟机执行 本质上是个压缩包，里面包含了运行这个程序所需要的文件和资源以及配置文件

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

```apl
C:\Users\74056\Desktop\物流需>javadoc -encoding UTF-8 -charset UTF-8 HelloWord.java                                   
正在加载源文件HelloWord.java...                                                                                   
正在构造 Javadoc 信息...                                                                                           
标准 Doclet 版本 1.8.0_102                                                                                       
正在构建所有程序包和类的树...                                                                                       正在生成.\HelloWord.html...                                                                                       正在生成.\package-frame.html...                                                                                   正在生成.\package-summary.html...                                                                                 正在生成.\package-tree.html...                                                                                   正在生成.\constant-values.html...                                                                                 正在构建所有程序包和类的索引...                                                                                     正在生成.\overview-tree.html...                                                                                 正在生成.\index-all.html...                                                                                       正在生成.\deprecated-list.html...                                                                                 正在构建所有类的索引...                                                                                             正在生成.\allclasses-frame.html...                                                                               正在生成.\allclasses-noframe.html...                                                                             正在生成.\index.html...                                                                                           
正在生成.\help-doc.html... 
```



## 数组的使用

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

#### 二维数组

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

## 8种基本数据类型

#### 基本数据类型分为三类：

1. 数值型：数值型又分为整数型和浮点型（byte,short,int,long,float,double）；
2. 字符型（char)
3. 布尔型（boolean)

#### 转换方式

按照转换方式，有两种（**注意**：boolean类型不参与类型转换）：

- 自动类型转换：
  范围小的数据类型直接转换成范围大的数据类型，小->大。
- 强制类型转换：
  范围大的数据类型强制转换成范围小的数据类型，大->小，会造成精度丢失，尽量不使用浮点数参**与运算**。

#### 转换规则

​		自动类型转换，也称为“隐式类型转换，就是把范围小的数据类型直接转换成范围大的数据类型。		byte、short、char—>int—>long—>float—>double
​		byte、short、char相互之间不转换，他们参与运算首先转换为int类型

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

#### 包装类

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



## java支持的运算符

>- 算术运算符：+，-，*，/，%（膜运算：取余），++，–
>- 赋值运算符：=
>- 关系运算符：> , < , >= , <= , ==（java里等于要用两个等于号） , != （不等于） instapceof
>- 逻辑运算符：&&（与）,||（或）,!（非）
>- 位运算符：&，|，^，~，>>，<<，>>>
>- 条件运算符？：
>- 扩展赋值运算符：+=，-=，*=，/=

#### 运算符的优先级

| 优先级 |                      运算符                      | 顺序     |
| ------ | :----------------------------------------------: | -------- |
| 1      |                    ()、[]、{}                    | 从左向右 |
| 2      |                !、+、-、~、++、--                | 从右向左 |
| 3      |                     *、/、%                      | 从左向右 |
| 4      |                       +、-                       | 从左向右 |
| 5      |                    «、»、>>>                     | 从左向右 |
| 6      |             <、<=、>、>=、instanceof             | 从左向右 |
| 7      |                      ==、!=                      | 从左向右 |
| 8      |                        &                         | 从左向右 |
| 9      |                        ^                         | 从左向右 |
| 10     |                        \|                        | 从左向右 |
| 11     |                        &&                        | 从左向右 |
| 12     |                       \|\|                       | 从左向右 |
| 13     |                        ?:                        | 从右向左 |
| 14     | =、+=、-=、*=、/=、&=、\|=、^=、~=、«=、»=、>>>= | 从右向左 |

#### 位运算





## 常用类

#### Math



#### BigDecimal







## 内部类

* 成员内部类

* 静态内部类

* 局部内部类

* 匿名内部类

## Error和Exception的区别: 

>* Error通常是灾难性的致命的错误，是程序无法控制和处理的，当出现这些异常时，Java虚拟机(JVM)一般会选择终止线程; 
>
>* Exception通常情况下是可以被程序处理的，并且在程序中应该尽可能的去处理这些异常。

### Error

>* Error类对象由Java虚拟机生成并抛出，大多数错误与代码编写者所执行的操作无关。
>* Java虚拟机运行错误(Virtual MachineError)，当JVM不再有继续执行操作所需的内存资源
>  时，将出现OutOfMemoryError。这些异常发生时，Java虚拟机(JVM) 一般会选择线程终
>  止;
>* 还有发生在虚拟机试图执行应用时，如类定义错误(NoClassDefFoundError) 、链接错误
>  (LinkageError)。这些错误是不可查的，因为它们在应用程序的控制和处理能力之外,而且
>  绝大多数是程序运行时不允许出现的状况。

### Exception

![image-20220622162932339](https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202206221629435.png)

#### CheckedException和RuntimeException的区别

>* 在Exception分支中有一个重要的子类RuntimeException (运行时异常)，不需要用throws 声明抛出 异常对象所属类，也可以不用throw 抛出异常对象或异常引用。对于调用该方法，也不需要放于 try-catch 代码块中。（***如果捕获它，就可能发生程序代码错误被掩盖在运行中无法察觉**)*,这些异常一般是由程序逻辑错误引起的，程序应该从逻辑角度尽可能避免这类异常的发生
>
>* 而检查异常 : 一旦 用throw 抛出异常，如果当前方法 可处理异常，那么直接在该方法内用try-catch 去处理。如果当前方法不具备处理该异常的能力，那么就必须在 参数列表后方法体前用 throws 声明 异常 所属类，交给调用该方法的 调用者(方法) 去处理 。

#### ClassNotfoundException

>java开发中经常遇到java.lang.ClassNotfoundException异常，ClassNotfoundException异常一般就是编译时找不到类，Console台就会输出异常信息。一般情况下，我们都会rebuild或者clean一下工程，让项目重新编译一遍

#### 常见异常

>ArithmeticException (算术异常)
>
>MissingResourceException (丢失资源)
>
>ClassNotFoundException (找不到类)等异常
>
>ArithmeticException（除数为0的异常）
>
>BufferOverflowException（缓冲区上溢异常）
>
>BufferUnderflowException（缓冲区下溢异常）
>
>IndexOutOfBoundsException（下标越界异常）
>
>NullPointerException（空指针异常）
>
>EmptyStackException（空栈异常）,
>
>IllegalArgumentException（不合法的参数异常）
>
>NegativeArraySizeException
>
>NoSuchElementException
>
>SecurityException
>
>SystemException
>
>UndeclaredThrowableException

#### Exceptiou关键字

>try :捕获（Throwable，Exception，Error）
>
>catch:处理
>
>finally:无论程序执行与否，finally代码块最终都会执行
>
>throw:主动抛出
>
>throws:无法处理在方法申明抛出
>
>Throwable:异常超类

