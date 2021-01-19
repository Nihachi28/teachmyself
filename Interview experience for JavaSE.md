# Java基础面经

- [Java基础面经](#java----)
  * [描述正则表达式及其用途](#-----------)
  * [说明一下如何跳出当前的多重嵌套循环？](#------------------)
  * [讲讲&和&&的区别？](#----------)
  * [int和Integer有什么区别？](#int-integer------)
  * [请你说明String 和StringBuffer的区别（StringBuilder）](#----string--stringbuffer----stringbuilder-)
  * [String是最基本的数据类型吗?](#string-----------)
  * [谈谈大O符号(big-O notation)](#---o---big-o-notation-)
  * [讲讲数组(Array)和列表(ArrayList)的区别？什么时候应该使用Array而不是ArrayList？](#-----array-----arraylist-------------array---arraylist-)
  * [解释什么是值传递和引用传递](#-------------)
  * [为什么会出现4.0-3.6=0.40000001这种现象](#------40-36-040000001----)
  * [Lambda表达式的优缺点](#lambda-------)
  * [请你说明符号“==”比较的是什么？和equals()有什么区别？](#------------------equals--------)
  * [解释hashCode()和equals()方法有什么联系？](#--hashcode---equals----------)
  * [Object若不重写hashCode()的话，hashCode()如何计算出来的？](#object----hashcode-----hashcode----------)
  * [为什么HashMap比较key要重写equals还要重写hashcode？](#---hashmap--key---equals----hashcode-)
  * [介绍一下map的分类和常见的情况](#----map---------)
  * [final关键字是怎么用的](#final--------)
  * [谈谈关于Synchronized和lock](#----synchronized-lock)
  * [介绍一下volatile](#----volatile)
  * [什么是构造函数？什么是构造函数重载？什么是复制构造函数？](#----------------------------)
  * [重写(Overriding)和方法重载(Overloading)是什么意思？](#---overriding-------overloading-------)
  * [面向对象的"六原则一法则"](#-------------)
  * [如何通过反射获取和设置对象私有字段的值？](#--------------------)
  * [内部类可以引用包含他的类的成员吗，如果可以，有没有什么限制吗？](#-------------------------------)
  * [说明JAVA语言如何进行异常处理，关键字：throws,throw,try,catch,finally分别代表什么意义？在try块中可以抛出异常吗？](#--java---------------throws-throw-try-catch-finally----------try----------)
  * [说说Java的接口](#--java---)
  * [请判断当一个对象被当作参数传递给一个方法后，此方法可改变这个对象的属性，并可返回变化后的结果，那么这里到底是值传递还是引用传递?](#----------------------------------------------------------------)
  * [说说Static Nested Class 和 Inner Class的不同](#--static-nested-class---inner-class---)
  * [讲讲abstract class和interface有什么区别?](#--abstract-class-interface------)
  * [请说明一下final, finally, finalize的区别。](#-----final--finally--finalize----)
  * [面向对象的特征有哪些方面](#------------)
  * [说明Comparable和Comparator接口的作用以及它们的区别](#--comparable-comparator------------)
  * [谈谈如何通过反射创建对象？](#-------------)
  * [请解释一下extends和super泛型限定符](#-----extends-super-----)
  * [讲讲什么是泛型？](#--------)
  * [请说明”static”关键字是什么意思？Java中是否可以覆盖(override)一个private或者是static的方法？](#----static----------java--------override---private---static----)
  * [列举你所知道的Object类的方法并简要说明。](#-------object----------)
  * [解释一下String为什么不可变？](#----string-------)
  * [讲讲wait方法的底层原理](#--wait-------)
  * [请说明List、Map、Set三个接口存取元素时，各有什么特点？](#---list-map-set-----------------)
  * [阐述ArrayList、Vector、LinkedList的存储性能和特性](#--arraylist-vector-linkedlist--------)
  * [请说明Collection 和 Collections的区别。](#---collection---collections----)
  * [请说明ArrayList和LinkedList的区别？](#---arraylist-linkedlist----)
  * [说明HashMap和Hashtable的区别？](#--hashmap-hashtable----)
  * [请你说说Iterator和ListIterator的区别？](#----iterator-listiterator----)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>



## 描述正则表达式及其用途

编写处理字符串的程序，需要查找符合某和复杂规则的字符串。正则表达式就是用于描述这些规则的工具，即记录文本规则的代码。



## 说明一下如何跳出当前的多重嵌套循环？

在最外层循环前加一个标记如A，然后用break A，就可以跳出多重循环

```java
class Dome {
	public static void main(String[] args) {
		a:{
	       System.out.println("0");
	       b:{
	       System.out.println("1");
	       c:{
	       System.out.println("2");
	       if(1==1)
	       break a;
	       }
	       System.out.println("3");
	       }
	       System.out.println("4");
	       }
	       System.out.println("5");
	}
}
```



## 讲讲&和&&的区别？

&运算符有两种用法：(1)按位与；(2)逻辑与。而&&是短路与运算。

> 按位与例子：
>
> a&b —— a=129，b=128
>
> “a”的值是129，转换成二进制就是10000001，而“b”的值是128，转换成二进制就是10000000。根据与运算符的运算规律，只有两个位都是1，结果才是1，可以知道结果就是10000000，即128。

逻辑与和短路与差别很大。虽然二者都要求运算符左右两端的布尔值都是true整个表达式的值才是true，但是短路与若左边的为false则不再执行右边，逻辑与则相反，不论左边结果依旧会判断右边情况。

很多时候我们可能都需要用&&而不是&，例如在验证用户登录时判定用户名**不是null而且不是空字符串**，应当写为：`username != null && !username.equals("")`，二者的顺序不能交换，因为第一个条件如果不成立，根本不能进行字符串的equals比较，否则会产生`NullPointerException`异常。



## int和Integer有什么区别？

int为基本数据类型，integer为包装类型；Java中每一个基本数据类型都有一对一的包装类型对应。Java中存在自动拆箱/自动装箱机制，使得两者可以互相转换。

\- 原始类型: boolean，char，byte，short，int，long，float，double
\- 包装类型：Boolean，Character，Byte，Short，Integer，Long，Float，Double

包装类的存在将这些基本数据类型可以被当作对象进行操作，每个包装类都写有对应的方法。

```java
Integer(String s)
static Integer valueOf(String s)
```



## 请你说明String 和StringBuffer的区别（StringBuilder）

都可以存储和操作字符串。

String提供了数值不可改变的字符串，而StringBuffer类提供的字符串可供修改，方便我们动态构造字符数据。

StringBuffer在内存中是一个数字，占用空间少效率高。可以通过toString和其本身的构造方法与String互相转换。可利用append方法加入新的字符串。

StringBuffer是线程安全的，StringBuilder是线程不安全的。因为 StringBuffer 的所有公开方法都是 synchronized 修饰的，而 StringBuilder 并没有 synchronized 修饰。

StringBuffer 每次获取 toString 都会直接使用缓存区的 toStringCache 值来构造一个字符串。

而 StringBuilder 则每次都需要复制一次字符数组，再构造一个字符串。

所以，缓存冲这也是对 StringBuffer 的一个优化吧，不过 StringBuffer 的这个toString 方法仍然是同步的。

所以，StringBuffer 适用于用在多线程操作同一个 StringBuffer 的场景，如果是单线程场合 StringBuilder 更适合。



## String是最基本的数据类型吗?

基本数据类型包括byte、int、char、long、float、double、boolean和short。
java.lang.String类是final类型的，因此不可以继承这个类、不能修改这个类。为了提高效率节省空间，我们应该用StringBuffer类。



## 谈谈大O符号(big-O notation)

大O符号描述了当数据结构里面的元素增加的时候，算法的规模或者是性能在最坏的场景下有多么好。大O符号也可用来描述其他的行为，比如：内存消耗。

定义是：对于函数f(n),g(n),如果存在一个常数c，使得f(n)<=c*g(n),则f(n)=O(g(n))；



## 讲讲数组(Array)和列表(ArrayList)的区别？什么时候应该使用Array而不是ArrayList？

Array和ArrayList的不同点：

* Array可以包含基本类型和对象类型，ArrayList只能包含对象类型。
* Array大小是固定的，ArrayList的大小是动态变化的。
* ArrayList底层数据存储结构是数组，查找快，增删慢
* ArrayList提供了更多的方法和特性，比如：addAll()，removeAll()，iterator()等等。
* 对于基本类型数据，集合使用自动装箱来减少编码工作量。但是，当处理固定大小的基本数据类型的时候，这种方式相对比较慢。所以处理基本类型时应该使用Array，对象类型时候应该考虑含有较多方法与特性的ArrayList。



## 解释什么是值传递和引用传递

值传递是对基本型变量而言的，传递的是该变量的一个副本，改变副本不影响原变量。

引用传递一般是对于对象型变量而言的，传递的是该对象地址的一个副本，并不是原对象本身。所以对引用对象进行操作会同时改变原对象。



## 为什么会出现4.0-3.6=0.40000001这种现象

2进制的小数无法精确的表达10进制小数，计算机在计算10进制小数的过程中要先转换为2进制进行计算，这个过程中出现了误差。



## Lambda表达式的优缺点

优点：

1. 简洁
2. 非常容易并行计算
3. 可能代表未来的编程趋势

缺点：

1. 若不用并行计算，很多时候计算速度没有比传统的 for 循环快。（并行计算有时需要预热才显示出效率优势）
2. 不容易调试
3. 若其他程序员没有学过 lambda 表达式，代码不容易让其他语言的程序员看懂



## 请你说明符号“==”比较的是什么？和equals()有什么区别？

“==”如果两边是引用类型数据，则基于内存引用对比两个对象，如果两个对象的引用完全相同（指向同一个对象）时，操作将返回true，否则返回false。

“==”如果两边是基本数据类型，就是比较数值是否相等。

若对一个类不重写，它的equals()方法是比较是对象的地址。



和equals()相比，**==的比较规则是定死的**，就是比较两个数据的值。**而 equals() 的比较规则是不固定的，可以由用户自己定义**。

比如说String类中的equals()方法：

从源码可以看到, 当调用 String 类型数据的 equals() 方法时，**首先会判断两个字符串的引用是否相等，也就是说两个字符串引用是否指向同一个对象，是则返回true。**

**如果不是指向同一个对象，则把两个字符串中的字符挨个进行比较。**由于 s1 和 s3 字符串都是 "hello"，是可以匹配成功的，所以最终返回 true。

为什么要设计equals()方法？因为**系统无法提前知道两个对象在什么条件下算相等，Java干脆把判断对象是否相等的权力交给编程人员**



## 解释hashCode()和equals()方法有什么联系？

Java对象的eqauls方法和hashCode方法是这样规定的：

1. 相等（相同）的对象（equals方法返回true）必须具有相等的哈希码（或者散列码）；
2. 如果两个对象的hashCode相同，它们并不一定相同；



## Object若不重写hashCode()的话，hashCode()如何计算出来的？

Object 的 hashcode 方法是本地方法，也就是用 c 语言或 c++ 实现的，该方法直接返回对象的 内存地址。



## 为什么HashMap比较key要重写equals还要重写hashcode？

因为HashMap中，如果要比较key是否相等，要同时使用这两个函数！

即便有相同含义的两个对象，hashcode的默认情况是内存地址，比较也是不相同的。HashMap中的比较key是先求出key的hashcode()，比较其是否相等，若相等再比较equals()，若相等则认为他们确实是相等的。

如果只重写hashcode()不重写equals()方法，当比较equals()时只是看他们是否为同一对象（即进行内存比较）

* 重载hashCode()是为了对同一个key，能得到相同地HashCode。
* 重载equals()是为了向HashMap表明当前对象和key上所保存的对象是相等的。



## 介绍一下map的分类和常见的情况

java为数据结构中的映射定义了一个接口java.util.Map；

它有四个实现类，分别是HashMap、Hashtable、LinkedHashMap和TreeMap；

Map主要用于存储健值对，根据键得到值，因此不允许键重复（重复了就覆盖了），但允许值重复。

**HashMap**

HashMap 是一个最常用的Map，它根据键的HashCode值存储数据，根据键可以直接获取它的值，具有很快的访问速度。遍历时，取得数据的顺序是完全随机的。 

HashMap 最多只允许一条记录的键为Nul；允许多条记录的值为 Null；

HashMap 不支持线程的同步（线程不安全），即任一时刻可以有多个线程同时写HashMap，可能会导致数据的不一致。如果需要同步，可以用 Collections的synchronizedMap方法使HashMap具有同步的能力，或者使用ConcurrentHashMap。

**Hashtable** 

与 HashMap类似，不同的是：

* 它不允许记录的键或者值为空；
* 它支持线程的同步（线程安全），即任一时刻只有一个线程能写Hashtable；
* 因此也导致了Hashtable在写入时会比较慢；

**LinkedHashMap**

LinkedHashMap 是HashMap的一个子类，保存了记录的插入顺序（即在用Iterator遍历LinkedHashMap时，先得到的记录肯定是先插入的）也可以在构造时用带参构造，按照应用次数排序。

在遍历的时候会比HashMap慢，不过有种情况例外，当HashMap容量很大，实际数据较少时，遍历起来可能会比 LinkedHashMap慢，因为**LinkedHashMap的遍历速度只和实际数据有关，和容量无关**，而**HashMap的遍历速度和他的容量有关**。

**TreeMap**

TreeMap实现SortMap接口，能够把它保存的记录根据键排序，默认是按键值的升序排序，也可以指定排序的比较器，当用Iterator遍历TreeMap时，得到的记录是排过序的。

一般情况下，我们用的最多的是HashMap，在Map中插入、删除和定位元素，HashMap 是最好的选择。但如果您要按自然顺序或自定义顺序遍历键，那么TreeMap会更好。如果需要输出的顺序和输入的相同,那么用LinkedHashMap可以实现,它还可以按读取顺序来排列。



## final关键字是怎么用的

用final修饰一个类时，表明这个类不能被继承。

* final类中的成员变量可以根据需要设为final，但是要注意final类中的所有成员方法都会被隐式地指定为final方法。

用final修饰一个变量时：

* 如果是基本数据类型的变量，则其数值一旦在初始化之后便不能更改；
* 如果是引用类型的变量，则在对其初始化之后便不能再让其指向另一个对象；

使用final修饰一个方法时，是为了把方法锁定，以防任何继承类修改它的含义。



## 谈谈关于Synchronized和lock

synchronized是Java的关键字，当它用来修饰一个方法或者一个代码块的时候，能够保证在同一时刻最多只有一个线程执行该段代码。

JDK1.5以后引入了自旋锁、锁粗化、轻量级锁，偏向锁来有优化关键字的性能。

Lock是一个接口（实现类对象是ReentranLock对象），Lock实现提供了比使用synchronized方法和语句更为广泛的锁定操作。而synchronized是Java中的关键字，synchronized是内置的语言实现；

* synchronized在发生异常时，会自动释放线程占有的锁，因此不会导致死锁现象发生；
* 而Lock在发生异常时，如果没有主动通过unLock()去释放锁，则很可能造成死锁现象，因此使用Lock时需要在finally块中释放锁；
* Lock可以让等待锁的线程响应中断，而synchronized却不行，使用synchronized时，等待的线程会一直等待下去，不能够响应中断；
* 通过Lock可以知道有没有成功获取锁，而synchronized却无法办到。



## 介绍一下volatile

volatile关键字是用来保证有序性和可见性的。

这跟Java内存模型有关。

有序性：代码不一定是按照我们自己书写的顺序来执行的，编译器会做重排序，CPU也会做重排序的，这样的重排序是为了减少流水线的阻塞，提高CPU的执行效率，所以有happens-before规则，其中有条就是volatile变量规则：对一个变量的写操作先行发生于后面对这个变量的读操作；（有序性实现的是通过插入内存屏障来保证的）

可见性：首先Java内存模型分为，主内存，工作内存。比如线程A从主内存把变量从主内存读到了自己的工作内存中，做了加1的操作，但是此时没有将i的最新值刷新会主内存中，线程B此时读到的还是i的旧值。

综上，一旦一个共享变量（类的成员变量、类的静态成员变量）被volatile修饰之后，那么就具备了两层语义：

1. 保证了不同线程对这个变量进行操作时的可见性，即一个线程修改了某个变量的值，这新值对其他线程来说是立即可见的。
2. 禁止进行指令重排序。



## 什么是构造函数？什么是构造函数重载？什么是复制构造函数？

**构造函数**

当新对象被创建的时候，构造函数会被调用。

每一个类都有构造函数，在程序员没有给类提供自定义构造函数的情况下，Java编译器会为这个类创建一个默认的构造函数。

**构造函数重载**

构造函数重载和方法重载很相似。可以为一个类创建多个构造函数，但每一个构造函数必须有它自己**唯一的**参数列表。

**复制构造函数**

Java**不支持像C++中那样的复制构造函数**，这个不同点是因为如果你不自己写构造函数的情况下，Java不会创建默认的复制构造函数。



## 重写(Overriding)和方法重载(Overloading)是什么意思？

重写（Overriding）

指子类重新定义了父类的方法。方法的覆盖必须有相同的方法名，参数列表和返回类型。

重载（Overloading）

指同一个类里面两个或者多个方法的方法名相同但是参数不同。



方法的重载和重写都是实现多态的方式，区别在于前者实现的是编译时的多态性，而后者实现的是运行时的多态性。



## 面向对象的"六原则一法则"

* 单一职责原则
  * 一个类只做它该做的事情，即高内聚，一个代码模块只完成一项功能
* 开闭原则
  * 软件实体应当对扩展开放，对修改关闭。即新增功能只派生新类，而不改原有内容。
    * 关键点：（1）抽象、（2）封装可变性
* 接口隔离原则
  * 接口要小而专，绝不能大而全
    * 琴棋书画应该是四个接口，而不是一个类的四个方法
    * java中的接口代表能力、代表约定、代表角色
* 依赖倒转原则
  * 面向接口编程，即声明方法的参数类型、方法的返回类型、变量的引用类型，尽可能使用抽象类型而不用具体类型。
* 里氏替换原则
  * 任何时候都可以用子类替换父类 —— 为了检查继承关系是否合理
    * 子类一定是增加父类的能力，而不是减少父类的能力
    * 能力多的对象可以当作能力少的对象使用没有任何问题
* 合成聚合复用原则
  * 优先使用聚合或合成关系复用代码
    * 类与类之间简单的说有三种关系，Is-A关系、Has-A关系、Use-A关系，分别代表继承、关联和依赖
      * 关联关系根据其关联的强度又可以进一步划分为关联、聚合和合成，但说白了都是Has-A关系，合成聚合复用原则想表达的是优先考虑Has-A关系而不是Is-A关系复用代码
* 迪米特法则（最少知识原则）
  * 一个对象应当对其他对象有尽可能少的了解
    * 调停者模式
    * 减小了系统的耦合度和复杂度



## 如何通过反射获取和设置对象私有字段的值？

通过类对象的`getDeclaredField()`方法字段（Field）对象，然后再通过字段对象的`setAccessible(true)`将其设置为可以访问，接下来就可以通过get/set方法来获取/设置字段的值了。

```java
import java.lang.reflect.Method;
class MethodInvokeTest {
    public static void main(String[] args) throws Exception {
        String str = "hello";
        Method m = str.getClass().getMethod("toUpperCase");
        System.out.println(m.invoke(str));  // HELLO
    }
}
```



## 内部类可以引用包含他的类的成员吗，如果可以，有没有什么限制吗？

一个内部类对象可以访问创建它的外部类对象的内容。

* 内部类如果不是static的，那么它可以访问创建它的外部类对象的所有属性。
* 内部类如果是static的，即为nested class，那么它只可以访问创建它的外部类对象的所有static属性



## 说明JAVA语言如何进行异常处理，关键字：throws,throw,try,catch,finally分别代表什么意义？在try块中可以抛出异常吗？

Java 通过面向对象的方法进行异常处理，把各种不同的异常进行分类，并提供了良好的接口。在Java中，每个异常都是一个对象，它是Throwable类或其它子类的实例。当一个方法出现异常后便抛出一个异常对象，该对象中包含有异常信息，调用这个对象的方法可以捕获到这个异常并进行处理。

一般情况下是用try来执行一段程序，如果出现异常，系统会抛出（throws）一个异常，这时候你可以通过它的类型来捕捉（catch）它，或最后（finally）由缺省处理器来处理。用try来指定一块预防所有”异常”的程序。紧跟在try程序后面，应包含一个catch子句来指定你想要捕捉的”异常”的类型。throw语句用来明确地抛出一个”异常”。throws用来标明一个成员函数可能抛出的各种”异常”。Finally为确保一段代码不管发生什么”异常”都被执行一段代码。



## 说说Java的接口

由于Java不支持多继承，而有可能某个类或对象要使用分别在几个类或对象里面的方法或属性，现有的单继承机制就不能满足要求。

故产生了接口，与继承相比，接口有更高的灵活性，因为接口中没有任何实现代码。当一个类实现了接口以后，该类要实现接口里面所有的方法和属性。

接口里面的属性在默认状态下面都是public static，所有方法默认情况下是public。一个类可以实现多个接口。



## 请判断当一个对象被当作参数传递给一个方法后，此方法可改变这个对象的属性，并可返回变化后的结果，那么这里到底是值传递还是引用传递?





## 说说Static Nested Class 和 Inner Class的不同



## 讲讲abstract class和interface有什么区别?

|          | Abstract class                                               | Interface                                                    |
| :------: | :----------------------------------------------------------- | :----------------------------------------------------------- |
|  实例化  | 不能                                                         | 不能                                                         |
|    类    | 一种继承关系，一个类只能使用一次继承关系。可以通过继承多个接口实现多重继承 | 一个类可以实现多个interface                                  |
| 数据成员 | 可有自己的                                                   | 静态的不能被修改即必须是static final，一般不在此定义         |
|   方法   | 可以私有的，非abstract方法，必须实现                         | 不可有私有的，默认是public，abstract 类型                    |
|   变量   | 可有私有的，默认是friendly 型，其值可以在子类中重新定义，也可以重新赋值 | 不可有私有的，默认是public static final 型，且必须给其初值，实现类中不能重新定义，不能改变其值。 |
| 设计理念 | 表示的是“is-a”关系                                           | 表示的是“like-a”关系                                         |
|   实现   | 需要继承，要用extends                                        | 要用implements                                               |

声明方法的存在而不去实现它的类被叫做抽象类（abstract class）；

接口（interface）是抽象类的变体。在接口中，所有方法都是抽象的；



## 请说明一下final, finally, finalize的区别。

* final 用于声明属性，方法和类，分别表示属性不可变，方法不可覆盖，类不可继承。
* finally是异常处理语句结构的一部分，表示总是执行。
* finalize是Object类的一个方法，在垃圾收集器执行的时候会调用被回收对象的此方法，可以覆盖此方法在垃圾收集时回收其他资源，例如关闭文件等



## 面向对象的特征有哪些方面

* 抽象
  * 过程抽象、数据抽象
* 继承
  * 层次模型，明确表述共性
* 封装
  * 把过程和数据包围，访问只可通过定义的界面
* 多态
  * 不同类的对象对同一消息做出响应



## 说明Comparable和Comparator接口的作用以及它们的区别

```java
public interface Comparable<T> {
   public int compareTo(T o);
}
public interface Comparator<T> {
   int compare(T o1, T o2);
   boolean equals(Object obj);
}
```

**Comparator 和 Comparable 比较**

Comparable是排序接口；若一个类实现了Comparable接口，就意味着“该类支持排序”。

而Comparator是比较器；我们若需要控制某个类的次序，可以建立一个“该类的比较器”来进行排序。

我们不难发现：Comparable相当于“内部比较器”，而Comparator相当于“外部比较器”。



Java提供了只包含一个`compareTo()`方法的**Comparable接口**。这个方法可以个给两个对象排序。强行对实现它的每个类的对象进行整体排序。这种排序被称为类的自然排序，类的`compareTo()`方法被称为它的自然比较方法。

实现此接口的对象列表（和数组）可以通过`Collections.sort（Arrays.sort）`进行自动排序。实现此接口的对象可以用作有序映射中的键or有序集合中的元素，无需指定比较器。

* 返回：
  * 负数 —— 小于
  * 0 —— 等于
  * 正数 —— 大于



## 谈谈如何通过反射创建对象？

* 方法1：通过类对象调用`newInstance()`方法，例如：String.class.newInstance()

* 方法2：通过类对象的`getConstructor()`或`getDeclaredConstructor()`方法获得构造器（Constructor）对象并调用其`newInstance()`方法创建对象，例如：

  ```java
  String.class.getConstructor(String.class).newInstance("Hello");
  ```



## 请解释一下extends和super泛型限定符

1. 泛型中上界和下界的定义
   * 上界<? extend Fruit>
     * 代表使用的泛型只能是Fruit类型的子类/本身
   * 下界<? super Apple>
     * 代表使用的泛型只能是Apple类型的父类/本身





## 讲讲什么是泛型？



## 请说明”static”关键字是什么意思？Java中是否可以覆盖(override)一个private或者是static的方法？

“static”关键字表明一个成员变量或者是成员方法可以在没有所属的类的实例变量的情况下被访问。
Java中static方法不能被覆盖，因为方法覆盖是基于运行时动态绑定的，而static方法是编译时静态绑定的。static方法跟类的任何实例都不相关，所以概念上不适用。



## 列举你所知道的Object类的方法并简要说明。

Object() —— 默认构造方法

clone() —— 创建并返回此对象的一个副本

equlas(Object obj) —— 比较两个对象的地址

finalize() —— 当垃圾回收器确定不存在对该对象的更多引用时，由对象的垃圾回收器调用此方法

hashCode() —— 返回该对象的哈希码值

notify() —— 唤醒线程

notifyAll() —— 唤醒所有线程

toString() —— 返回该对象字符串表示

wait() —— 线程等待



## 解释一下String为什么不可变？

String 不可变是因为在 JDK 中 String 类被声明为一个 final 类，且类内部的 value 字节数组也是 final 的。



## 讲讲wait方法的底层原理

wait方法是继承自Object类中的方法，当对象调用wait方法时，其实是让执行对象方法的当前线程等待（而不是对象等待），线程会被放入等待队列中并释放锁，只有当其他线程调用notify或者notifyAll方法时，这个线程才会从等待队列中被唤醒，然后重新去争用锁。



## 请说明List、Map、Set三个接口存取元素时，各有什么特点？

* List：可重复、有序
* Set：不可重复、无序
* Map：映射关系（一对一、多对一）



## 阐述ArrayList、Vector、LinkedList的存储性能和特性

ArrayList和Vector都是使用数组方式存储数据，此数组元素数大于实际存储的数据以便增加和插入元素，它们都允许直接按序号索引查询元素，适合查找不适合插入。

Vector中的方法由于添加了synchronized修饰，因此Vector是线程安全的容器，但性能上较ArrayList差。

LinkedList使用双向链表实现存储，查询数据需要进行前向或后向遍历，但是插入数据时只需要记录本项的前后项即可，所以插入速度较快。包含大量对首尾元素进行操作的方法。



## 请说明Collection 和 Collections的区别。

Collection是集合类的上级接口，继承与他的接口主要有Set 和List.
Collections是针对集合类的一个帮助类，他提供一系列静态方法实现对各种集合的搜索、排序、线程安全化等操作。



## 请说明ArrayList和LinkedList的区别？

ArrayList和LinkedList都实现了List接口，他们有以下的不同点：

* ArrayList是基于索引的数据接口，它的底层是数组。
  * O(1)时间复杂度对元素进行随机访问
  * 查找快，增删慢
* LinkedList是以元素链表的形式存储它的数据
  * O(n)。时间复杂度查找某个元素
  * 查找满，增删快
  * 比较占内存，每个节点存储了两个引用



## 说明HashMap和Hashtable的区别？ 

HashMap和Hashtable都实现了Map接口，他们有以下不同点：

* HashMap
  * 线程不安全
  * 适合单线程
  * 允许键与值为null
* Hashtable
  * 线程安全
  * 适合多线程
  * 不允许键与值为null



## 请你说说Iterator和ListIterator的区别？

Iterator和ListIterator的区别是：

* Iterator可用来遍历Set和List集合，但是ListIterator只能用来遍历List。
* Iterator对集合只能是前向遍历，ListIterator既可以前向也可以后向。
* ListIterator实现了Iterator接口，并包含其他的功能，比如：增加元素，替换元素，获取前一个和后一个元素的索引，等等。


![Image](https://github.com/Nihachi28/teachmyself/blob/main/28.jpg)



