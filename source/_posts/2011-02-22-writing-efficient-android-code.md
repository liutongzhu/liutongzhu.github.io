---
author: 飞雪无情
comments: true
date: 2011-02-22 02:38:51+00:00
layout: post
slug: writing-efficient-android-code
title: 编写高效的Android代码
wordpress_id: 96
categories:
- Android
tags:
- Android
---



毫无疑问，基于Android平台的设备一定是嵌入式设备。现代的手持设备不仅仅是一部电话那么简单，它还是一个小型的手持电脑，但是，即使是最快的最高端的手持设备也远远比不上一个中等性能的桌面机。

这就是为什么在编写Android程序时要时刻考虑执行的效率，这些系统不是想象中的那么快，并且你还要考虑它电池的续航能力。这就意味着没有多少剩余空间给你去浪费了，因此，在你写Android程序的时候，要尽可能的使你的代码优化而提高效率。

本页介绍了几种可以让开发者的Android程序运行的更加有效率的方法。通过下面的一些跳转的连接，你可以学会怎么让你的程序更加有效运行




内容



	
  * 介绍

	
  * 尽量避免创建对象

	
  * 使用自身方法

	
  * 使用虚拟优于使用接口

	
  * 使用静态优于使用虚拟

	
  * 避免内部使用Setter和Getter

	
  * 缓冲属性调用

	
  * 声明Final常量

	
  * 慎重使用增强型的For循环语句

	
  * 避免列举类型Avoid Enums

	
  * 通过内联类使用包空间

	
  * 尽量避免浮点类型

	
  * 一些标准操作的时间比较

	
  * [结束语](/admin/blogs/l#closing_notes)





## 介绍


对于如何判断一个系统的不合理，这里有两个基本的原则：



	
  * 不要做不必要做的事情。

	
  * 尽可能的节省内存的使用。


下面的所有方法都是基于这两项的。

有人会认为本页花了大量的篇幅去讲如何进行“初步优化”（ premature
optimization）。虽然有时候微观优化对开发高效的数据结构和算法很困难，但是在嵌入式手持设备上面你毫无选择。例如，如果把桌面电脑的虚拟机
移植到你的Android系统中，你会发现你写的程序会耗尽你的内存。这就会导致程序运行起来极度缓慢，即使不考虑它对系统上其他的运行程序的影响。

这就是为什么上面两条原则这么重要。Android的成功在于开发程序提供给用户的体验，然而用户体验的好坏又决定于你的代码是否能及时的响应而不
至于慢的让人崩溃。因为我们所有的程序都会在同一个设备上面运行，所以我们把它们作为一个整体来考虑。本文就像你考驾照需要学习的交通规则一样：如果所有
人遵守，事情就会很流畅；但当你不遵守时，你就会撞车。

在我们讨论实质问题之前，有一个简要的说明：无论虚拟机是否是Java编译器的一个特点，下面介绍的所有观点都是正确的。如果我们有两种方法完成同
样的事情，但是foo()的解释执行要快于bar()，那么foo()的编译速度一定不会比bar()慢，仅仅靠编译器使你的代码运行速度提升是不明智的
做法。




## 尽可能避免创建对象（Object）


对象的创建并不是没有代价的。一个带有线程分配池的generational的内存管理机制会使创建临时对象的代价减少，不是分配内存总比不上不分配内存好。

如果你在一个用户界面的循环中分配一个对象，你不得不强制的进行内存回收，那么就会使用户体验出现稍微“打嗝”的现象。

因此，如果没有必要你就不应该创建对象实例。下面是一件有帮助的例子：



	
  * 当从原始的输入数据中提取字符串时，试着从原始字符串返回一个子字符串，而不是创建一份拷贝。你将会创建一个新的字符串对象，但是它和你的原始数据共享数据空间。

	
  * 如果你有一个返回字符串地方法，你应该知道无论如何返回的结果是StringBuffer，改变你的函数的定义和执行，让函数直接返回而不是通过创建一个临时的对象。


一个比较激进的方法就是把一个多维数组分割成几个平行的一维数组：

	
  * 一个Int类型的数组要比一个Integer类型的数组要好，但着同样也可以归纳于这样一个原则，两个Int类型的数组要比一个（int，int）对象数组的效率要高的多
。对于其他原始数据类型，这个原则同样适用。

	
  * 
如果你需要创建一个包含一系列Foo和Bar对象的容器（container）时，记住：两个平行的Foo[]和Bar[]要比一个（Foo,Bar）对
象数组的效率高得多。（这个例子也有一个例外，当你设计其他代码的接口API时；在这种情况下，速度上的一点损失就不用考虑了。但是，在你的代码里面，你
应该尽可能的编写高效代码。）


一般来说，尽可能的避免创建短期的临时对象。越少的对象创建意味着越少的垃圾回收，这会提高你程序的用户体验质量。




## 使用自身方法（Use Native Methods）


当处理字符串的时候，不要犹豫，尽可能多的使用诸如String.indexOf()、String.lastIndexOf()这样对象自身带有的方法。因为这些方法使用C/C++来实现的，要比在一个java循环中做同样的事情快10-100倍。

还有一点要补充说明的是，这些自身方法使用的代价要比那些解释过的方法高很多，因而，对于细微的运算，尽量不用这类方法。




## 使用虚拟优于使用接口


假设你有一个HashMap对象，你可以声明它是一个HashMap或则只是一个Map：

    
    Map myMap1 = new HashMap();
    HashMap myMap2 = new HashMap();




哪一个更好呢？

一般来说明智的做法是使用Map，因为它能够允许你改变Map接口执行上面的任何东西，但是这种“明智”的方法只是适用于常规的编程，对于嵌入式系统并不适合。通过接口引用来调用会花费2倍以上的时间，相对于通过具体的引用进行虚拟函数的调用。

如果你选择使用一个HashMap，因为它更适合于你的编程，那么使用Map会毫无价值。假定你有一个能重构你代码的集成编码环境，那么调用Map
没有什么用处，即使你不确定你的程序从哪开头。（同样，public的API是一个例外，一个好的API的价值往往大于执行效率上的那点损失）




## 使用静态优于使用虚拟


如果你没有必要去访问对象的外部，那么使你的方法成为静态方法。它会被更快的调用，因为它不需要一个虚拟函数导向表。这同时也是一个很好的实践，因为它告诉你如何区分方法的性质（signature），调用这个方法不会改变对象的状态。




## 尽可能避免使用内在的Get、Set方法


像C++iyangde编程语言，通常会使用Get方法(例如` i = getCount()`
)去取代直接访问这个属性（`i=mCount`
）。 这在C++编程里面是一个很好的习惯，因为编译器会把访问方式设置为Inline，并且如果想约束或调试属性访问，你只需要在任何时候添加一些代码。

在Android编程中，这不是一个很不好的主意。虚方法的调用会产生很多代价，比实例属性查询的代价还要多。我们应该在外部调用时使用Get和Set函数，但是在内部调用时，我们应该直接调用。




## 缓冲属性调用Cache Field Lookups


访问对象属性要比访问本地变量慢得多。你不应该这样写你的代码：

    
    for (int i = 0; i < this.mCount; i++)
          dumpItem(this.mItems[i]);




而是应该这样写：

    
      int count = this.mCount;
      Item[] items = this.mItems;
    
      for (int i = 0; i < count; i++)
          dumpItems(items[i]);




（我们直接使用“this”表明这些是它的成员变量）

一个相似的原则就是：决不在一个For语句中第二次调用一个类的方法。例如，下面的代码就会一次又一次地执行getCount（）方法，这是一个极大地浪费相比你把它直接隐藏到一个Int变量中。

    
    for (int i = 0; i < this.getCount(); i++)
        dumpItems(this.getItem(i));




这是一个比较好的办法，当你不止一次的调用某个实例时，直接本地化这个实例，把这个实例中的某些值赋给一个本地变量。例如：

    
     protected void drawHorizontalScrollBar(Canvas canvas, int width, int height) {
            if (isHorizontalScrollBarEnabled()) {
                int size = mScrollBar.getSize(false
    
    );
                if (size <= 0) {
                    size = mScrollBarSize;
                }
                mScrollBar.setBounds(0, height- size, width, height);
                mScrollBar.setParams(
                        computeHorizontalScrollRange(),
                        computeHorizontalScrollOffset(),
                        computeHorizontalScrollExtent(), false
    
    );
                mScrollBar.draw(canvas);
            }
        }




这里有四次`mScrollBar`
的属性调用，把`mScrollBar`
缓冲到一个堆栈变量之中，四次成员属性的调用就会变成四次堆栈的访问，这样就会提高效率。

附带说一下，对于方法同样也可以像本地变量一样具有相同的特点。




## 声明Final常量


我们可以看看下面一个类顶部的声明：

    
    static int intVal = 42;
    static String strVal = "Hello, world!";


当一个类第一次使用时，编译器会调用一个类初始化方法——``
，这个方法将42存入变量`intVal`
，并且为`strVal`
在类文件字符串常量表中提取一个引用，当这些值在后面引用时，就会直接属性调用。

我们可以用关键字“final”来改进代码：

    
    static final int intVal = 42;
    static final String strVal = "Hello, world!";


这个类将不会调用es a ``
方法，因为这些常量直接写入了类文件静态属性初始化中，这个初始化直接由虚拟机来处理。代码访问`intVal`
将会使用Integer类型的42，访问`strVal`
将使用相对节省的“字符串常量”来替代一个属性调用。

将一个类或者方法声明为“final”并不会带来任何的执行上的好处，它能够进行一定的最优化处理。例如，如果编译器知道一个Get方法不能被子类重载，那么它就把该函数设置成Inline。

同时，你也可以把本地变量声明为final变量。但是，这毫无意义。作为一个本地变量，使用final只能使代码更加清晰（或者你不得不用，在匿名访问内联类时）。




## 慎重使用增强型For循环语句


增强型For循环（也就是常说的“For-each循环”）经常用于Iterable接口的继承收集接口上面。在这些对象里面，一个
iterator被分配给对象去调用它的hasNext（）和next（）方法。在一个数组列表里面，你可以自己接的敷衍它，在其他的收集器里面，增强型
的for循环将相当于iterator的使用。

尽管如此，下面的源代码给出了一个可以接受的增强型for循环的例子：

    
    public class Foo {
        int mSplat;
        static Foo mArray[] = new Foo[27];
    
        public static void zero() {
            int sum = 0;
            for (int i = 0; i < mArray.length; i++) {
                sum += mArray[i].mSplat;
            }
        }
    
        public static void one() {
            int sum = 0;
            Foo[] localArray = mArray;
            int len = localArray.length;
    
            for (int i = 0; i < len; i++) {
                sum += localArray[i].mSplat;
            }
        }
    
        public static void two() {
            int sum = 0;
            for (Foo a: mArray) {
                sum += a.mSplat;
            }
        }
    }


zero()
函数在每一次的循环中重新得到静态属性两次，获得数组长度一次。

one()
函数把所有的东西都变为本地变量，避免类查找属性调用

two()

函数使用Java语言的1.5版本中的for循环语句，编辑者产生的源代码考虑到了拷贝数组的引用和数组的长度到本地变量，是例遍数组比较好的方法，它在
主循环中确实产生了一个额外的载入和储存过程（显然保存了“a”），相比函数one（）来说，它有一点比特上的减慢和4字节的增长。

总结之后，我们可以得到：增强的for循环在数组里面表现很好，但是当和Iterable对象一起使用时要谨慎，因为这里多了一个对象的创建。




## 避免列举类型Avoid Enums


列举类型非常好用，当考虑到尺寸和速度的时候，就会显得代价很高，例如：

    
    public class Foo {
       public enum Shrubbery { GROUND, CRAWLING, HANGING }
    }


这会转变成为一个900字节的class文件（Foo$Shrubbery.class）。第一次使用时，类的初始化要在独享上面调用方法去描述列举的每一项，每一个对象都要有它自身的静态空间，整个被储存在一个数组里面（一个叫做“$VALUE”的静态数组）。那是一大堆的代码和数据，仅仅是为了三个整数值。

    
    Shrubbery shrub = Shrubbery.GROUND;


这会引起一个静态属性的调用，如果GROUND是一个静态的Final变量，编译器会把它当做一个常数嵌套在代码里面。

还有一点要说的，通过列举，你可以得到更好地API和一些编译时间上的检查。因此，一种比较平衡的做法就是：你应该尽一切方法在你的公用API中使用列举型变量，当处理问题时就尽量的避免。

在一些环境下面，通过`ordinal`
（）方法获取一个列举变量的整数值是很有用的，例如：把下面代码

    
    for (int n = 0; n < list.size(); n++) {
        if (list.items[n].e == MyEnum.VAL_X)
           // do stuff 1
        else if (list.items[n].e == MyEnum.VAL_Y)
           // do stuff 2
    }


替换为：

    
       int valX = MyEnum.VAL_X.ordinal();
       int valY = MyEnum.VAL_Y.ordinal();
       int count = list.size();
       MyItem items = list.items();
    
       for (int  n = 0; n < count; n++)
       {
            int  valItem = items[n].e.ordinal();
    
            if (valItem == valX)
              // do stuff 1
            else if (valItem == valY)
              // do stuff 2
       }


在一些条件下，这会执行的更快，虽然没有保障。




## 通过内联类使用包空间


我们看下面的类声明

    
    public class Foo {
        private int mValue;
    
        public void run() {
            Inner in = new Inner();
            mValue = 27;
            in.stuff();
        }
    
        private void doStuff(int value) {
            System.out.println("Value is " + value);
        }
    
        private class Inner {
            void stuff() {
                Foo.this.doStuff(Foo.this.mValue);
            }
        }
    }


这里我们要注意的是我们定义了一个内联类，它调用了外部类的私有方法和私有属性。这是合法的调用，代码应该会显示"Value is 27"。

问题是Foo$Inner在理论上（后台运行上）是应该是一个完全独立的类，它违规的调用了Foo的私有成员。为了弥补这个缺陷，编译器产生了一对合成的方法：

    
    /*package*/ static int Foo.access$100(Foo foo) {
        return foo.mValue;
    }
    /*package*/ static void Foo.access$200(Foo foo, int value) {
        foo.doStuff(value);
    }


当内联类需要从外部访问“mValue”和调用“doStuff”时，内联类就会调用这些静态的方法，这就意味着你不是直接访问类成员，而是通过公共的方法来访问的。前面我们谈过间接访问要比直接访问慢，因此这是一个按语言习惯无形执行的例子。

让拥有包空间的内联类直接声明需要访问的属性和方法，我们就可以避免这个问题，哲理诗是包空间而不是私有空间。这运行的更快并且去除了生成函数前面
东西。（不幸的是，它同时也意味着该属性也能够被相同包下面的其他的类直接访问，这违反了标准的面向对象的使所有属性私有的原则。同样，如果是设计公共的
API你就要仔细的考虑这种优化的用法）




## 避免浮点类型的使用


在奔腾CPU发布之前，游戏作者尽可能的使用Integer类型的数学函数是很正常的。在奔腾处理器里面，浮点数的处理变为它一个突出的特点，并且浮点数与整数的交互使用相比单独使用整数来说，前者会使你的游戏运行的更快，一般的在桌面电脑上面我们可以自由的使用浮点数。

不幸的是，嵌入式的处理器通常并不支持浮点数的处理，阴齿所有的“float”和“double”操作都是通过软件进行的，一些基本的浮点数的操作就需要花费毫秒级的时间。

同事，即使是整数，一些芯片也只有乘法而没有除法。在这些情况下，整数的除法和取模操作都是通过软件实现。当你创建一个Hash表或者进行大量的数学运算时，这都是你要考虑的。




## 一些标准操作的时间比较


为了距离说明我们的观点，下面有一张表，包括一些基本操作所使用的大概时间。注意这些时间并不是绝对的时间，绝对时间要考虑到CPU和时钟频率。系
统不同，时间的大小也会有所差别。当然，这也是一种有意义的比较方法，我们可以比叫不同操作花费的相对时间。例如，添加一个成员变量的时间是添加一个本地
变量的四倍。

Action
Time


<table border="0" >
<tbody >
<tr >

<td >Add a local variable
</td>

<td >1
</td>
</tr>
<tr >

<td >Add a member variable
</td>

<td >4
</td>
</tr>
<tr >

<td >Call String.length()
</td>

<td >5
</td>
</tr>
<tr >

<td >Call empty static native method
</td>

<td >5
</td>
</tr>
<tr >

<td >Call empty static method
</td>

<td >12
</td>
</tr>
<tr >

<td >Call empty virtual method
</td>

<td >12.5
</td>
</tr>
<tr >

<td >Call empty interface method
</td>

<td >15
</td>
</tr>
<tr >

<td >Call Iterator:next() on a HashMap
</td>

<td >165
</td>
</tr>
<tr >

<td >Call put() on a HashMap
</td>

<td >600
</td>
</tr>
<tr >

<td >Inflate 1 View from XML
</td>

<td >22,000
</td>
</tr>
<tr >

<td >Inflate 1 LinearLayout containing 1 TextView
</td>

<td >25,000
</td>
</tr>
<tr >

<td >Inflate 1 LinearLayout containing 6 View objects
</td>

<td >100,000
</td>
</tr>
<tr >

<td >Inflate 1 LinearLayout containing 6 TextView objects
</td>

<td >135,000
</td>
</tr>
<tr >

<td >Launch an empty activity
</td>

<td >3,000,000
</td>
</tr>
</tbody>
</table>



## 结束语


写高效的嵌入式程序的最好方法就是要搞清楚你写的程序究竟做了些什么。如果你真的想分配一个iterator类，进一切方法的在一个List中使用增强型的for循环，使它成为一个有意而为之的做法，而不是一个无意的疏漏而产生负面影响。

有备无患，搞清楚你在做什么！你可以假如你自己的一些行为准则，但是一定要注意你的代码正在做什么，然后开始寻找方法去优化它。

转自：http://www.chinaup.org/docs/toolbox/performance.html
