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





## 

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









## Exception



>ClassNotfoundException
>java开发中经常遇到java.lang.ClassNotfoundException异常，ClassNotfoundException异常一般就是编译时找不到类，Console台就会输出异常信息。一般情况下，我们都会rebuild或者clean一下工程，让项目重新编译一遍

