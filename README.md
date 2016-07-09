#《Thinking In Java》学习笔记
[TOC]

### 阅读计划

#### 章节列表

1. 对象导论
2. 一切都是对象
3. 操作符
4. 控制执行流程
5. 初始化与清理
6. 访问权限控制
7. 复用类
8. 多态
9. 接口
10. 内部类
11. 持有对象
12. 通过异常处理错误
13. 字符串
14. 类型信息
15. 泛型
16. 数组
17. 容器深入研究
18. Java I/O系统
19. 枚举类型
20. 注解
21. 并发
22. 图形化用户界面

#### 目标重点学习章节

5#初始化与清理

8#多态

9#接口

11#持有对象

14#类型信息

15#泛型

17#容器深入研究

18#Java I/O系统

20#注解

21#并发

#### 学习计划

《Think In Java》共计22章，达800多页，重点难点章节集中在后半部分，计划学习时间为2016/7/7-2016/7/14，部分章节有过学习经历，结合情况采取前半部分内容快速学习和总结，后半部分预期以较慢速度学习并且每天回顾。每章对有疑问的地方采取问答式记录，有必要时记录测试代码跟输出结果。其余属于可查阅型的知识点也可做一定程度的记录。

因时间原因与过去使用SWT和Swing的经历，暂时决定放弃22章的系统学习，将中心放在上述目标重点学习章节，以及其他不太熟悉的章节上。



### 《第一章 对象导论》 学习笔记

#### 1.4 被隐藏的具体实现

类关键字访问控制关系

|      | Public | Private | Protected | Friendly（default）    |
| ---- | ------ | ------- | --------- | -------------------- |
| 类内   | 可见     | 可见      | 可见        | 包内同public,包外同private |
| 类外   | 可见     | 不可见     | 不可见       | -                    |
| 子类   | 可见     | 不可见     | 可见        | -                    |

#### 1.7 伴随多态的可互换对象

前期绑定：编译器对需要调用的具体函数解析为将被执行的代码的绝对地址。

后期绑定：使用对象中存储的信息计算方法体的地址（在第八章中详述），C++中利用virtual关键字表示该方法为动态绑定，而Java中动态绑定为默认行为



#### 1.8 单根继承结构

单根继承：所有的类最终都继承自单一的基类。

优点：所有对象都具备某些功能，方便统一实现基础操作，使得所有对象都很容易在堆上创建，简化了参数传递和垃圾回收器实现，而垃圾回收器正是Java相对C++的重要改变。除此之外在Java中所有对象都保证有自己的类型信息，是反射特性的基础。



#### 1.10 对象的创建和生命期

##### 对象创建方式

堆栈上创建：为了追求最大执行速度，对象的存储空间和声明周期可以在编写程序时确定，可以通过将对象置于堆栈（自动变量*Automatic variable* ||  限域变量*Scoped variable*  || 局部变量*Local variable*）、静态存储区域（静态变量 *Static*）来实现。牺牲了灵活性，必须在编写程序时知道对象确切的数量、生命周期和类型。

堆上创建：在被称为堆（*Heap*）的内存池中动态创建对象。需要大量的时间在堆中分配存储空间，这个时间远大于在堆栈中创建存储空间的时间。

##### 带来的问题

在堆栈上创建的语言编译器容易知道其存活的时间，并可以自动销毁它；在堆上创建对象时，编译器则对对象的声命周期一无所知，故需通过编程方式及时销毁变量（不销毁则会引起内存泄漏）。

##### 垃圾回收器

典型的例子：C++中的new操作和Java中的new操作对应的都是在堆上创建对象。但是Java的垃圾回收期提供了更高层的保障避免了内存泄漏问题。垃圾回收器的实现受益于单根继承结构和Java只能在堆上创建对象这两个特性。



#### 1.13 Java与Internet

客户端编程和脚本语言编程已经成为Web开发的主流技术。

Java在最初提供了applet，可以在HTML中嵌入Java代码进行执行，类似代码如下：（摘自W3C）

```java
import java.applet.*;
import java.awt.*;
import java.net.*;
public class ImageDemo extends Applet
{
  	private Image image;
    private AppletContext context;
    public void init()
    {
        context = this.getAppletContext();
        String imageURL = this.getParameter("image");
        if(imageURL == null)
        {
           imageURL = "java.jpg";
        }
        try
        {
           URL url = new URL(this.getDocumentBase(), imageURL);
           image = context.getImage(url);
        }catch(MalformedURLException e)
        {
           e.printStackTrace();
           // Display in browser status bar
           context.showStatus("Could not load image!");
        }
     }
     public void paint(Graphics g)
     {
        context.showStatus("Displaying image");
        g.drawImage(image, 0, 0, 200, 84, null);
        g.drawString("www.javalicense.com", 35, 100);
     } 
}
```

HTML代码：

```html
<html>
<title>The ImageDemo applet</title>
<hr>
<applet code="ImageDemo.class" width="300" height="200">
<param name="image" value="java.jpg">
</applet>
<hr>
</html>
```

后来发展为servlet及其衍生物 JSP，消除了处理具有不同能力的浏览器时所遇到的问题。详情参看《企业Java编程思想》（*Thinking in Enterprise Java*）







### 《第二章 一切都是对象》学习笔记

#### 2.1用引用操纵对象

内存中对象相当于电视，引用相当于遥控器，遥控器可脱离电视独立存在。

```java
String s_ref0;						//s_ref0为引用，而不是对象
String s_ref = "Thinking In Java";	//s_ref为关联了对象的引用 
```

#### 2.2必须由你创建所有对象

##### 2.2.1存储到什么地方

数据存储的位置有以下五个位置：

- 寄存器：最快存储区，处于处理器内部，需要根据需求由处理器分配。在Java中不支持这一点，也无法在编程中察觉到。但是在C和C++中，允许编程者向处理器提出寄存器分配建议。
- 堆栈：位于通用RAM（随机访问存储器，即内存）中，通过向上和向下移动堆栈指针，实现内存的分配和释放。但是创建程序时必须知道堆栈内所有项的确切声明周期，以便控制堆栈指针的移动。某些Java数据，例如**对象引用**就存储与堆栈中，但是Java对象不存储其中。 Ref: [对象创建方式](#对象创建方式)  
- 堆：一种通用内存池，用于存放所有的Java对象。创建时不需要知道数据的声明周期，比较灵活。但是相对堆栈分配而言，存储分配和清理时间更多。
- 常量存储：常量值直接存放在程序代码内部，或者跟其他部分分离开来，存放在ROM（只读存储器）中。
- 非RAM存储：数据存储于程序之外，程序不运行时也可以存在。典型例子有**流对象**（转化为字节流发送给另外一台机器）和**持久化对象** （存放与磁盘上）。必要时可以恢复呈常规的、基于RAM的对象。

##### 2.2.2基本类型

| 类型      | 大小      | 默认值            |
| ------- | ------- | -------------- |
| byte    | 8 bits  | (byte)0        |
| char    | 16 bits | '\u0000'(null) |
| boolean | 1 bit   | false          |
| short   | 16 bits | (short)0       |
| int     | 32 bits | 0              |
| long    | 64 bits | 0L             |
| float   | 64 bits | 0.0f           |
| double  | 64 bits | 0.0d           |



#### 2.3永远不需要销毁对象

##### 2.3.1作用域

以下代码在C和C++中合法，但是在Java中会报告x已定义过。（Java中较大作用域变量，不能被较小作用域变量所覆盖和隐藏）

```java
{
  	int x = 12;
  	{
  		int x = 96;//Illegal
	}
}
```

##### 2.3.2 对象的作用域

Java对象跟基本类型声明周期不一样，new创建的对象可以存活于作用域之外。例如String对象：

```java
{
  	String s = new String("This is a string.");
}	//End Of Scope
```

引用s在作用域终点就消失了，但是s指向的String对象仍然占据内存空间。这点跟C++是类似的，故C++需要确保对象在需要使用的时间内一直存活，并且使用完后销毁它。

而在Java中，这一点由垃圾回收器来完成，垃圾回收期见识了所有用new创建的对象，并且辨别那些不会再被引用的对象。





### 《第三章 操作符》学习笔记

#### 3.1 更简单的打印语句

##### 静态导入

使用 `import static`来进行声明为`static`的对象、引用、 方法的导入。



```java
//导入前
import java.utils.*;
//导入操作
import static java.lang.System.out.*;
public class StaticImport{
  	public static void main(String[] args){
  		//导入前打印写法
      	System.out.print("Hello Java");
      	//导入后写法
      	print("Hello Java");
	}
}
```



#### 3.4 赋值

为基本类型赋值时，基本类型存储了实际数值，相当于值传递。

为对象赋值时，真正操作的是对象的引用，相当于引用传递。

`String`相对于其他类比较特殊，那它的对象进行赋值时是哪种传递呢？

##### 【问题】String类的对象赋值时，采用的是值传递还是引用传递？

测试Demo如下：

```java
public class Main {
    public static void main(String[] args) {
        int a = 1;
        int b = 2;
        System.out.println("before change");
        System.out.println("a:" + a + " b:" + b);
        a = b;
        b = 3;
        System.out.println("after change");
        System.out.println("a:" + a + " b:" + b);

        Num c = new Num();
        Num d = new Num();
        c.val = 1;
        d.val = 2;
        System.out.println("\nbefore change");
        System.out.println("c:" + c + " d:" + d);
        c = d;
        d.val = 3;
        System.out.println("after change");
        System.out.println("c:" + c + " d:" + d);

        String s1 = "str1";
        String s2 = "str2";
        System.out.println("\nbefore change");
        System.out.println("s1:" + s1 + " s2:" + s2);
        s1 = s2;
        s2 = "str3";
        System.out.println("after change");
        System.out.println("s1:" + s1 + " s2:" + s2);
    }
    private static class Num{
        int val;
        @Override
        public String toString() {
            return "Num{" +
                    "val=" + val +
                    '}';
        }
    }
}

/**Output
before change
a:1 b:2
after change
a:2 b:3

before change
c:Num{val=1} d:Num{val=2}
after change
c:Num{val=3} d:Num{val=3}

before change
s1:str1 s2:str2
after change
s1:str2 s2:str3
*/
```



##### 【问题】short、char、byte类型的数值进行移位时，以多少位作为标准位数？

在对short、char、byte类型的数值进行移位时，他们会被转化为int类型，并且得到的结果也是int类型，所以在使用无符号右移结合赋值操作时，例如:

```java
short s = -1;
s >>>= 10;
```

会发生先被转换为int，右移，再转回short的现象，导致截断，得到-1的结果，示例如下：

```java
public class Main {
    public static void main(String[] args) {
        short b = -1;
        System.out.println(Integer.toBinaryString(b>>>10) + " " + (b>>>10));
        b>>>=10;
        System.out.println(Integer.toBinaryString(b) + " " + b);
    }
}

/**Output
1111111111111111111111 4194303
11111111111111111111111111111111 -1
*/
```

###  《第四章 控制执行流程》学习笔记

【问题】带标签和不带标签的continue\break分别所起作用是什么？

- 不带标签的continue会退回最内层循环的开头，并继续执行
- 带标签的continue会到达标签的位置，并重新进入紧接在那个标签后的循环
- 不带标签的break会中断并跳出当前循环
- 带标签的break会中断并跳出标签所指的循环

###  《第五章 初始化与清理》学习笔记

#### 5.5清理：终结处理和垃圾回收

##### finalize()的作用

一旦垃圾回收期准备好释放对象占用的存储空间，将首先调用其`finalize()`方法，并且在下一次垃圾回收动作发生时，才真正回收对象占用的内存。其不可作为通用的清理方法，而只能与内存及其回收有关。

##### 三个要点

+ 对象可能不被垃圾回收：在对象已经不再使用时，垃圾回收并不是一定准时的，且回收动作本身有一定开销。
+ 垃圾回收并不等于析构：析构函数在局部对象出作用域以后会立即调用，非常准点。new对象则在delete调用时，调用相应的析构函数。
+ 垃圾回收只与内存有关：垃圾回收的唯一目的即回收内存，所以`finalize()`也必须与此相关。


##### 【问题】finalize()调用的不确定性：`finalize()`手动调用会影响垃圾回收吗？`System.gc()`方法调用可以使垃圾回收立刻进行吗？

在这个部分书上的代码由于输出只有一种，即`finalize()`成功调用的情况，不能很好地表现其不确定性。我在这之上做了修改，在里先附上自定义类`Book`的代码：

```java
class Book{
    boolean checkedout = false;
        Book(boolean checkedout){
            this.checkedout = checkedout;
        }
        void checkIn(){
            checkedout = false;
        }

        @Override
        protected void finalize(){
            if (checkedout){
                System.out.println("Error : checked out");
            }else{
                try{
                    System.out.println("finalize");
                    super.finalize();
                }catch (Throwable e){
                    e.printStackTrace();
                }
            }
        }
}
```

验证不确定性：

```java
public static void main(String[] args) {
    Book novel = new Book(true);
    novel.checkIn();
    //novel.finalize();
    new Book(true);
    //书上说明gc()方法会强制进行垃圾回收，但是查阅资料表明，gc()方法依然只是对JVM的一个垃圾回收建议，所以上述两个对象依然不会保证被回收
    System.gc();
    //System.runFinalization();
	//System.runFinalizersOnExit(true);
}
/**Output
有时为
Error : checked out
有时为：
Error : checked out
finalize
有时为空

当`novel.finalize()`未被注释时，输出则在前面的输出前加上一条finalize

*/
```

可以看到在调用了`System.gc()`的情况下，垃圾回收依然不是一定会发生的。这就给`finalize()`的执行带来了极大的不确定性。

如果手动调用`novel.finalize()`，那么只相当于普通的方法调用，不会对垃圾回收造成任何影响。

**之后的笔记中，为了叙述方便，垃圾回收简称为GC(Garbage Collection)**

##### 【问题】`System.gc()`  `System.runFinalization()  `  `System.runFinalizersOnExit(true)` 三者的区别是什么？

`System.gc()` ：建议执行GC，效果参见上述验证不确定性的例子

 `System.runFinalization()  ` ：将失去引用(被丢弃但是`finalize()`还没有调用)的对象进行回收，但是此处的`finalize()`调用依然不考虑手动调用的情况。函数说明和示例如下：

```java
/**
     * Runs the finalization methods of any objects pending finalization.
     * <p>
     * Calling this method suggests that the Java Virtual Machine expend
     * effort toward running the <code>finalize</code> methods of objects
     * that have been found to be discarded but whose <code>finalize</code>
     * methods have not yet been run. When control returns from the
     * method call, the Java Virtual Machine has made a best effort to
     * complete all outstanding finalizations.
     * <p>
     * The call <code>System.runFinalization()</code> is effectively
     * equivalent to the call:
     * <blockquote><pre>
     * Runtime.getRuntime().runFinalization()
     * </pre></blockquote>
     *
     * @see     java.lang.Runtime#runFinalization()
     */
    public static void runFinalization() {
        Runtime.getRuntime().runFinalization();
    }

	public static void main(String[] args) {
        Book novel = new Book(true);
        novel.checkIn();
        new Book(true).finalize();
        System.runFinalization();
    }
	/**Output
	恒为
	Error : checked out
	Error : checked out
	*/
```

 `System.runFinalizersOnExit(true)` ：在程序运行结束时，强制对所有的对象进行回收，但是该方法已经不再被推荐使用了。且从输出结果上我们可以看到，回收顺序可能是已经失去引用的对象被优先回收的。

```java
public static void main(String[] args) {
    Book novel = new Book(true);
    novel.checkIn();
    new Book(true);
    System.runFinalizersOnExit(true);
}
/**Output
恒为
Error : checked out
finalize
*/
```

##### 5.5.4垃圾回收器如何工作

###### 引用计数

每个对象含有一个引用计数器，当有引用连接至对象时，计数器+1；引用离开作用域或者被置为null时，计数器-1。进行GC时会在含有全部对象的列表上进行遍历，当发现某个对象引用计数为0，则回收该对象。缺陷在于**对象之前存在循环引用**，例如

```java
class A{
  public B b_of_a;
   
}
class B{
  public A a_of_b;
}
public class Main{
    public static void main(String[] args){
    A a = new A();
    B b = new B();
    a.b_of_a = b;
    b.a_of_b = a;
    }
}
```

a要被回收时，需等待成员b_of_a被回收，而b_of_a指向b，所以需要先回收b；b要被回收时，需要等待a_of_b的回收，即等待a的回收。两者陷入了互相等待，出现**对象应该被回收，但是引用计数器不为0**的情况。

定位这种情况对于GC而言工作量非常大，且每次为对象进行计数也需要不小的开销，故引用计数常用来说明GC的工作方式，而不会实际应用在Java虚拟机中。



###### 非引用计数思想

对活的对象，一定能最终追溯到其存活在堆栈或静态存储区之间的引用，虽然这一引用链可能会穿过多个对象层次（以局部变量为例，在引用出作用域以后，会从堆栈中移除）。只要从堆栈和静态存储区开始，直到*根源于堆栈和静态存储区的引用* 所形成的网络，就能找到活的对象。其他的对象则是可以被回收的。



##### GC基本算法

###### 标记-清除（Mark - Sweep）

此算法执行分两阶段。第一阶段从引用根节点开始标记所有被引用的对象，第二阶段遍历整个堆，把未标记的对象清除。此算法会产生内存碎片。当产生的垃圾较少甚至不会产生垃圾时，GC速度是非常快的。



###### 停止-复制（Stop - Copying）
此算法把内存空间划为两个相等的区域，每次只使用其中一个区域。 



##### 【问题】Java的对象都是在堆上，而非引用计数思想是根据访问“根源于堆栈和静态存储区的引用”所形成的网络来寻找存活的对象的，前面已经提到Java并不会在堆栈上分配对象，那Java虚拟机是如何判断对象存活的呢？




