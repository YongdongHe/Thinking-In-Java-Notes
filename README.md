#《Thinking In Java》学习笔记


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
此算法把内存空间划为两个相等的区域，每次只使用其中一个区域。 释放旧有对象前，将存活的对象从一个区域复制到另外一个区域，然后清除旧区域，将导致大量的复制行为。



###### 自适应的、分代的、停止-复制、标记-横扫

内存以块为单位，每个块用代数来记录它是否存活。块被引用时，将导致代数增加，垃圾回收器将对上次回收动作之后新分配的块进行整理：可以处理大量短命的临时对象。

同时垃圾回收器定时进行完整清理，大型对象不会被赋值，但是代数增加，小型对象则被复制并整理。如果所有对象都很稳定，这时候GC效率会比较低，虚拟机切换到“标记-清除”算法。当碎片很多时则启用“停止-复制”，这就是自适应技术。



##### 【问题】Java的对象都是在堆上，而非引用计数思想是根据访问“根源于堆栈和静态存储区的引用”所形成的网络来寻找存活的对象的，前面已经提到Java并不会在堆栈上分配对象，那Java虚拟机是如何判断对象存活的呢？



#### 5.7构造器初始化

##### 【问题】static的静态子句跟static成员变量定义都会在类第一次被访问时调用，他们的调用顺序是什么样的？

查看下面的代码

```java

public class Main {
    public static void main(String[] args) {
        System.out.println("Cups1");
        new Cups();
        System.out.println("\nCups1");
        new Cups();
    }

    public static class Cups{
      	/*区域1*/
        static {
            cup1 = new Cup(1);//此处虽然cup1在后面才声明，但是却不会报错
            System.out.println("static");
        }
        static Cup cup2 = new Cup(2);
        static Cup cup1;/*区域1*/
      	/*区域2*/
        {
            System.out.println("not static");
        }/*区域2*/
        Cups(){
            System.out.println("Cups");
        }
    }

    public static class Cup{
        Cup(int mark){
            System.out.println(String.format("Cup(%d)",mark));
        }
    }
}

/**Output
Cups1
Cup(1)
static
Cup(2)
not static
Cups

Cups1
not static
Cups
*/
```

最后结果可以看到，声明为static的子句和static的对象都是按照所写顺序来进行初始化的。并且只会在类被首次访问时会且只会一次运行。但是句子在前面也可以访问到后面声明的对象。

而不带static的子句则在每次构造对象时都会运行一次。如区域2。

#### 5.8数组初始化

##### 【问题】带有可变参数`double...`的函数A，带有可变参数`double...`的函数B，和同名的不带可变参数的函数C，当除了必选参数的附加参数数目为0时，A和B谁将被优先调用？

```java

public class Main {
    public static void main(String[] args) {
        print();
    }
	//A
    static void print(double...ds){
        System.out.println("print double");
        for (double arg : ds){
            System.out.println(arg);
        }
    }
  	//B
    static void print(int...is){
        System.out.println("print int");
        for (int arg : is){
            System.out.println(arg);
        }
    }
  	//C
  	//    static void print(){
	//        System.out.println("empty print");
	//    }
}

/**Output
print int
当C函数取消注释后结果为
empty print
*/
```

输出表明，不带可变参数的函数会更优先被调用，上图调用优先级分别为C>B>A

那为什么B>A呢？

### 《第六章 访问权限控制》学习笔记

#### 6.1 包：库单元

##### 6.1.1代码组织

如图所下类

```java
public class Main {
    public static void main(String[] args) {
        class B{
            B(){
                class C{

                }
            }
            class C{

            }
        }
    }
    class C{

    }
    static class B{

    }

}
class A{
    class D{
        class E{

        }
    }
}
```

在编译后生成的class文件如下

+ A\$D\$E.class
+ A\$D.class
+ A.class
+ Main\$1B\$1C.class
+ Main\$1B\$C.class
+ Main\$1B.class
+ Main\$B.class
+ Main\$C.class
+ Main.class

可以看到class文件是以类的层级关系来命名的，其中类的内外部关系用\$符号表示，如果是局部类或者匿名类的话，则会在类名前加上数字 



### 《第七章 复用类》

#### 7.8 `final`关键字

##### 【问题】final跟C++中的const异同？

相同点：

+ 都可以用来修饰变量、参数、方法
+ 都表示修饰的内容的不变性

不同点：

+ 修饰变量时：`final`允许生成空白的final域，相关的变量可以在声明之后（使用之前）再进行初始化。而`const`则需要在声明时便初始化。
+ 修饰参数时：都表示参数在函数内不能被改变。但是由于C++中指针的存在，`const`修饰`const char* Var`表示参数指针Var所指内容为常量不可变，`const`修饰`char* const Var`时表示参数指针本身为常量不可变，不可再指向其他的对象。
+ 修饰方法时：`final`表示该方法不可被继承。（故所有`private`方法其实都相当于被`final`修饰过）。而`const`修饰的方法智能调用其他也被`const`修饰的方法。



其中`final`在过去被建议使用的原因是，修饰为`final`的方法将被转为内嵌调用，避免了参数压栈、调至方发处执行等等操作，可以提高效率。但是当方法很大时，这种内嵌可能会导致代码膨胀，反而无法提高效率。在最新的Java版本中，虚拟机可以自动检测并优化去掉这种效率降低的额外内嵌。



### 《第八章  多态》

#### 8.2 方法调用绑定

将一个方法调用同一个方法主体关联起来被称作**绑定**。在程序执行前进行绑定称**前期绑定**，在运行时根据对象的类型进行绑定称为**后期绑定**（也成为动态绑定和运行时绑定）。要进行后期绑定，必须在对象中安置某种类型信息。

Java中除了`static`方法和`final`方法，其他的方法都是后期绑定。

效果示例

```java
public class Main {
    public static void main(String[] args) {
        class Tool {
            public void p(){
                System.out.println("Tool p");
            }
            public void f(){
                System.out.println("Tool f");
            }
        }
        class Tool2 extends Tool {
            public void p(){
                System.out.println("Tool2 p");
            }
            /*
            @Override
            //添加override注解可以帮助编译器检查错误
            //Method doesn't override method form its superclass
            public void f2(){
                
            }
            */
        }
        Tool tool = new Tool2();
        tool.p();
        tool.f();
    }
}

/*Output
Tool2 p
Tool f
*/
```

​	

##### 【问题】static方法具有多态性吗？

答案是不具有的。静态方法与类关联，而并非与单个对象关联，所以会出现下面的示例状况：

```java
public class Main {
    public static void main(String[] args) {
        StaticSuper staticSuper = new StaticSub();
        staticSuper.StaticPrint();
    }
    public static class StaticSuper {
        public static void  StaticPrint(){
            System.out.println("StaticSuper print");
        }
    }

    public static class StaticSub extends StaticSuper{
        public static void StaticPrint(){
            System.out.println("StaticSub print");
        }
    }
}

/**Output
StaticSuper print
*/
```

#### 8.3构造器和多态

##### 8.3.3 构造器内部的多态方法行为

##### 【问题】在构造器内部调用正在构造的对象的某个动态绑定方法，如下的调用将会发生什么？

```java
public class Main {
    public static void main(String[] args) {
        StaticSuper staticSuper = new StaticSub();
    }
    public static class StaticSuper {
        int printContent = 1;

        public StaticSuper() {
            StaticPrint();
            DynamicPrint();
        }

        public  void  StaticPrint(){
            System.out.println("StaticSuper print");
        }
        public void DynamicPrint(){
            //System.out.println("Super Dynamic Print");//B
          	System.out.println(printContent);//A
        }
    }

    public static class StaticSub extends StaticSuper{
        int printContent = 2;

        public StaticSub() {
        }

        public void StaticPrint(){
            System.out.println("StaticSub print");
        }
        
        public void DynamicPrint(){
            System.out.println(printContent);
        }
    }
}
```

如果在构造器中执行的方法依然遵循我们之前所了解的动态绑定规则，那么在`StaticSuper()`构造器里运行的`StaticPrint()`和`DynamicPrint()`都应该是子类中的函数，那么结果如何呢？实际输出为下：



```
StaticSub print
0
```

可以看到确实依然遵循了动态绑定规则，但是输出的`printContent`却有些不同了，而且显然调用的依然是`StaticSub`类的`DynamicPrint()`，因为将A\B两句互换后，输出结果依然是这样。说明在调用`StaticSuper`的构造方法时，`StaticSub`的成员并没有初始化完全。

实际上初始化的过程是这样的：

1. 在任何其他事物发生前，将分配给对象的存储空间初始化为二进制的零
2. 在子类构造器构造开始前，调用基类构造器。如果在基类构造器里调用了动态绑定方法，则调用对应的覆盖后的方法。
3. 按照声明的顺序，调用初始化导出类的成员。
4. 调用导出类（对象实际的类型）的构造器主体。


正是因为在步骤2进行时，步骤3尚未进行，才会出现上述的情况。

所以编写构造器时的一条有效准则是： **尽量使用简单的方法使对象进入正常状态，可以的话，避免调用其他方法**。



### 《第九章  接口》

#### 9.2 接口

接口中的方法默认是，也只能是`public`，否则在方法被继承的过程中，其可访问权限就被降低了。这不是设计接口的初衷。

接口中的变量则默认是`public static fianl`，当同时实现了多个接口时，变量前需要加上接口名来区分。

```java
public class Main {
    public static void main(String[] args) {
        StaticSub staticSuper = new StaticSub();
        staticSuper.DynamicPrint();
    }

    public static class StaticSub  implements Print,Print2{
        @Override
        public void DynamicPrint() {
            System.out.println(Print.PRINT + " "+ Print2.PRINT);
        }
    }
    public interface Print{
        int PRINT = 1;
        void DynamicPrint();
    }
    public interface Print2{
        int PRINT = 2;
    }
}
```



#### 9.3 完全解耦

当某个方法操作的是类而非接口，那么当你向将这个方法应用于不在此继承结构中的某个类，你的解决方法只能是为这个类再编写一个方法，而无法将该方法复用。利用接口则在很大程度上放宽了这种限制，使得我们可以编写复用性更好的代码。例如：

```java
public static void main(String[] args) {
        Photo photo = new Photo();
        Pictrue pictrue = new Pictrue();
        PrintSomething(photo);
        PrintSomething(pictrue);
        if (photo instanceof Print)
            System.out.println("instance of print");
        if (photo instanceof Photo)
            System.out.println("instance of photo");
    }
    public static class Photo implements Print{
        @Override
        public void DynamicPrint() {
            System.out.println("This is a photo.");
        }
    }

    public static class Pictrue implements Print{
        @Override
        public void DynamicPrint() {
            System.out.println("This is a Pictrue.");
        }
    }
    interface Print{
        void DynamicPrint();
    }

    public static void PrintSomething(Print print){
        print.DynamicPrint();
    }
}
/*Output
This is a photo.
This is a Pictrue.
instance of print
instance of photo
*/
```

可以看到接口可以帮助我们实现多重继承。

##### 【设计模式】策略设计模式

创建一个根据所传递的参数对象的不同，而具有不同行为的方法，称为策略设计模式。这类方法包含所要执行的算法中固定不变的部分，而“策略”包含变化的部分。下面的`Processor`对象就是一个策略。

```java
class Processor{
    Object process(Object input){return input;}
}
class Upcase extends Processor{
    String process(Object input){
        return ((String)input).toUpperCase();
    }
}
class Downcase extends Processor{
    String process(Object input){
        return ((String)input).toLowerCase();
    }
}
class Splitter extends Processor{
    String process(Object input){
        return Arrays.toString(((String)input).split(" "));
    }
}

public class Main {
    public static void process(Processor p,Object s){
        System.out.println("Using processor " + p.getClass().getSimpleName());
        System.out.println(p.process(s));
    }

    public static void main(String[] args){
        process(new Upcase(),"this is a string");
        process(new Downcase(),"this is a STRING");
        process(new Splitter(),"this is a string");
    }
}
```

而此处若将`Processor`修改为接口，将使耦合性进一步减小，具体参看书9.3

##### 【设计模式】适配器模式

有时你 **无法修改你想使用的类**，即无法在该类基础上继续实现接口，但是你却想它拥有某个其他接口的特性。这时就可以使用适配器模式。

例如我们的手机需要的是低电压来进行充电，现在只有高电压的插座可用，所以我们需要把高电压变为低电压供手机使用。然而我们又不能修改高电压可提供的电压，也不能为这个插座增加更为人性化的功能（提供变压），毕竟电工不是我们的长项~这时候常见的做法是做一个电源适配器。**适配器模式**即是用来解决这一类问题的。即**接受你所拥有的接口，产生你所需要的接口**。

```java
class MobilePhone{
    int battery = 0;
    public void charging(LowVoltageCharger charger){
        charger.charging(battery);
    }
}

interface LowVoltageCharger{
    void charging(int battery);
}

interface HighVoltageCharger{
    void charging(int battery);
}

//此处命名为Adapter是为了与highVoltageCharger区分开来，事实上使用HighVoltageAdapter更合适，
class Adapter implements LowVoltageCharger{
    HighVoltageCharger highVoltageCharger;

    public Adapter(HighVoltageCharger highVoltageCharger) {
        this.highVoltageCharger = highVoltageCharger;
    }

    @Override
    public void charging(int battery) {
        highVoltageCharger.charging(battery);
    }
}

public class Main {
    public static void main(String[] args){
        MobilePhone mobilePhone = new MobilePhone();
        //使用低电压插口直接充电
        LowVoltageCharger lowVoltageCharger = new LowVoltageCharger() {
            @Override
            public void charging(int battery) {
                battery += 1;
            }
        };
        mobilePhone.charging(lowVoltageCharger);
        //使用适配器来让高电压插口也能为手机充电
        HighVoltageCharger highVoltageCharger = new HighVoltageCharger(){
            @Override
            public void charging(int battery) {
                battery += 100;
            }
        };
        Adapter adapter = new Adapter(highVoltageCharger);
        //通过适配器充电
        mobilePhone.charging(adapter);
    }
}
```

可以看出适配器使用了代理的方法，工作交由自己的成员变量进行完成。

由于书上的例子单独拿出来解释适配器模式篇幅依然较大，所以这里用的是笔者自己所理解的例子，如果您有更好的例子或者觉得该示例存在问题，欢迎交流~

#### 9.4 多重继承

##### 【问题】我们应该使用接口还是抽象类？

对于创建类，几乎任何时刻都可以替代为创建一个接口和一个工厂。抽象性应该是应需求而生的，必须时才应该去重构接口，而不是到处添加额外的间接性（创建对象由构造函数到工厂的转变事实上增加了这种间接性）。恰当的原则是优先选择类而不是接口，一旦接口的需求变得明确，再进行重构。

接口是一种重要的工具，但是容易被滥用。

##### 【问题】同时实现多个具有相同方法名的接口，会发生什么？

```java
class People{}
interface Study{
    void f();
}
interface Study2{
    void f();//若换成int f()则会命名冲突
}
class Student {
    public void f() {}
}
class UniversityStudent extends Student implements Study2,Study{
    @Override
    public void f() {}//甚至可以去掉，因为Student中已经实现了
}
```

如图所示，可以同一个方法作为两个接口的实现，但是从程序设计的角度上来说，这样并没有任何好处。



#### 9.7 接口中的域

接口中的域都是`static`和`final`的，所以可以用于常量组生成

```java
public interface Months{
    int JANURAY = 1 , FEBRUAYT = 2;//...
}
```

同时可以被非常量表达式初始化

```java
public interface RandVals{
  	Random RAND = new Random(47);
  	int RANDOM_INT = RAND.nextInt(10);
}
```



#### 9.9 接口与工厂

##### 【设计模式】工厂设计模式

不直接调用构造器来构造对象，而是是通过工厂生成**接口的某个实现的对象**，使得这些对象一部分与接口中的方法有关的操作(操作比较复杂)得以复用，而不需要为每一**类**对象都去实现这些操作。

此处例子采用练习19的练习结果：

```java
interface Game{
    static Random random = new Random();
    int PLAYER1_VICTORY = 1, PLAYER2_VICTORY = 2, DRAW = 3;
    int play();
}

interface GameFactory{
    Game getGame();
}

class CoinTossing implements Game{
    @Override
    public int play() {
        boolean result = random.nextBoolean();
        System.out.println(result?"front":"back");
        return result?PLAYER1_VICTORY:PLAYER2_VICTORY;
    }
	//使用lambda表达式使代码更为简洁
    public static GameFactory factory = () -> new CoinTossing();
}

class DiceRolling implements Game{
    @Override
    public int play() {
        int res1 = random.nextInt() % 6 + 1,res2 = random.nextInt() % 6 + 1;
        System.out.println("result of palyer1:" + res1);
        System.out.println("result of palyer2:" + res2);
        if (res1 == res2)return DRAW;
        return res1 > res2 ? PLAYER1_VICTORY : PLAYER2_VICTORY;
    }
    public static GameFactory factory = () -> new DiceRolling();
}

public class Main {
    public static void main(String[] args){
        playGameInTimes(CoinTossing.factory.getGame(),10);
        playGameInTimes(DiceRolling.factory.getGame(),10);
    }

    public static void playGameInTimes(Game game,int times){
        int[] results = new int[times];
        for (int i = 0; i < times ; i++){
            results[i] = game.play();
        }
        //计算分数ing
        //假设这是一个复杂的控制流程,这个方法主要针对回合制游戏
        //双方进行一定的次数，并根据胜负记录进行复杂的计分，反正就是…很复杂就对了，假设代码很多很冗杂！
      	//此处代码即可通过工厂模式复用
        System.out.println("Game result print.");
    }
}
```



### 《第十章 内部类》

#### 10.3 使用`.this`和`.new`

```java
public class DotNeW{
    public class Inner{}
    public static void main(String[] args){
        DotNeW dn = new DotNeW();
   		DotNeW.Inner dni = dn.new Inner();
    }
}
```

当内部类为`public`时，通过外部类的对象`.new`来调用创建内部类对象。不必也不能声明`dn.new DotNew.Inner()`

当内部类为嵌套类（静态内部类）时，则只需`new DotNew.Inner()`就可以构造内部类对象。

#### 10.6 匿名内部类

匿名内部类没有命名构造器，但是可以通过实例初始化，达到构造器的效果。并且在匿名类内部可以对外部类的方法进行调用，修改外部类的成员。

```java
abstract class Inner{
    int var = 1;
    abstract void setOuterVar(int arg);
}

class Outer{
    int outvar = 10;
    Inner getInner(int innervar){
        return new Inner() {
            int var = innervar;
            @Override
            void setOuterVar(int arg) {
                Outer.this.outvar = arg;
                print();
            }
        };
    }
    void print(){
        System.out.println("Outer print");
    }
}

public class Main {
    public static void main(String[] args){
        Outer outer = new Outer();
        System.out.println("outvar: " + outer.outvar);
        Inner inner = outer.getInner(2);
        inner.setOuterVar(3);
        System.out.println("outvar: " + outer.outvar);
    }
}
/**output
outvar: 10
Outer print
outvar: 3
*/
```

#### 10.9 内部类继承

由于内部类构造器需要连接到外围类对象的引用，平常这种引用是**秘密**的，但是要对内部类进行继承时，无法传递这个秘密引用，下面的特殊语法可以说明关联

```java
class Outer{
    //Outer秘密地传递了引用给Inner，也可以通过Outer.this访问这个引用
    class Inner{}
}
public class InheritInner extends Outer.Inner{
    //如果不做任何处理，Outer的引用是没法通过Inner传递给InheritInner的，报错如下
    //No enclosing instance of type 'Outer' is in scope
    InheritInner(Outer outer){
        //使用outerClass.super()来提供必要引用
        outer.super();
    }
    public static void main(String[] args){
        Outer outer = new Outer();
        InheritInner inheritInner = new InheritInner(outer);
    }
}
```





### 《第十一章 持有对象》

#### 11.4 容器的打印

##### 【问题】`HashMap` `TreeMap` `LinkedHashMap`三者的联系和区别是什么？

三者中只有 `TreeMap`是线程安全的。

 `HashMap`以hashcode的映射来查找元素位置，最终元素存取的顺序和插入顺序是无关的。

`LinkedHashMap`通过在 `HashMap`的基础上增加了一个运行于所有条目的双重链接列表，用来记录每个元素的头一个和后一个元素，并且链表在增加和修改时开销是常数级别的，所以效率不会降低。通过这种方法`LinkedHashMap`实现了存取顺序跟插入顺序一致。

`TreeMap`则按照比较结果的升序来保存键。

#### 11.6 迭代器

##### 【设计模式】迭代器模式

遍历并选择序列中的对象，而不需要关心序列的底层结构。例如迭代器可以同时用于各种容器内元素的遍历，而使用者只需要关心容器内元素的类型，而不需要关心容器类型。



### 《第十二章 通过异常处理错误》

#### 12.3 捕获异常

异常处理理论包括终止模型和恢复模型。恢复模型显得非常诱人但是并不适合。因为他需要了解异常抛出的地点，势必要包含以来抛出位置的非通用性代码，增加了维护和编写代码的难度。



#### 12.6 捕获所有异常

##### 【问题】在捕获异常后重新抛出异常，`printStackTrace()`显示的是原来异常抛出点的调用栈信息还是新的抛出点的信息？

依然为原来抛出点的调用栈信息。若想更新这个信息可以使用`fillInStackTrace()`方法。如下：

```java
public class Main {
    public static void main(String[] args){
        try {
            getValue();
        }
        catch (Exception e){
            e.printStackTrace();
        }
    }
    public static int getValue()throws Exception{
        try{
            throw new Exception();
        }catch (Exception e){
            e.printStackTrace();
            //throw e;
            throw (Exception)e.fillInStackTrace();
        }
    }
}
/**Output
java.lang.Exception
	at com.tencent.mobile.Main.getValue(Main.java:18)
	at com.tencent.mobile.Main.main(Main.java:10)
...
java.lang.Exception
	at com.tencent.mobile.Main.getValue(Main.java:22)
	at com.tencent.mobile.Main.main(Main.java:10)
...
可以看到两次输出异常的位置不同，如果换成注释中的throw e直接抛出，则两次输出相同，如下所示
java.lang.Exception
	at com.tencent.mobile.Main.getValue(Main.java:18)
	at com.tencent.mobile.Main.main(Main.java:10)
...
java.lang.Exception
	at com.tencent.mobile.Main.getValue(Main.java:18)
	at com.tencent.mobile.Main.main(Main.java:10)
...
*/
```

#### 12.8 使用`finally`进行清理

##### 【问题】finally子句能否被return所屏蔽？如下图的代码输出是多少？

```java
public class Main {
    public static void main(String[] args){
        System.out.println(getValue());
    }
    public static int getValue(){
        try{
            return 0;
        }catch (Exception e){
            return 1;
        }finally {
            return 2;
        }
    }
}
/**Output
2
*/
```

且无论图中子句是否被注释，输出都是一样的，可以看出finally无论如何都会被执行。



#### 12.9 异常的限制

##### 【问题】父类跟接口中有同样名字的方法但是标记的异常类型不同，会发生什么？

我们先不考虑有异常抛出的情况

```java
class A{
    public void event(){}
  	//去掉public则会出错，因为继承自A的方法被声明为默认访问权限，事实上弱化了接口的public权限
  	//报错信息attempting to assign weaker access privileges
}

interface B{
    void event();
}

class Asub extends A implements B{
    
}

public class Main {
    public static void main(String[] args){
        Asub asub = new Asub();
    }
}
```

就算`Asub`中没有实现任何方法也能通过编译，因为父类相当于已经实现这个接口了

下面来考虑有异常抛出的情况：

```java
class AtypeException extends Exception{};
class BtypeException extends Exception{};

class A{
  	
    public void event()throws AtypeException{throw new AtypeException();}
}

interface B{
    void event()throws AtypeException;
  	//void event()throws BtypeException;
  	//Override method doesn't throw inherited exceptions.
}

class Asub extends A implements B{
	
}

public class Main {
    public static void main(String[] args){
        Asub asub = new Asub();
    }
}
```

可以发现接口方法中所抛出的异常必须和父类中`throws`标记的类型一样。否则将编译失败。这便是异常限制。



同时可以看到异常限制对构造器不起作用。假如上面加入`A`和`Asub`的构造方法如下:

```java
class A{
    public A()throws AtypeException{}
  	//public A()throws BtypeException{}
  	//A is already defined:异常说明本身不属于方法类型的一部分，不能基于异常说明来重载方法
   	public A(int i)throws CtypeException{}
}

class Asub extends A {
  	//调用默认构造器
    public Asub()throws AtypeException,BtypeException{}
  	//用super指定基类构造器
  	public Asub()throws BtypeException,CtypeException{
        super(1);
    }
}
```

可以看到`Asub`的构造器可以任意抛出异常，不需要跟`A`的构造器一样，但是需要包含对应的基类构造器的异常说明。



### 《第十三章 字符串》

#### 13.1 不可变String

喜闻乐见经典比较

```java
public class Main {
    public static void main(String[] args) {
        String str1 = "abv";
        String str2 = new String("abv");
        String str3 = str1;
        String str4 = "abv";
        String str5 = new String("ab") + "v";
        String str6 = "ab" + "v";
        System.out.println(str1 == str2);
        System.out.println(str1 == str3);
        System.out.println(str1 == str4);
        System.out.println(str1 == str5);
        System.out.println(str1 == str6);

        System.out.println("\n" + str1.equals(str2));
        System.out.println(str1.equals(str3));
        System.out.println(str1.equals(str4));
        System.out.println(str1.equals(str5));
        System.out.println(str1.equals(str6));
    }
}
/**Output
false
true
true
false
true

true
true
true
true
true
*/
```

`==`是对引用的对象进行比较，`equal()`则是对引用对象的内容比较

只要通过`new`生成的`String`，以及其通过运算得到的对象，都是堆上的新对象。而其他的`String`对象都是不可变的。

##### `StringBuilder`和`StringBuffer`
通常字符串操作是通过隐式调用`StringBuilder`来完成的，如果改成显式调用可以显著减少字节码长度，效率更高。
`StringBuffer`是线程安全的，所以开销会大一点，有时应该可能还没有字符串操作快。

#### 13.5 格式化输出

##### 13.5.4 格式化说明符

```
%[argument_index$][flags][width][.precision]conversion
```

+ argument_index : 用于指定参数在参数列表中的位置，未指定的则按顺序从左到右依次填入

  ```java
   System.out.format("%2$s %1$s %s %s %s","a","b","c","d");
   /**Output
   b a a b c
   */
  ```

+ width:最小尺寸大小，默认右对齐

+ flags:例如`-`之类的，可以使width控制的宽度内的内容变成右对齐

+ .precision：精度控制，应用于浮点数时默认6位小数，过多则舍入，过少则补0，无法应用于整数


conversion：

| 类型转换字符 | 含义        |
| ------ | --------- |
| d      | 整数型（十进制）  |
| c      | Unicode字符 |
| b      | Boolean   |
| s      | String    |
| f      | 浮点数（十进制）  |
| e      | 浮点数（科学计数） |
| x      | 整数（十六进制）  |
| h      | 散列码（十六进制） |



#### 13.6 正则表达式

##### Group分组

组数分组，利用group以括号进行分组，为0时表示整个表达式例如

`A(B(C))D`中，0代表`ABCD`,1代表`BC`,2代表`C`,示例如下：

```java
public class Main {
    public static void main(String[] args) {
        String str1 = "InterestinG StrinG AlwayS BorN IN HerE , AhH";
        Pattern pattern = Pattern.compile("([A-Z]+)[a-z]+([A-Z]+)");
        Matcher matcher = pattern.matcher(str1);
        while (matcher.find()){
            for (int i = 0;i <= matcher.groupCount();i++){
                System.out.printf(i +"."+matcher.group(i) + "     ");
            }
            System.out.printf("\n");
        }
    }
}
/**Output
0.InterestinG     1.I     2.G     
0.StrinG     1.S     2.G     
0.AlwayS     1.A     2.S     
0.BorN     1.B     2.N     
0.HerE     1.H     2.E     
0.AhH     1.A     2.H 
*/
```

可以看到这是一种迭代器模式。同时scanner也可以用于正则扫描

```java
public class Main {
    public static void main(String[] args) {
        String str1 = "InterestinG StrinG AlwayS BorN IN HerE , AhH";
        Pattern pattern = Pattern.compile("([A-Z]+)[a-z]+([A-Z]+)");
        Scanner scanner = new Scanner(str1);
        while (scanner.hasNext(pattern)){
            scanner.next(pattern);
            MatchResult matcher = scanner.match();
            System.out.println(matcher.group(0));
        }
    }
}
/**Output
InterestinG
StrinG
AlwayS
BorN
*/
```





### 《第十四章  类型信息》

#### 14.1  为什么需要RTTI(*Run-Time Type Information*)

多态的实现需要。

#### 14.2 Class对象

每个类都有一个`Class`对象，保存在同名的.class文件中。JVM通过类加载器来加载它们并生成对象。

通过`Class.forName`可以根据名字获得`Class`对象，但是名字必须是含包名的完整类名，该方法的副作用则是如果类还没有加载就进行加载：

```java
class N{
    static M m = new M();
    static {
        System.out.println("Static");
    }
    {
        System.out.println("N constucting");
    }
}
class M{
    public M() {
        System.out.println("yes");
    }
}
public class Main {
    public static void main(String[] args) {
        try {
             Class.forName("com.tencent.mobile.N");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
/**Output
yes
Static
*/
```

可以看到static方法和对象是在类被加载时运行和初始化的。

不会引发类加载的情况：

+ 类似`N.class`的方法也可以获得Class对象，但是不会对类进行加载。
+ 类中`static final`的**常量**为编译器常量，不需要类的初始化也可以使用。

#### 14.5 instanceof 与 Class 的等价性

`equals`和`==`结果完全一致，均不考虑继承关系，对类型做最确切的判断

`instanceof`和`isInstance()`的结果完全一致，考虑了继承关系  



#### 14.9 接口与类型信息

##### 【问题】如何使得`HideApi implements PublicApi`中只有A所声明的方法可以调用？

一个比较好的方法是对实现的类`HideApi`使用包管理权限，这样在包外部就无法使用HideApi的方法，也无法将`PublicApi`的对象强转为`HideApi`.

```java
package A;
/** PublicApi.java*/
public interface PublicApi{
    void publicMethod();
}
/** HideApi.java*/
public class HideApi {
    public static PublicApi makePublicApi(){return new Api();}
}
class Api implements PublicApi{
  	public void privateMethod() {}
    @Override
    public void publicMethod() {}
}
```

```java
package B;
public class Main {
    public static void main(String[] args) {
        PublicApi publicApi = HideApi.makePublicApi();
      	/**在这里
    }
}
```

这样在外部就完全隐藏了`Api`类的实现细节。

##### 【问题】通过反射能访问到私有方法吗？

```java
class PrivateApi{
    private void print(){
        System.out.println("A private print.");
    }
}

public class Main {
    public static void main(String[] args) {
        PrivateApi privateApi = new PrivateApi();
        //can't access privateApi.print()
        try {
            Method method = privateApi.getClass().getDeclaredMethod("print");
            method.setAccessible(true);
          	/**
          	public Object invoke(Object obj, Object... args)
          	obj为调用该方法的对象，后面是方法需要的参数
          	*/
            method.invoke(privateApi);
        } catch (NoSuchMethodException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        }
    }
}
/**Output
A private print.
*/
```

可以看到通过反射我们依然可以调用私有方法，而且无论这个类是私有的、或是匿名的。

##### 【问题】反射的`newInstance()`方法在类没有默认构造函数时能调用吗？

答案是不能。会产生`java.lang.NoSuchMethodException`错误

可以采用如下方法来调用构造器并且生成对象

```java
class Person{
    public Person(String name){
        System.out.println(name);
    }
}
public class Main {
    public static void main(String[] args) {
        Class c = Person.class;
        try {
            //Person person = (Person)c.newInstance();  //wrong
            Constructor constructor = c.getConstructor(String.class);
            Person person = (Person) constructor.newInstance("realhe");
        } catch (InstantiationException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (NoSuchMethodException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        }
    }
}
/**
realhe
*/
```



### 《第十五章  泛型》

——理解了边界所在，你才能成为高手。

#### 15.2 简单泛型

```java
public class Main {
    public static void main(String[] args) {
        class A<M,N,O,P>{
            M k;
            N v;
            O t;
            P yk;

            public A(M k, N v, O t, P yk) {
                this.k = k;
                this.v = v;
                this.t = t;
                this.yk = yk;
            }
            public void print(){
                System.out.println(k.getClass());
                System.out.println(v.getClass());
                System.out.println(t.getClass());
                System.out.println(yk.getClass());
            }
        }
        A<String,String,String,Integer> a = new A<>("a","b","c",2);
        a.print();
        A<String,String,Integer,Integer> a1 = new A<>("a","b",1,2);
        a1.print();
    }
}
/**Output
class java.lang.String
class java.lang.String
class java.lang.String
class java.lang.Integer
class java.lang.String
class java.lang.String
class java.lang.Integer
class java.lang.Integer
*/
```

基本用法如上

#### 15.4 泛型方法

非泛型类也可以使用泛型方法，需要将泛型参数置于返回值之前

```java
public static <T> void f(T a){
	System.out.print(a.getClass().getName());
}
```

##### 15.4.1 利用泛型进行类型推断

在老的Java版本中，进行泛型定义时需要写出如下的代码，造成每次泛型参数都要重复两遍。

```java
Map<Person,List<? extends Pet>> petPeople = 
                new HashMap<Person,List<? extends Pet>>();
```

在新版本中，jdk支持如下的写法；

```java
Map<Person,List<? extends Pet>> petPeople =  new HashMap<>();
```

即可以进行自动的类型推断，原来老版本中我们可以通过如下办法做一些省事的事情，即泛型返回值的方法可以自动根据赋值的目标类型，来确定泛型参数，并生成对应的对象

```java
class Person{}
class Pet{}
public class Main {
    public static void main(String[] args) {
        Map<Person,List<? extends Pet>> petPeople = New.map();//is OK
      	f(New.map());//wrong
    }
    public static class New{
        public static <K,V> Map<K,V> map(){
            return new HashMap<K, V>();
        }
    }
  	public static f( Map<Person,List<? extends Pet>> petPeople) {}
}
```

但是这种参数推断只能用在赋值上，用在参数传递上就会出错,如上所示

#### 15.5 匿名内部类

下面的例子结合泛型，工厂模式为Person类实现了Generator接口。使得Person类的对象创建更为安全可靠。

```java
interface Generator<T>{
    T next();
}
class Person{
    public Person(String name){
        System.out.println(name);
    }
    public static Generator<Person> generator 
            = new Generator<Person>() {
        @Override
        public Person next() {
            return new Person("default name");
        }
    };
}
public class Main {
    public static void main(String[] args) {
        Person person = Person.generator.next();
    }
}
```

#### 15.7 擦除的神秘之处

##### 【问题】`ArrayList<String> list1`与`ArrayList<Integer> list2`的类型相同吗？

如下，泛型的参数不同时，人们的直觉认为它们是不同类型，但是程序上会认为他们是相同的类型。

```java
public class Main {
    public static void main(String[] args) {
        ArrayList<String> list1 = new ArrayList<>();
        ArrayList<Integer> list2 = new ArrayList<>();
        System.out.println(list1.getClass().equals(list2.getClass()));
        System.out.println(Arrays.toString(list1.getClass().getTypeParameters()));
    }
}
/**Output
true
[E]
*/
```

**在泛型内部无法获得任何有关泛型参数类型的信息**，及时用`Class.getTypeParameters()`取出`TypeVariable`对象数组，你也最多只能获得泛型参数的占位符，没有任何意义。

原因是因为泛型是使用擦除来实现的。在使用泛型时，任何具体的类型信息都被擦除了，唯一留下的就是他们的原生类型`ArrayList`。

擦除其实是一种妥协。其减少了泛型的泛化性。如果泛型更早地出现，可能会使用具体化来实现，使类型参数保持为第一类实体，这样就能进行基于类型的语言操作和反射操作。由于使用擦除来实现，我们无法这样做。

基于擦除的实现中，泛型的类型被当作第二类类型处理，在有些环境中无法使用。



##### 15.7.4 边界处的动作

泛型与非泛型的`get`与`set`函数生成的字节码是相同的，对进入`set()`函数的参数类型检查不需要，因为这由编译器检查。对`get`函数返回的值检查仍旧是需要的。但是使用泛型时，这些检查由编译器自动插入，可以减少自己编码的“噪声”。

泛型中的所有动作都发生在**边界**处：

+ 对传递进来的值进行额外的编译器检查
+ 插入对传递出去的值的转型

#### 12.8 擦除的补偿

如下的操作由于确切类型未知都无法工作：

```java
class Erased<T>{
    private final int SIZE = 100;
  	@suppressWarnings("unchecked")
    public void f(Object arg){
        if (arg instanceof T){}				//Error
        T var = new T();					//Error
        T[] array = new T[SIZE];			//Error
        T[] array = (T)new Object()[SIZE];	//Unchecked Waining
    }
}
```

但是在后面我们会知道由于擦除的对象最后都擦除为`Object`，事实上最后一条语句是可以行得通的，我们也将在 `ArrayList` 类中看到大量的类似的做法。同时这种做法需要在方法名前面加上`@suppressWarnings("unchecked")`注解。虽然这种做法不一定是最好的解决之道，但是标准库确实会产生大量的类似的警告。



#### 15.9 边界

泛型参数同时对类跟接口有限制时，写法如下：

```java
class People{}
interface Study{}
class Student implements Study{}
class Worker extends People{}
//要求继承自People同时实现Study接口
class Group <T extends People & Study>{
}
//要求实现Study接口
class AnotherGroup <T extends Study>{
}

public class Main {
    public static void main(String[] args) {
        Group<Student> group = new Group<Student>();
        Group<Worker> group1 = new Group<Worker>();
    }
}
```



#### 15.11 问题

##### 15.11.2 实现参数化接口

```java
class People{}
interface Study<T>{}
class Math{}
class ComputerScience{}
//不能通过编译
class Student implements Study<Math>{}
class UniversityStudent extends Student implements Study<ComputerScience>{}
//可以通过编译
class Student implements Study{}
class UniversityStudent extends Student implements Study{}
```

不能编译的原因是相当于两次重复实现了相同的接口（虽然这一点其实并不会有问题，在前面的问题有提到，Ref: [【问题】同时实现多个具有相同方法名的接口，会发生什么？]( # 9.4 多重继承)

但是将泛型参数移除后又可以编译了……很迷……



##### 15.11.4 重载

下面的程序无法编译，因为擦除导致重载方法签名相同

```java
class People<K,T>{}
    void f(K k){}
    void f(T t){}
}
```

##### 15.11.5 基类劫持接口

同理在这里有说明

Ref: [【问题】同时实现多个具有相同方法名的接口，会发生什么？]( # 9.4 多重继承)

但是更类似于15.11.2中的情况；由于基类实现了`Study<Math>`的接口，导致派生类无法实现 `Study<ComputerScience>`接口。这点在实现`Comparable`接口时会非常麻烦。



