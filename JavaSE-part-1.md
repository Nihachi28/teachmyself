# JavaSE Part1

------

## 关于Java的类

### 属性

#### 成员变量与局部变量

* **方法体外，类体内**声明的变量称为成员变量
* **方法体内部**声明的变量称为局部变量

![Image](C:\Users\79260\AppData\Local\Temp\Image.png)

* 实例变量：在类中实例化成对象后才可以使用
* 类变量（静态变量）：这样的变量**不需要实例化**成对象就可以使用，直接可以通过”**类名.属性**“的方式调用

![image-20210105203610229](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210105203610229.png)

### 方法

#### 类的访问机制

* 在**一个类**中的访问机制：类中的方法可以直接访问类中的成员变量（例外：static方法不能访问非static成员变量）
* 在**不同类**中的访问机制：先创建要访问类的对象，再用对象访问类中定义的成员。

#### 参数传递

Java方法的参数传递只有一种方式 —— **值传递**（副本传入方法，本身不受影响）

![image-20210105203838535](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210105203838535.png)

* 如果方法的形参是**基本数据类型**，那么实参向形参传递参数时，就是直接传递值，把**实参的值**复制给形参。
* 如果方法的形参是**对象**，那么实参向形参传递参数时，也是把值给形参，这个值是实参在栈内存中的值，也就是**引用对象在堆内存中的地址**。

#### this & super

##### this

> **this表示当前对象，可以调用类的属性、方法和构造器**

* 在方法内部使用，即这个**方法所属对象的引用**
* 在构造器内部使用，表示**该构造器正在初始化的对象**

**☆注意**

* 调用this()必须放在构造器的首行
* 构造器不能调用构造器自己，比如在person（）中写this（）保证至少有一个构造器不是用this的

```java
public class Person{
    public Person(){
    };
    
    public Person(int age){
        this.age = age;
    };

    public Person(String name){
        this();    //等同于调用了Person（）
        this.name = name;
    };

    public Person(int age,String name){
        this.age = age;  //当前这个对象的age.前面是成员变量，后面是形参
        this.name = name;
    }
    int age;
    String name;
    
    public void setName1(String name){
        this(1);    //等同于调用了Person(int age)
        this.name = name;
    }

    public void setName2String name(){
        this.setName1(name);
    } 
    
    public void showName(){
        System.out.println("Name is " + this.name);    
    }

}
```

##### super

在Java类中使用super来调用父类中的指定操作

* super可用于访问父类中定义的属性
* super可用于调用父类中定义的成员方法
* super可用于在子类构造方法中调用父类的构造器

**尤其当子父类出现同名成员时，可以用super进行区分**

调用父类的构造器：

* 子类中所有的构造器**默认**都会访问父类中**空参数**的构造器
* 当父类中没有空参数的构造器时，子类的构造器必须**通过this（参数列表）或者super（参数列表）语句指定调用本类或者父类中相应的构造器**，且必须放在构造器的第一行
* 如果子类构造器中既未显式调用父类或本类的构造器，且父类中又没有无参的构造器，则**编译出错**

```java
//该默认调用类似于
public class Kids entends ManKind{
    public Kids(){
        super();
    }
}
```



##### this与super的区别

| No.  | 区别点     | this                                           | super                                    |
| ---- | ---------- | ---------------------------------------------- | ---------------------------------------- |
| 1    | 访问属性   | 访问本类中的属性，**若没有则从父类中继续查找** | 访问父类中的属性                         |
| 2    | 调用方法   | 访问本类中的方法                               | 直接访问父类中的方法                     |
| 3    | 调用构造器 | 调用本类构造器，必须放在构造器的首行           | 调用父类构造器，必须放在子类构造器的首行 |
| 4    | 特殊       | 表示当前对象                                   | 无此概念                                 |



## Java编译过程

大致过程：发人员编写Java代码(.java文件)，然后将之编译成字节码(.class文件)，再然后字节码被装入内存，一旦字节码进入虚拟机，它就会被解释器解释执行，或者是被即时代码发生器有选择的转换成机器码执行。

（1）Java代码编译（Java编译器）

![image-20210105204717446](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210105204717446.png)

（2）Java字节码的执行（JVM执行引擎）

![image-20210105204807771](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210105204807771.png)

> 参考：https://www.cnblogs.com/fengyiliang/p/10030092.html

## JavaBean

> JavaBean是一种Java语言写成的可重用组件。

所谓javaBean，是指符合如下标准的Java类：

* 类是公共的
* 有一个无参的公共的构造器
* 有属性，属性一般是私有的，且有对应的get、set方法

用户可以使用JavaBean将功能、处理、值、数据库访问和其他任何可以用java代码创造的对象进行打包，并且其他的开发者可以通过内部的JSP页面、Servlet、其他JavaBean、applet程序或者应用来使用这些对象。用户可以认为JavaBean提供了一种随时随地的复制和粘贴的功能，而不用关心任何改变。（☆不破坏向后兼容性）

![image-20210105205158004](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210105205158004.png)

**形象解释：** https://www.zhihu.com/question/19773379

## 继承

![image-20210105205341475](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210105205341475.png)

```java
class Subclass extends Superclass{}
```

☆ 把共性的内容抽出，形成父类，实际需求的子类在继承父类的基础上写自己特有的代码：

* 提高代码的复用性
* 使类与类之间产生关系，提供了多态的前提
* 不要只是为了获取某个功能而去继承，要有逻辑关系（狗继承人类）

注意：

* 子类继承父类的属性与方法，但不能直接访问私有的（private）
* Java**只支持单继承**，不允许多重继承
  * 一个子类只能有一个父类
  * 一个父类可以派生多个子类



## 封装

> 对不能让调用者随意使用的属性做封装和隐藏

方法：

* 利用**private**关键字，把**属性声明为私有的**
* 通过编写public类型的setXxx()和getXxx()方法来设置属性和获取属性

```java
public class Student {
       private int age;

       public int getAge() {
              return age;
       }
       
       public void setAge(int i) {
              if(i<=0) {
                     System.out.println("错误输入");
              }
              else {
                     age = i;
                     System.out.println("年龄为"+age);
              }
       }
}
```



## 构造器

```java
修饰符 类名（参数列表）{
    初始化语句
}
```

☆ 根据参数不同，构造器可以分为如下两类：

* 隐式无参构造器（系统默认提供）
* 显式定义一个或多个构造器

☆ 注意：

* java语言中，每个类都至少有一个构造器
* 默认构造器的修饰符与所属类的修饰符一致
* 一旦显式定义了构造器，则系统不再提供默认构造器
* 一个类可以创建多个重载的构造器
  * 方便调用，可以灵活创建出不同需要的对象
  * 重载多个构造方法实际上就是给了不同的初始化模板
* **父类的构造器不可被子类继承**

## 修饰符

> Java权限修饰符置于类的成员顶以前，用来限定对象对该类成员的访问权限

| 修饰符    | 类内部 | 同一个包 | 子类 | 任何地方 |
| --------- | ------ | -------- | ---- | -------- |
| private   | Yes    |          |      |          |
| （缺省）  | Yes    | Yes      |      |          |
| protected | Yes    | Yes      | Yes  |          |
| public    | Yes    | Yes      | Yes  | Yes      |

☆ 对于**class的权限修饰**只可以用public和default（缺省）

* public类可以在任意地方被访问
* default类只可以被**同一个包内部的类访问**

☆ 同一个java文件中写多个的class，但是只有一个可用public的，其他的class只能用default（缺省的）

## 关键字

### final

> 在Java中声明类、属性和方法时，可使用关键字final来修饰,表示“最终，不可改变的”

* final标记的类**不能被继承**。
  * 提高安全性，提高程序的可读性。
* final标记的**方法不能被子类重写**。
  * 比如Object类中的getClass()
* final**标记的变量(成员变量或局部变量)即称为常量**。
  * 名称大写，且只能被赋值一次，不能再改变
  * final修饰的变量是常量所以必须显示赋值
  * final和static一起修饰，就是全局常量

```java
final int nun2 = 200;
```

☆ 对于基本类型来说，不可改变说的是变量中的**数据不可改变**

☆ 对于引用类型来说，不可改变说的是变量当中的**地址值不可改变**

```java
final Student stu2 = new Student("Tom");
System.out.println(stu2.getName());	//Tom
stu2.setName("Jerry");
System.out.println(stu2.getName());	//Jerry
```



### static

> static要解决的问题 —— 对于一个类，创建不同对象时却对某个属性赋予相同的值，重复操作过多

```java
public class Chinese{
    Static String country;    //类变量，直接类名.属性名就可以访问，是类的一部分，被所有这个类的实例化对象所共享，也可以叫静态变量
    String name;    //实例变量，只有实例化之后才能使用，属于实例化对象的一部分，不能共有
    int age;    //实例变量，只有实例化之后才能使用，属于实例化对象的一部分呢
}

Chinese.country = "中国";	//可以直接使用
Chinese c = new Chinese;
Chinese c1 = new Chinese;
System.out.println("Chinese.country");
```

☆ 在设计类时，**不希望某个属性因为对象的不同而改变**，将这些属性设置为类属性 | 相应的方法设置为类方法

☆ **方法与调用者无关**，则这样的方法通常被声明为**类方法**

☆ **类方法（静态方法）**通常被用来做**工具类**

```java
//判断String是否为空字符串，经常使用，所以可以做一个工具类
public static boolean isEmpty(String s){
        boolean flag = false;
    if((s!=null)&&(!s.equals("")){
        flag = ture;
    }
    return flag;
}
```

被修饰后的成员具备以下特点：

* 随着类的加载而加载
  * 类加载后就可以直接用了（类名.方法/属性名）
* 优先于对象存在
  * 没new的时候就可以使用
* 修饰的成员，被所有对象所共享
  * 比如说可以用来做计数，new了多少个对象
* 访问权限允许时，可不创建对象，直接被类调用
  * 一般都是public修饰

☆ 可以被所有的实例化对象都共享的属性，使用起来必须慎重。**因为只要改变，所有类都改变。**

☆ 因为不需要实例化就可以访问static方法，因此static方法**内部不能有this、super（this和super指的是对象，不是类）**

![image-20210105214652985](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210105214652985.png)

**静态不能访问非静态**：在内存当中是先有静态内容，后有非静态内容。

#### static属性的内存图

![image-20210105214841916](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210105214841916.png)

#### 静态代码块

特点：当第一次用到本类，静态代码块执行唯一的一次。

==静态代码块比构造方法先执行==

一般用于：一次性地对静态成员变量进行赋值

静态代码块（用static 修饰的代码块）：

* 可以有输出语句。
* 可以对类的属性声明进行初始化操作。
* 不可以对非静态的属性初始化。即：不可以调用非静态的属性和方法。
* 若有多个静态的代码块，那么按照从上到下的顺序依次执行。
* 静态代码块的执行要先于非静态代码块。
* 静态代码块只执行一次。

```java
public class Person{
    String name;
    static Test tp = new Test();
    
    static{
        tp.name = "Tom";
        tp.age = 18;
    }
}
```

☆ 在匿名内部类中，用代码块代替构造方法（没有类，没法通过类.属性来构造）

![image-20210105215706322](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210105215706322.png)

## 对象

### 对象的产生

| 成员变量类型       | 初始值             |
| ------------------ | ------------------ |
| byte               | 0                  |
| short              | 0                  |
| int                | 0                  |
| long               | 0L                 |
| float              | 0.0F               |
| double             | 0.0D               |
| char               | '\u0000'(标识为空) |
| boolean            | false              |
| 引用类型（String） | null               |

### 匿名对象

**不定义对象的句柄，而直接调用这个对象的方法**。这样的对象叫做匿名对象。

```java
new Person().shout()
```

使用情况

* 如果对一个对象只需要进行一次方法调用，那么就可以使用匿名对象
* 我们经常将匿名对象作为实参传递给一个方法调用

## 数据类型

### 基本数据类型

8种基本数据类型：

* 整数类型：long、int、short、byte
* 浮点类型：float、double
* 字符类型：char
* 布尔类型：boolean

### 引用数据类型

引用数据类型有很多，大致如下：类、 接口类型、 数组类型、 枚举类型、 注解类型、 字符串型例如，**String** 类型就是引用类型

#### 基本数据类型和引用数据类型之间的区别

1. 存储位置

   * 基本变量类型
     * 在方法中定义的非全局基本数据类型变量的**具体内容是存储在栈中的**
   * 引用变量类型
     * 只要是引用数据类型变量，其**具体内容都是存放在堆中的**，而栈中存放的是其**具体内容所在内存的地址**

   ![image-20210105212148037](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210105212148037.png)

2. 传递方式

* 基本变量类型

  * 在方法中定义的非全局基本数据类型变量，调用方法时作为参数是**按数据传递的**

    ![image-20210105212403780](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210105212403780.png)

* 引用变量类型

  * 引用数据类型变量，调用方法时作为**参数是按引用传递**的

    ![image-20210105212510819](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210105212510819.png)

### 数据类型的强制转化

在需要转型的数据前加上“( )”，然后在括号内加入需要转化的数据类型。有的数据经过转型运算后，**精度会丢失，而有的会更加精确**

1. 两种类型是彼此兼容的
2. 转换的目的类型占得空间范围一定要大于转化的源类型

正向过程：由低字节向高字节自动转换

> byte->short->int->long->float->double

逆向过程：使用强制转换,可能丢失精度。

```java
int a=(int)3.14;
```

### 数据类型的自动转换

Java定义了若干使用于表达式的类型提升规则：

1. 所有的byte型. short型和char型将被提升到**int**型(例外: final修饰的short, char变量相加后不会被自动提升。)
2. 如果一个操作数是**long**形 计算结果就是**long**型;
3. 如果一个操作数是**float**型，计算结果就是**float**型;
4. 如果一个操作数是**double**型，计算结果就是**double**型;



## 多态

### 方法的多态性

#### 重载（overload）

#### 重写（override）

在子类中可以根据需要对从父类中继承来的方法进行改造，也称方法的重置、覆盖。在程序执行时，子类的方法将覆盖父类的方法。

要求：

* 重写方法必须和被重写方法具有**相同的方法名称、参数列表和返回值类型**
  * 只是重写方法体的代码
* 重写方法不能使用比被重写方法更严格的访问权限
  * 父类public，子类不能是default或者private
* 重写和被重写的方法必须同时为static，或同时非static的
* 子类方法抛出的异常不能大于父辈被重写方法的异常

### 对象的多态性

首先，对象的多态性主要表现在抽象类和接口上。



Java引用变量有两个类型：**编译时类型和运行时类型**。

* 编译时类型由声明该变量时使用的类型决定，运行时类型由实际赋给该变量的对象决定。
* 若编译时类型和运行时类型不一致，就出现多态

（1）关于继承后父类与子类的同名方法：（看new的等号右边是谁，没有则向上找）

![image-20210105220610350](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210105220610350.png)

（2）关于继承后父类与子类的同名属性：（看new的等号左边是谁，没有则向上找）

![image-20210105220809147](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210105220809147.png)

![image-20210105220821830](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210105220821830.png)

* 一个变量只能有一种确定的数据类型
* 一个**引用类型**变量可以指向（引用）多种不同类型的对象

```java
Person p = new Person();
Student e = new Student();
//以上为正常情况

Person s = new Student( );    //父类的引用对象可以指向子类的实例

Person p = new Person();
p = new Student();
//当前这个引用对象p引用的是Student实例对象
```

☆ 一个引用类型变量如果**声明为父类的类型**，但实际引用的却是子类对象，那么该变量就**不能再访问子类中添加的属性（方法可以）**

```java
Student m = new Student();
m.school = "pku";    //合法
Person e = new Student();
e.school = "pku";    //非法
```

#### 子类的向上转型与向下转型

![image-20210106095436308](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210106095436308.png)

![image-20210106095523680](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210106095523680.png)

#### 转型后方法的调用

![image-20210106095710742](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210106095710742.png)

如何判断父类引用的对象，本来是什么子类？

![image-20210106095827854](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210106095827854.png)

实例：

![image-20210106095903041](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210106095903041.png)

正常方法调用与虚拟方法调用：

```java
//正常方法调用
Person p = new Person();
p.getInfo();
Student s = new Student();
s.getInfo();

//虚拟方法调用
Person e = new Student();
e.getInfo();    //调用Student类的getInfo()方法
```

🙋‍ 编译e为Person类型，而**方法的调用是在运行时确定**的，所以调用的是Student类的getInfo（）方法 —— **动态绑定**

![image-20210106100120028](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210106100120028.png)

子类继承父类：

* 若子类重写了父类方法，就意味着子类里定义的方法彻底覆盖了父类里的同名方法，系统将不可能把父类里的方法转移到子类中
* 对于实例变量则不存在这样的现象，即使子类里定义了与父类完全相同的实例变量，这个实例变量依然不可能覆盖父类中定义的实例变量

![image-20210106100650419](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210106100650419.png)

#### 抽象类

> 随着继承层次中一个个新子类的定义，类变得越来越具体，而父类则更一般，更通用。类的设计应该保证父类和子类能够共享特征。
>
> 有时将一个父类设计得非常抽象，以至于它没有具体的实例，这样的类叫做抽象类。

* 用abstract关键字来修饰一个类时，这个类叫做抽象类
  * 含有抽象方法的类必须被声明为抽象类
* 用abstract来修饰一个方法时，该方法叫做抽象方法
  * 只有方法的声明，没有方法的实现。以分号结束：abstract int abstractMethod( int a );
* 抽象类不能被实例化。
  * 抽象类是用来作为父类被继承的，抽象类的子类必须重写父类的抽象方法，并提供方法体。**若没有重写全部的抽象方法，仍为抽象类。**
* 不能用abstract修饰属性、私有方法、构造器、静态方法、final的方法。

> 总之，抽象类是用来模型化那些父类无法确定全部实现，而是由其子类提供具体实现的对象的类

```java
public abstract class Employee {
       public Employee(){
              
       }
       String name;
       String id;
       int salary;
       
       public abstract void work();
}
```

```java
public class Manager extends Employee{
       int bonus;
       
       public void setCommonEmployeeInfo(String name,String id,int salary,int  bonus) {
              super.id = id;
              super.name = name;
              super.salary = salary;
              this.bonus = bonus;
       }
       
       public void getCommonEmployeeInfo() {
              System.out.println(super.id);
              System.out.println(super.name);
              System.out.println(super.salary);
              System.out.println(this.bonus);
       }      
       
       @Override
       public void work() {
              System.out.println("Manager");
              
       }
       
}
```

#### 接口

> 有时必须从几个类中派生出一个子类，继承它们所有的属性和方法。但是，Java不支持多重继承。有了接口，就可以得到多重继承的效果。

接口(interface)是抽象方法和常量值的定义的集合。

从本质上讲，接口是一种特殊的抽象类，这种抽象类中只包含常量和方法的定义，而没有变量和方法的实现。

一个类可以实现多个接口，接口也可以继承其它接口。

```java
class SubClass implements InterfaceA{ }
```

接口可以定义抽象方法

```java
public abstract 返回值类型 方法名(参数列表)；
```

注意：

1. 接口当中的抽象方法，修饰符必须是public abstract
2. 接口中的所有成员变量都默认是由public static final修饰的
3. 可以选择性省略这两个修饰符

接口的使用步骤：

1. 接口必须有一个“实现类”来“实现”该接口

   ```java
   public class 实现类名称 implements 接口名称{
   	...
   }
   ```

2. 接口的实现类必须覆盖重写（实现）接口中的所有抽象方法

   * 去掉abstract关键字，加上方法体大括号

3. 创建实现类的对象，进行使用

注意：如果实现类没有覆盖重写接口中所有的抽象方法，则实现类本身必须是抽象类



##### 接口升级问题

> 当接口中新增了方法，如果是抽象方法，则以前实现过该接口的所有类都会变成抽象类，所以使用**默认方法**可以解决该问题

```java
public default 返回值类型 方法名称(参数列表){
	方法体
}
```

```java
public interface MyInterfaceDefault{
    //抽象方法
	public abstract void methodAbs();
    //新增抽象方法
    public default void methodDefault(){
        System.out.println("New default method");
    }
}
//当调用默认方法时，如果实现类当中没有，则会向上找接口
/*
1. 接口的默认方法，可以通过接口实现类对象，直接调用
2. 接口的默认方法，也可以呗接口实现类进行覆盖重写
*/
```



##### 接口中定义静态方法

```java
public interface MyinterfaceStatic{
	public static void methodStatic(){
		System.out.println("This is static method");
	}
}
/*
☆ 不可以通过接口实现类来调用接口中的静态方法 ☆
而必须通过接口名称，直接调用其中的静态方法
接口名称.静态方法名(参数)
*/
```

##### 接口中定义私有方法

> Java9开始接口当中允许定义私有方法
>
> 若需要抽取一个共有方法，私有方法解决了两个默认方法之间重复代码的问题，共有方法不让实现类使用

1. 普通私有方法：解决多个默认方法之间重复代码问题

   ```java
   private 返回值类型 方法名称(参数列表){
   	方法体
   }
   ```

2. 静态私有方法：解决多个静态方法之间重复代码问题

   ```java
   private static 返回值类型 方法名称(参数列表){
   	方法体
   }
   ```

   

### 多态总结

前提：

* 需要存在**继承或者实现**关系
* 要有**覆盖操作**

成员方法

* 编译时：要查看引用变量所属的类中是否有所调用的方法
* 运行时：调用**实际对象所属的类中的重写方法**

成员变量

* 不具备多态性，只看引用变量所属的类



## 内部类

> 在Java中，允许一个类的定义位于另一个类的内部，前者称为内部类，后者称为外部类。

```java
public class TEST1{
    int i;
    public int z;
    private int k;
    
    //内部类 A
    class A{
        public void setTestFileds(){
               TEST1.this.i = 1;
               TEST1.this.z = 2;
               TEST1.this.k = 3;
        }
    }
    
    public void setInfo(){
//        new A().setTestFileds(); //错误写法
       A a = new A();    //外部类想要使用内部类，需要先实例化
       a.setTestFileds();
    }
    public void showInfo(){
        System.out.println(this.i);
        System.out.println(this.z);
        System.out.println(this.k);
    }
    public static void main(String[] args) {
              TEST1 t = new TEST1();
              t.setInfo();
              t.showInfo();
       }
}
```

Inner class作为类的成员：

* 可以声明为final的
* 和外部类不同，Inner class可声明为private或protected
* Inner class 可以声明为static的，但此时就不能再使用外层类的非static的成员变量

Inner class作为类：

* 可以声明为abstract类 ，因此可以被其它的内部类继承

【注意】非static的内部类中的成员不能声明为static的，只有在外部类或static的内部类中才可声明static成员。

🙋‍ 内部类主要**为了解决JAVA不能多重继承的问题**

```java
public class TEST1{
       public static void main(String[] args) {
              new A().testB();
              new A().testC();
       }
}
//目标让A获得B与C的方法并且重写
class A{
       public void testB() {
              new InnerB().testB();
       }
       
       public void testC() {
              new InnerC().testC();
       }
       
       //继承B，重写B内方法
       private class InnerB extends B{
              @Override
              public void testB() {
                     System.out.println("TESTB");
              }
       }
    
       //继承C，重写C内方法
       private class InnerC extends C{
              @Override
              public void testC() {
                     System.out.println("TESTC");
              }
       }
       
       
}
class B{
       public void testB() {
           ...    
       }
}
class C{
       public void testC() {
           ...   
       }
}

```

```java
/*
内用外，随意访问；外用内，需要内部类对象
使用方式（2种）：
1. 间接方式：在外部类的方法中，使用内部类，然后main只是调用外部类的方法
2. 直接方式：
类名称 对象名 = new 类名称()；
外部类名称。内部类名称 对象名 = new 外部类名称().new 内部类名称
*/

public class Demo01InnerClass{
    public static void main(String[] args){
        Body body = new Body();	//外部类对象
        //通过外部类的对象，调用外部类的方法，里面间接再使用内部类Heart
        body.methodBody();
    }
}

public class Body{
    class Heart{
        ...
    }
    public void methodBody(){
        Heart h = new Heart;
        h.userHeart();
    }
}
```

内部类与外部类属性同名的情况：

![image-20210106104710969](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210106104710969.png)

局部内部类：

![image-20210106104835657](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210106104835657.png)

局部内部类中使用局部变量的final问题

```java
/*
如果希望访问所在方法的局部变量，那么这个局部变量必须是“有效final”
原因：
1. new出来的对象在堆内存中
2. 局部变量是跟着方法走的，在栈内存中
3. 方法运行结束之后，立刻出栈，局部变量就会立刻消失
4. 但是new出来的对象会在堆当中持续存在，直到垃圾回收消失
*/
public class MyOuter{
    public void methodOuter(){
        final int num = 10;	
        //new出来的MyInner生命周期会比栈种的变量num久，如果使用num而num已经被垃圾回收，则无法使用，所以使用的过程中其实会copy一下num的内容，如果num发生变化，则失效
        
        class MyInner{
            public void methodInner(){
                System.out.println(num);
            }
        }
    }
}

```

关于权限修饰符的选择：

1. 外部类：public / default
2. 成员内部类：public / protected / default / private
3. 局部内部类：什么都不写

### 匿名内部类

```java
public class DemoMain{
    public static void main(String[] args){
        MyInterface obj = new MyInterface(){
            @Override
            public void method(){
                System.out.println("匿名内部类实现了方法")；
            }
        }
    }
}
```

使用匿名内部类，替代接口的实现类（减少了麻烦，不需要为了一个接口再写一个实现类，但是使用匿名内部类，只能使用一次）

![image-20210106110344135](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210106110344135.png)

总结：

1. new代表创建对象的动作
2. 接口名称就是匿名内部类需要实现哪个接口
3. {...}这才是匿名内部类的内容

注意：

1. 匿名内部类，在创建对象的时候，只能使用唯一一次；如果想要多次创建对象，且类的内容一致，那么就必须使用单独定义的实现类
2. 匿名对象，在调用方法的时候，只能调用唯一一次；如果想要同一个对象，调用多次方法，那么必须给对象起个名字
3. 匿名内部类是省略了实现类/子类，但是匿名对象是省略了对象名称



## 包装类

> 针对八种基本定义的引用类型 —— 包装类（封装类）
>
> 有了类的特点，就可以调用类中的方法

![image-20210106110822890](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210106110822890.png)

| 基本数据类型 | 包装类    |
| ------------ | --------- |
| boolean      | Boolean   |
| byte         | Byte      |
| short        | Short     |
| int          | Integer   |
| long         | Long      |
| char         | Character |
| float        | Float     |
| double       | Double    |

### 装箱

> 把基本类型的数据，包装到包装类种（基本类型的数据 -> 包装类）

```java
//构造方法如下：
Integer(int value)	//构造一个新分配的Integer对象，他表示指定的int值
Integer(String s)	//构造一个新分配的Integer对象，他表示指定的String值所指示的int值
    
//静态方法如下：
static Integer valueOf(int i)	//返回一个表示指定的int值得Integer实例
static Integer valueOf(String s)	//返回保存指定得String值得Integer对象
```

```java
//通过包装类的构造器实现：
int i = 500;   
Integer t = new Integer(i);

//通过字符串参数构造：
Float f = new Float(“4.56”);
Long l = new Long(“asdf”);    //NumberFormatException
```

### 拆箱

> 在包装类中取出基本类型的数据（包装 -> 基本类型的数据）

```java
//成员方法如下：
int intValue()	//以int类型返回该Integer的值，其他类型类似
```



```java
//调用包装类的.xxxValue()方法
boolean b = new Boolean("false").booleanValue();
```

JDK1.5之后，支持自动装箱，自动拆箱。但类型必须匹配。

```java
Interger i1 = 112;    //自动装箱
int i2 = i1;    //自动拆箱
```

### 字符串与基本数据类型的直接转换

#### 字符串转换成基本数据类型

* 通过包装类的构造器实现：
* 通过包装类的parseXxx（String s）静态方法：

```java
  int i = new Integer(“12”);
  Float f = Float.parseFloat(“12.1”);
```

#### 基本数据类型转换成字符串

```java
String fstr = String.valueOf(2.34f);
String intStr = 5 + “”；	//更直接的方式
```



## 异常处理

> 捕获错误最理想的是在编译期间，但有的错误只有在运行时才会发生。
>
> 对于这些错误，一般有两种方法：
>
> * 遇到错误就终止程序的运行
> * 由程序员在编写程序时，就考虑到错误的检测、错误消息的提示，以及错误的处理

Java程序运行过程中所发生的异常事件可分为两类：

* Error: JVM系统内部错误、资源耗尽等严重情况
* Exception: 其它因编程错误或偶然的外在因素导致的一般性问题，例如：
  1. 空指针访问
  2. 试图读取不存在的文件
  3. 网络连接中断
  4. ...

![image-20210106131514622](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210106131514622.png)

### 异常处理机制 —— 抓抛模型

```java
//try-catch
int i = 0;
try{
    System.out.println(3);    //会被执行
    System.out.println(3/i);
    System.out.println(2);    //上面报错，不会执行
}catch(Exception e){
    e.printStrackTrace;
    System.out.println(e.getMessage());
}finally{    //可写可不写
    System.out.println(1);
}
```

```java
//连续捕获
//捕捉可能出现的异常
//在捕捉异常的代码块中，如果前面的代码有异常了就不会执行后面了
try{
    错误1；
    错误2；
}catch(ArrayIndexOutOfBoundsException e1){
    Systemout...;
}catch(NullPointerException e2){
    Systemout...;
}
```

```java
//throw
//子类重写父类方法时，子类不能抛出比父类方法更大范围的异常
public void readFile(String file)  throws FileNotFoundException {
  // 读文件的操作可能产生FileNotFoundException类型的异常
  FileInputStream fis = new FileInputStream(file);
}
```

```java
//人工抛出异常
int age;
public void test(int age) throws Exception{
    if(age>=0 && age <=150){
        this.age = age;
        System.out.println("年龄是：" + this.age);
    }else{
        throw new Exception("年龄在0到150之间");
    }
}
```

```java
//创建用户自定义异常类
public class MyException extends Exception{
       private int idnumber;
       public MyException(String message, int id) {
              super(message);
              this.idnumber = id;
       }      
}
int age;
public void TEST2(int age) throws MyException{
       if(age <=150 && age>=0) {
              this.age = age;
       }else {
              throw new MyException(message, id)
       }
}
//Java所提供的异常类一般够用，不太会自定义建立异常类
```

### finally代码块

> 有一些特定的代码无论异常是否发生，都需要执行
>
> 另外，有一些代码发生异常后产生跳转，有的代码执行不到

```java
try{
	可能产生异常的代码
}catch(定义一个异常的变量，用来接收try中抛出的异常){
    异常的处理逻辑
    一般工作中会把异常信息记录到一个日志中
}finally{
    无论是否发生异常，都会执行
}
/*
注意事项：
1. finally不能单独使用，必须和try一起使用
2. finally一般用于资源释放（资源回收），不论程序是否出现异常，最后都要资源释放
*/

```



## 多线程

进程(process)是程序的一次执行过程，或是正在运行的一个程序。动态过程：有它自身的产生、存在和消亡的过程。**进程是系统进行资源分配和调度的基本单位。**

* 如：运行中的QQ，运行中的MP3播放器
* 程序是静态的，进程是动态的

---------------------

线程(thread)，进程可进一步细化为线程，是一个程序内部的一条执行路径。若一个程序可同一时间执行多个线程，就是支持多线程的。**线程是操作系统能够进行运算调度的最小单位。**一个进程至少有一个线程。

🙋‍ 集合只要是不安全的，就是多线程

--------------

并发与并行

* 并发：指两个或多个事件在同一个时间段内发生

  ![image-20210106145441281](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210106145441281.png)

* 并行：指两个或多个事件在同一时刻发生（同时发生）

  ![image-20210106145512976](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210106145512976.png)

------------------

### 线程调度

* 分时调度
  * 所有的线程轮流使用CPU的使用权，平均分配每个线程占用CPU的时间
* 抢占式调度
  * 优先让优先级高的线程使用CPU，如果优先级相同，那么随机选择一个

-------------

### 主线程

> 执行主方法的线程

单线程程序：指java程序中只有一个线程

执行从main方法开始，从上到下依次执行

![image-20210106151135612](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210106151135612.png)

### 线程类

#### 创建线程类

```java
//java.lang.Thread
/*
1. 创建一个Thread类的子类
2. 在Thread类的子类中重写Thread类中的run方法，设置线程任务
3. 创建Thread类中的子类对象
4. 调用Thread类中的方法start方法，开启新的线程执行run方法
	void start():java虚拟机调用该线程的run方法
	结果是两个线程并发运行，当前线程（main线程）和另一个线程（创建新的线程，执行其run方法）
	多次启动一个线程是非法的，特别是线程已经结束执行后，不能再重新启动
	java属于抢占式调度，优先级高的先执行，一样高随机执行
*/

public class MyThread extends Thread{
    @Override
    public void run(){
        for(int i = 0; i < 20; i++){
            System.out.println("执行第"+i+"次");
        }
    }
}

/*————————————————————————————————————————————————————————————*/

//创建子类对象
public class DemoThread{
    public static void main(String[] args){
        MyThread mt = new Thread();
        mt.start();		
        
        for(int i = 0; i < 20; i++){
            System.out.println("main"+i);
        }
        //两个线程交替运行
    }
}
```

![image-20210106161708569](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210106161708569.png)

多线程内存图解：

![image-20210106162118980](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210106162118980.png)

#### 使用实现Runnable创建线程类

> Runnbale接口应该由那些打算通过某一线程执行其实例的类来实现。
>
> 类必须定义一个成为run的无参数方法。

```java
//Thread的两种构造方法
Thread(Runnable target)
Thread(Runnable target, String name)
```

```java
//实现Runnbale接口
/*
实现步骤：
1. 创建一个Runnable接口的实现类
2. 在实现类中重写Runnable接口的run方法，设置线程任务
3. 创建一个Runnable接口的实现类对象
4. 创建Thread类对象，构造方法中传递Runnable接口的实现类对象
5. 调用Thread类中的start方法，开启新的线程执行run方法
*/

public class RunnableImpl implements Runnable{
    @Override
    public void run(){
        for(int i = 0; i < 20; i++){
           System.out.println(Thread.currentThread().getName()+"i");
        }
    }
}

/*---------------------------------------------*/

public class DemoRunnable{
    public static void main(String[] args){
        //创建一个Runnable接口的实现类对象
        RunnableImpl run = new RunnableImpl();
        Thread t = new Thread(run);
        t.start;
        
        for(int i = 0; i < 20; i++){
           System.out.println(Thread.currentThread().getName()+"i");
        }
    }
}

```

#### 两种创建线程类方法的区别

实现Runnable接口创建多线程程序的好处：

1. 避免了单继承的局限

   类继承了Thread类则不能继承其他的类，而实现了Runnable几口，还可以继承其他类，还可以实现其他接口

2. 增强了程序的扩展性，降低了程序的耦合性

   实现类中重写了run方法：用来设置线程任务

   而创建Thread类对象，调用start方法：用来开启新的线程

   方便创建新的实现类来实现不同的run方法，传递不同的实现类实现不同的内容

#### 匿名内部类实现现成的创建

```java
public class DemoRunnable{
    public static void main(String[] args){
        //线程的父类是Thread
        new Thread(){
            //重写run方法
            @Override
            public void run(){
                for(int i = 0; i < 20; i++){
                    System.out.print(Thread.currentThread().getName()+"--1");
                }
            }
        }
        
        //实现runnable接口
        //利用多态Runnbale r = new RunnableImpl();
        Runnable r = new Runnable(){
            //重写run方法，设置线程任务
            @Override
            public void run(){
                for(int i = 0; i < 20; i++){
                    System.out.print(Thread.currentThread().getName()+"--2");
                }
            }
        }
        new Thread(r).start();
        
        //Runnable实现的简化方法
        new Thread(new Runnable(){
            @Override
            public void run(){
                for(int i = 0; i < 20; i++){
                    System.out.print(Thread.currentThread().getName()+"--2");
                }
            }
        }).start();
    }
}
```



### Thread类常用方法

```java
public String getName()	//获取当前线程名
public void start()	//JVM调用此线程的run方法
public void run()	//此线程要执行的任务，在此处定义代码
public static void sleep(long millis)	//是当前正在执行的线程以指定毫秒数暂停（暂停执行）
public static Thread currentThread()	//返回对当前正在执行的线程对象的引用
```

```java
public class MyThread extends Thread{
    @Override
    public void run(){
        String name = getName();
        System.out.println(name);
        
        Thread t = Thread.currentThread();	//静态方法，也可以返回名字
        String name = t.getName();
        System.out.println(name);
        //Thread.currentThread().getName();
    }
}
/*----------------------------------------------------*/
/*线程的名称：
	主线程：main
	新线程：Thread-0，Thread-1，Thread-2
*/
public class DemoThread{
    public static void main(String[] args){
        MyThread mt = new Thread();
        mt.start();		
        
        new MyThread().start();
    }
}
```

#### 设置线程的名称

```java
/*
设置线程的名称（了解）：
1. 使用Thread类中的方法setName(名字)
	void setName(String Name)
2. 创建一个带参数的构造方法，参数传递线程的名称，调用父类的带参构造方法，把线程名称传递给父类，让父类Thread给子线程起一个名字
*/
public class MyThread extends Thread{
    //第二种方法
    public MyThread(){}
    public MyThread(String Name){
        super(name);
        //把线程名称传递给父类，让父类（Thread）给子类起一个名字
    }
    
    @Override
    public void run(){
        //获取线程的名称
        System.out.println(Thread.currentThread().getName());
        
    }
}
/*----------------------------------------------*/
public class DemoSetName{
    public static void main(String[] args){
        //开启多线程
        MyThread mt = new MyThread();
        mt.setName("Thread-No.1");	//第一种方法
        mt.start;
        
        //开启多线程
        new MyThread("Thread-No.2").start();	//第二种方法
    }
}


```

#### 暂停线程的执行

```java
//毫秒数结束后，线程继续执行
public class DemoSetName{
    public static void main(String[] args){
        for(int i = 1; i <= 60; i++){
            System.out.print(i);
            //使用Thread类的sleep方法让程序睡眠1秒钟
            try{
                Thread.sleep(1000);
            }catch(InterruptedException e){
                e.printStackTrace();
            }
            
        }
    }
}
```

### 线程同步机制

#### 线程安全问题

🙋‍ **原因：多线程访问了共享数据**

![image-20210106205821538](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210106205821538.png)

解决线程安全问题的方法：

1. 同步代码块
2. 同步方法
3. 锁机制

--------------------

#### 同步代码块

```java
synchronized(同步锁){
	需要同步操作的代码
}
```

```java
/*
注意：
1. 通过代码块的锁对象，可以使用任意对象
2. 但是必须保证多个线程使用的锁对象是同一个
3. 锁对象作用：把同步代码块锁住，只让一个线程在同步代码块中执行
*/

public class RunnableImp implements Runnable{
    //定义一个多个线程共享的票源
    private int ticket = 100;
    
    //创建一个锁对象，要写在run方法外边
    Object obj = new Object();
    
    @Override
    public void run(){
        while(true){
            Sybcgribuzed(obj){
                //票是否存在
            	if(ticket>0){
                	System.out.println(Thread.currentThread().getName()+"正在卖第"+ticket+"张票")；
                	ticket--;
            	}
            }
        }
    }
}
/* main方法中有三个线程 */
```

同步技术的原理：

> 使用了一个锁对象，这个锁对象叫同步锁，也叫对象锁，也叫对象监视器

* 当一个线程得到了CPU执行权，遇到synchronized代码块，会检查代码块是否有锁对象
  * 如果有，则会获取锁对象，并进入到同步中执行
  * 如果没有，则会进入到阻塞状态，等到上一个获得锁对象的线程归还锁对象，该线程获得锁对象才能进入同步中执行

🙋‍ **总结：同步中的线程，没有执行完毕不会释放锁，同步外的线程没有锁进不去同步**

✅ 给通过同步保证了只能由一个线程在同步中执行共享数据，保证了安全

❌ 查程序频繁的判断锁，获取锁，程序的效率会降低

--------------------

#### 同步方法

使用步骤：

1. 把访问了共享数据的代码抽取出来，放到一个方法中
2. 在方法上添加一个synchronized修饰符

```java
修饰符 synchronized 返回值类型 方法名(参数列表){
	可能会出现线程安全问题的代码（访问了共享数据的代码）
}
```

```java
//同步方法
/*
定义一个同步方法，同步方法也会把方法内部的代码锁住，只让一个线程执行
同步方法的锁对象是谁？是实现类对象 new RunnableImpl()，也就是this
	其实和利用同步代码块然后把this丢进去作为锁对象是一个道理
*/
public synchronized void payTicket(){
    if(ticket>0){
		System.out.println(Thread.currentThread().getName()+"正在卖第"+ticket+"张票")；
		ticket--;
    }
}
/*----------------------------------*/
public class RunnableImp implements Runnable{
    //定义一个多个线程共享的票源
    private int ticket = 100;
    
    @Override
    public void run(){
        while(true){
            payTicket();	//调用同步方法
        }
    }
}
/* main方法中有三个线程 */
```



##### 静态同步方法

```java
/*
静态方法的锁对象不能再是this了，因为this是创建对象之后产生的，静态方法优先于对象
静态同步方法的锁对象是本类的class属性 --> class文件对象（反射）
*/

public static synchronized void payTicketStatic(){
    if(ticket>0){
		System.out.println(Thread.currentThread().getName()+"正在卖第"+ticket+"张票")；
		ticket--;
    }
}
/*----------------------------------*/
public class RunnableImp implements Runnable{
    //定义一个多个线程共享的票源
    private static int ticket = 100;
    
    @Override
    public void run(){
        while(true){
            payTicket();	//调用同步方法
        }
    }
}
/* main方法中有三个线程 */
```



#### 锁机制（Lock锁）

> java.util.concurrent.locks.lock
>
> Lock实现提供了比使用synchronized方法和语句更为广泛的锁定操作。

```java
void lock()		//获取锁
void unlock()	//释放锁
```

使用步骤：

1. 在成员位置创建一个lock的实现类对象ReentranLock对象
2. 在可能出现安全问题的代码前调用Lock接口中的方法lock获取锁
3. 在可能出现安全问题的代码后调用Lock接口中的方法unlock获取锁

```java
public class RunnableImp implements Runnable{
    //定义一个多个线程共享的票源
    private int ticket = 100;
    
    //创建一个ReentranLock对象
    Lock l = new ReentranLock();
    
    @Override
    public void run(){
        while(true){
            l.lock();		//获取锁
            if(ticket>0){
                System.out.println(Thread.currentThread().getName()+"正在卖第"+ticket+"张票")；
                ticket--;
            }
            l.unlock();		//释放锁
        }
    }
}
/* main方法中有三个线程 */
```

```java
/*
更好的方法是把unlock()方法放在finally中使用
*/
l.lock();
...
try{
    ...
}catch(...){
    ...
}finally{
    l.unlock();
}
```



### 线程状态

线程共有六种状态：

1. 新建状态
2. 运行状态
3. 阻塞状态
4. 计时等待状态
5. 无限等待状态
6. 死亡状态

线程状态转换图

![image-20210106220656236](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210106220656236.png)

-------------------------

#### TIMED_WAITING（计时等待状态）

![image-20210106220935739](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210106220935739.png)

#### BLOCKED（锁阻塞状态）

![image-20210106221024553](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210106221024553.png)

#### WAITING（无限等待状态）

```java
/*
	void wait():其他线程调用此对象的notify()方法或notifyAll()方法前，导致先前线程等待
	void notify():唤醒在此对象监视器上等待的单个线程，会继续执行wait方法后的代码
*/
public class DemoWaitAndNotify{
    public static void main(String[] args){
        //创建锁对象，保证唯一
        Object obj = new Object();
        //创建一个顾客线程（消费者）
        new Thread(){
            @Override
            public void run(){
                //使用同步技术
                synchronized(obj){
                    System.out.println("Tell someone");
                    try{
                        obj.wait();	//调用obj进行等待
                    }catch(InterruptedException e){
                        e.printStackTrace();
                    }
                }
            }
        }.start();
    }
    
    //生产者线程
    new Thread(){
        @Override
        public void run(){
            //花费五秒钟生产
            try{
                Thread.sleep(5000);
            }catch(InterruptedException e){
                e.printStackTrace();
            }
            synchronized(obj){
                System.out.println("well done");
                obj.notify();	//调用obj进行唤醒，两者要一致
            }
        }
    }
}
```

```java
/*
进入到TimeWaiting（等待计时）有两种方式
1. 使用sleep(long m)方法，在毫秒值结束之后，线程唤醒进入到Runnable/Blocked状态
2. 使用wait(long m)方法，wait方法如何在毫秒值结束之后，还没有被notify唤醒，线程会自动醒来进入Runnable/Blocked状态

唤醒的方法：
1. notify() :如果有多个等待线程，随机唤醒一个
2. notifyAll() :唤醒所有等待的线程
*/

...
    synchronized(obj){
                    System.out.println("Tell someone");
                    try{
                        obj.wait(5000);	//调用obj进行等待，带参数wait
                    }catch(InterruptedException e){
                        e.printStackTrace();
                    }
                }
...
    synchronized(obj){
                System.out.println("well done");
                obj.notifyAll();	//调用obj进行唤醒，唤醒所有等待的线程
            }
```



### 线程间通信

> 多个线程处理同一个资源，但是处理的动作（线程的任务）却不相同。
>
> 而多个线程并发执行时，在默认情况下Cpu是随机切换的。当我们需要多个线程有规律的共同完成一件任务，就需要多个线程之间协调通信

🙋‍ 利用**等待唤醒机制**完成线程间通信的实现

#### 等待唤醒机制

```java
//所使用到的方法
wait()
notify()
notifyAll()
/*
即使只通知了一个等待的线程，被通知线程也不能立即恢复执行，因为它当初中断的地方是在同步块内，而此刻它已经不持有锁，所以她需要再次尝试去获取锁（很可能面临其他线程的竞争），成功后才能在当初调用wait方法之后的地方恢复执行。
总结：
	* 如果能获取锁，线程从waiting状态变成runnable状态
	* 否则，从wait set出来，又进入entry set，县城就从waiting状态变成blocked状态
*/
    
/*
使用细节：
1. wait方法与notify方法必须要由同一个锁对象调用。因为：对应的锁对象可以通过notify唤醒使用同一个锁对象调用的wait方法后的线程
2. wait方法与notify方法是属于Object类的方法
3. wait方法与notify方法必须要在同步代码块或者同步函数中使用。因为：必须要通过锁对象调用这2个方法。
*/
```



🔸 发等待唤醒需求分析，以生产者消费者（包子铺）为例

分许：一个生产者消费者问题需要哪些类

1. 资源类 —— 包子类：

   设置包子的属性：

   * 皮
   * 馅
   * 包子的状态：有true，无false

2. 生产者类 —— 线程类，继承Thread

   设置线程任务：生产包子

   ​	对包子状态进行判断：

     * true：有包子

       包子调用wait方法进入等待状态

     * false：没有包子

       生产者生产包子

       生产者生产完毕将包子的状态改为true

       同时唤醒消费者线程

3. 消费者类 —— 线程类，继承Thread

   设置线程任务：消费包子

   ​	对包子状态进行判断：

   * true：有包子

     消费者消费包子

     修改包子的状态为false

     唤醒生产者线程

   * false：没有包子

     消费者线程调用wait进入等待状态

4. 测试类 —— main方法

   创建生产者对象，创建包子线程，创建消费者线程

```JAVA
public class BaoZi{
    String pi;
    String xian;
    Boolean state = false;
}
/*---------------------------------------------------------------------------*/
public class BaoZiPu extends Thread{
    private BaoZi bz;

    public BaoZiPu(BaoZi bz) {
        this.bz = bz;
    }

    @Override
    public void run() {
        int count = 0;
        while(true){
            synchronized (bz){
                if(bz.state){
                    try {
                        bz.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }

                else{
                    if(count%2==0){
                        bz.pi = "薄皮";
                        bz.xian = "三鲜馅";
                    }else{
                        bz.pi = "冰皮";
                        bz.xian = "牛肉大葱馅";
                    }
                    count++;
                    System.out.println("正在生产："+bz.pi+bz.xian+"包子");
                    //生产包子需要三秒钟
                    try {
                        Thread.sleep(3000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    bz.state = true;
                    bz.notify();
                    System.out.println("包子铺已经生产完毕");
                }
            }
        }
    }
}
/*---------------------------------------------------------------------------*/
public class ChiHuo extends Thread{
    private BaoZi bz;

    public ChiHuo(BaoZi bz) {
        this.bz = bz;
    }

    @Override
    public void run() {
        while(true){
            synchronized (bz){
                if(bz.state){
                    System.out.println("吃货吃了一个"+bz.pi+bz.xian+"的包子");
                    bz.state=false;
                    bz.notify();
                }else{
                    try {
                        bz.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    }
}
/*---------------------------------------------------------------------------*/
public class Method {
    public static void main(String[] args) {
        BaoZi bz = new BaoZi();
        new BaoZiPu(bz).start();
        new ChiHuo(bz).start();

    }
}
```



### 线程池

如果并发的线程数量很多，并且每个线程都是执行一个时间很短的任务就结束了，这样频繁创建线程就会大大降低系统的效率，因为频繁创建线程和销毁线程需要时间。

所以需要一种办法使得线程可以复用，执行完一个任务，并不被销毁，继续执行其他任务 —— 线程池

![image-20210107130945284](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210107130945284.png)

![image-20210107131034993](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210107131034993.png)

java.util.concurrent.Executors：线程池的工厂类，用来生成线程池

Executors类中的静态方法：

```java
static ExecutorService newFixedThreadPool(int nThreads)		//创建一个可重用固定线程数的线程池
//参数：
    int nThreads //创建线程池中包含的线程数量
//返回值：
    ExecutorService接口，返回的是ExecutorService接口的实现类对象，我们可以使用ExecutorService接口接收（面向接口编程）

java.util.concurrent.ExecutorService：线程池接口
    用来从线程池中获取线程，调用start方法，执行线程任务
    	submit(Runnable task) 提交一个Runnable任务用于执行
    关闭/销毁线程池的方法
    	void shutdown()
```

```java
/*
线程池使用步骤：
1. 使用线程池的工厂类Executors里提供的静态方法newFixedThreadPool生产一个指定线程数量的线程池
2. 创建一个类，实现Runnable接口，重写run方法，设置线程任务
3. 调用ExecutorService中的方法submit，传递线程任务（实现类），开启线程执行run方法
4. 调用ExecutorService中的方法shutdown销毁线程池（不建议执行），线程池小时候就不能再使用了
*/

public class DemoThreadPool{
    public static void main(String[] args) {
        //1. 使用线程池的工厂类Executors里提供的静态方法newFixedThreadPool生产一个指定线程数量的线程池
        ExecutorService es = Executors.newFixedThreadPool(2);
        //3. 调用ExecutorService中的方法submit，传递线程任务（实现类），开启线程执行run方法
        es.submit(new RunnableImpl());
        //线程池会一直开启，使用完了线程会归还给线程池，线程可以继续使用
        es.submit(new RunnableImpl());
        es.submit(new RunnableImpl());   
    }
}
/*------------------------------------------------------------------*/
//2. 创建一个类，实现Runnable接口，重写run方法，设置线程任务
public class RunnableImpl implements Runnable{
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName());
    }
}
```

### Lambda表达式

#### 函数式编程思想

面向对象过程强调 —— 通过对象的形式来做事情

函数式思想 —— 强调做什么，要求结果，而不是以什么形式做

--------------

**冗余的Runnable代码**

```java
public class DemoRunnable{
	public static void main(String[] args){
        RunnableImpl run = new RunnableImpl();
        Thread t = new Thread(run);
        t.start();
        
        //简化代码,使用匿名内部类
        Runnbale r = new Runnable(){
            @Override
            public void run(){
                System.out.println(Thread.currentThread().getName());
            }
        }
        new Thread(r).start();
        
        //进一步简化
        new Thread(new Runnable(){
            @Override
            public void run(){
                System.out.println(Thread.currentThread().getName());
            }
        }).start();
	}
}

/*------------------------------------------------------------------*/

public calss RunnableImpl implements Runnable{
    @Override
    public void run(){
        System.out.println(Thread.currentThread().getName());
    }
}

```

代码分析：

1. Thread类需要Runnable接口作为参数，其中的抽象run方法是用来指定线程任务的核心
2. 为了指定run的方法体，不得不需要Runnable接口的实现类
3. 为了省去定义一个RunnbaleImpl实现类的麻烦，不得不使用匿名内部类
4. 必须覆盖重写抽象run方法，所以方法名称、方法参数、方法返回值不得不再写一遍，且不能写错
5. 而实际上，似乎只有方法体才是关键所在

🙋‍ 故需要改变编程思维

```java
public class DemoRunnable{
	public static void main(String[] args){
        //匿名内部类写法
        new Thread(new Runnable(){
            @Override
            public void run(){
                System.out.println(Thread.currentThread().getName());
            }
        }).start();
        
        //使用Lambda表达式
        new Thread(()->{
                System.out.println(Thread.currentThread().getName());
            }
        ).start();
	}
}
```

匿名内部类的好处与弊端

* 好处：省去了实现类的定义
* 弊端：语法复杂

#### Lambda标准格式

 包含3个部分：

* 一些参数
* 一个箭头
* 一段代码

格式：

```java
(参数列表)->{一些重写方法的代码}

/*
()：接口中抽象方法的参数列表，没有参数，就空着；有参数就写出参数，多个参数使用逗号分隔
->：传递的意思，把参数传递给方法体{}
{}：重写接口的抽象方法的方法体
*/
```

```java
public interface Cook {
    public abstract void makeFood();
}
/*-----------------------------------------------------*/
public class DemoLambda {
    public static void main(String[] args) {
        //匿名内部类
        invokeCook(new Cook() {
            @Override
            public void makeFood() {
                System.out.println("test"); 
            }
        });
        //lambda表达式
        invokeCook(()->{
            System.out.println("test");
            }
        );
    }

    public static void invokeCook(Cook cook){
        cook.makeFood();
    }
}
```

```java
//带参数的情况
public class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
/*------------------------------------------------------------*/
public class DemoArrays {
    public static void main(String[] args) {
        //数组存储多个Person对象
        Person[] arr = {
                new Person("Tom",38),
                new Person("Tim",18),
                new Person("Alice",15)
        };

        /*Arrays.sort(arr, new Comparator<Person>() {
            @Override
            public int compare(Person o1, Person o2) {
                return o1.getAge()-o2.getAge();
            }
        });*/

        //Lambda表达式，带参数，需要返回值
        Arrays.sort(arr,(Person o1,Person o2)->{
            return o1.getAge()-o2.getAge();
        });

        for(Person p:arr){
            System.out.println(p);
        }
    }
}
```

#### Lambda表达式的省略

1. （参数列表）：括号中参数列表的数据类型可以省略不写

2. （参数列表）：括号中的参数如果只有一个，那么类型和（）都可以省略

3.   {一些代码}   ：如果{}中的代码只有一行，无论是否有返回值，都可以省略（{}，return，分号）

   注意：要省略，则三者一起省略（{}，分号，return）

```java
//省略前
invokeCook(()->{
            System.out.println("test");
            }
        );
//省略后
invokeCook(()->System.out.println("test"));

//省略前
Arrays.sort(arr,(Person o1,Person o2)->{
            return o1.getAge()-o2.getAge();
        });
//省略后
Arrays.sort(arr,(o1, o2)->o1.getAge()-o2.getAge());
```

Lambda表达式的使用前提：

1. 必须**具有接口**，且要求接口中**只有且仅有一个抽象方法**

2. 必须具有**上下文推断**

   方法的参数或局部变量类型必须为Lambda对应的接口类型

> 备注：有且仅有一个抽象方法的接口，成为“函数式接口”



## 集合

> Java集合类存放于 java.util 包中，是一个用来存放对象的容器。

* 集合**只能存放对象**。比如你存一个 int 型数据1放入集合中，其实它是自动转换成 Integer 类后存入的，**Java中每一种基本类型都有对应的引用类型**。
* 集合**存放的是多个对象的引用**，对象本身还是放在堆内存中。
* 集合**可以存放不同类型，不限数量的数据类型**。

Java 集合可分为 Set、List 和 Map 三种大体系

* Set：**无序、不可重复**的集合
* List：**有序，可重复**的集合
* Map：**具有映射关系**的集合

![image-20210106132050648](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210106132050648.png)

![image-20210106132134446](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210106132134446.png)

### Collection中共性的方法

```java
public boolean add(E e)	//把给定的对象添加到集合中
public void clear()	//清空集合中所有的元素
public boolean remove(E e)	//把给定的对象在当前集合中删除
public boolean contains(E e)	//判断当前集合中是否包含给定的对象
public boolean isEmpty()	//判断当前集合是否为空
public int size()	//返回集合中元素的个数
public Object[] toArray()	//把集合中的元素存储到数组中
```

### 迭代器

iterator接口 —— 用来对集合进行遍历

```java
Iterator<String> it = coll.iterator();
boolean b = it.hasNext();
while(b){
    String s = it.next();
    System.out.println(s);
}
```

增强for循环

```java
for(String s:coll){
	...
}
```



### Set

![image-20210106135510019](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210106135510019.png)

#### HashSet

1. 无序，存储与取出顺序可能不一致
2. 底层是哈希表结构，查询速度快
3. 不是线程安全的
4. 集合元素可以为null

🙋‍ 存在set集合的哪个位置由这个值的hashcode决定

🙋‍ **不可重复，指的是hashcode不相同（如果两个元素的 equals() 方法返回 true，但它们的 hashCode() 返回值不相等，hashSet 将会把它们存储在不同的位置，但依然可以添加成功）**

##### LinkedHashSet

- 不允许重复
- 有序

#### TreeSet

> TreeSet 是 SortedSet 接口的实现类，TreeSet 可以确保集合元素处于排序状态。
>
> TreeSet 支持两种排序方法：自然排序和定制排序。默认情况下，TreeSet 采用自然排序(按升序排列)。

定制排序：如果需要实现定制排序，则需要在创建 TreeSet 集合对象时，提供一个 Comparator 接口的实现类对象。由该 Comparator 对象负责集合元素的排序逻辑。

```java
/*
自然排序：
排序：TreeSet 会调用集合元素的 compareTo(Object obj) 方法来比较元素之间的大小关系，然后将集合元素按升序排列
	1. 如果 this > obj,返回正数 1
	2. 如果 this < obj,返回负数 -1
	3. 如果 this = obj,返回 0 ，则认为这两个对象相等
☆必须放入同样类的对象.(默认会进行排序) 否则可能会发生类型转换异常.我们可以使用泛型来进行限制
*/

//定制排序
public static void main(String[] args){
	Person p1 = new Person(1);
	Person p2 = new Person(2);
	Person p3 = new Person(3);
    
    Set<Person> set = new TreeSet<>(new Person());
    set.add(p1);
    set.add(p2);
    set.add(p3);
    System.out.println(Set);
}

Class Person implements Comparator<Person>{
    public int age;
    public Person(){}
    public Person(int age){
        this.age = age;
    }
    @Override
    public int compare(Person o1,Person o2){
        if(o1.age > o2.age){
            return 1;
        }else if(o1.age < o2.age){
            return -1;
        }else{
            return 0;
        }
    }
}
```



### List

![image-20210106135732693](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210106135732693.png)

* List 代表一个**元素有序、且可重复**的集合，集合中的每个元素都有其对应的顺序索引
* List 允许使用重复元素，可以**通过索引来访问指定位置**的集合元素

#### ArrayList

ArrayList 底层数据存储结构是数组，查找快，增删慢

#### LinkedList

LinkedList 数据存储结构是链表结构，增删快，查询慢；包含大量对首尾元素进行操作的方法

- addFirst() = push()
- addLast() = add()
- getFirst()
- getLast()
- removeFirst()
- removeLast()
- pop()
- poll()

#### Vector

单线程，被ArrayList取代，都是数组结构



### Map

> 用于保存具有映射关系的数据，因此 Map 集合里保存着两组值，一组值用于保存 Map 里的 Key，另外一组用于保存 Map 里的 Value
>
> key 和 value 都可以是任何引用类型的数据
>
> Map 中的 Key 不允许重复
>
> Map会按照key进行排序，因为他的底层实现是tree

![image-20210106140316537](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210106140316537.png)

#### HashMap

底层是哈希表，查询速度块（**线程不安全**，可以保存null）

- 1.8之前，数组+单向链表
- 1.8之后，数组+单项联编/红黑树（链表的长度超过8）

无序集合（存取顺序可能不一致）

Hashmap中的键值对，如果想要使得对象唯一，如<Person类，String>，则需要重写Person类中的hashcode和equals方法

##### LinkedHashMap

底层是哈希表+链表（记录元素顺序）

有序集合，key不允许重复，存取顺序一致



#### Hashtable

底层是哈希表

- 但key和value不允许null值
- **线程安全**（单线程，速度慢）

Hashtable已被Hashmap取代（和Vector被ArrayList取代一样）

但子类Properties依旧活跃，是一个唯一和IO流相结合的集合



