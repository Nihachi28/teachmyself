# JavaSE Part2

## 泛型

![image-20210106140632386](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210106140632386.png)



### 泛型类

```java
//传入的形参类型用泛型表示
class Generic<T>{
    private T key;
    public void setKey(T key){
        this.key = key;
    }
    public T getKey(){
        return key;
    }
}
```



### 泛型接口

```java
//定义一个泛型接口
interface Generator<T> {
    T next();
}
```

```java
/**
* 未传入泛型实参时，与泛型类的定义相同，在声明类的时候，需将泛型的声明也一起加到类中
* 即：class FruitGenerator<T> implements Generator<T>{
* 如果不声明泛型，如：class FruitGenerator implements Generator<T>，编译器会报错："Unknown class"
*/
class FruitGenerator<T> implements Generator<T>{
    @Override
    public T next() {
        return null;
    }
```

```java
/**
* 传入泛型实参时：
* 定义一个生产器实现这个接口,虽然我们只创建了一个泛型接口Generator<T>
* 但是我们可以为T传入无数个实参，形成无数种类型的Generator接口。
* 在实现类实现泛型接口时，如已将泛型类型传入实参类型，则所有使用泛型的地方都要替换成传入的实参类型
* 即：Generator<T>，public T next();中的的T都要替换成传入的String类型。
*/
class FruitGenerator implements Generator<String> {    //如果实现接口时指定接口的泛型的具体数据类型，那么这个类实现接口的所有方法的位置都要泛型替换实际的具体数据类型
    @Override
    public String next() {
    	return null;
    }
}
```

```java
public class Test {    //实际使用中，当B1为没有传入泛型实参的方法时，创建对象时要指明对象的泛型
       public static void main(String[] args) {
              B1<Object> b1 = new B1<Object>();
       }
}

____________________________________________________________________________________________

public class Test {    //实际使用中，当B2为已经传入泛型实参的方法时，创建对象时不要指明对象的泛型，指明了反而会报错
       public static void main(String[] args) {
              B2 b2 = new B2();
       }
}
```



### 泛型方法

```java
class Cc<E>{
       private E e;
       
       public static <T> void test3(T t) {      //静态方法的泛型
              System.out.println(t);
       }
       
       public <T> void test(T s) {    //无返回值
              System.out.println(this.e);    //在类上定义的泛型，可以在普通方法中调用(在静态方法中不能使用)
              T t = s;
       }
       
       public <T> T test1(T s) {  //有返回值
              return s;
       }
       
       public <T> void test2(T...strs) { //可变参数类型方法
              for(T s : strs) {
                     System.out.println(s);
              }
       }
}
```



### 通配符

```java
/**
* 不确定集合中的元素具体的数据类型
* 使用?表示所有类型
* @param list
*/
public void test(List<?> list){
    System.out.println(list);
}
```

🙋‍ 作为参数传递可以使用，不可以创建对象使用

#### 泛型的上下限限定

> 泛型的上限限定：？ extends E		代表使用的泛型只能是E类型的子类/本身
>
> 泛型的下线限定：？ super E			代表使用的泛型只能是E类型的父类/本身

使用方法举例：

<? extends Person>   (无穷小 , Person]

只允许泛型为Person及Person子类的引用调用

<? super Person >   [Person , 无穷大)

只允许泛型为Person及Person父类的引用调用

<? extends Comparable>

只允许泛型为实现Comparable接口的实现类的引用调用

![image-20210106144039894](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210106144039894.png)

## 注解

## File类

### File类静态成员变量

```java
import java.io.File;
public class DemoFile {
    public static void main(String[] args) {
        String pathSeparator = File.pathSeparator;
        System.out.println(pathSeparator);	//路径分隔符，windows是分号，linux是冒号
        String separator = File.separator;
        System.out.println(separator);		//文件名称分隔符,windows是反斜杠\，linux是正斜杠/
        //为了保证可以在各种系统下运行，可以利用字符串获取地址："c:"+File.separator+"develop"+File.separator+"a.txt"
    }
}
```

### 绝对路径与相对路径

* 绝对路径
  * 一个完整的路径
* 相对路径
  * 一个简化的路径

注意：

1. 路径不区分大小写
2. 路径中的文件名分隔符windows使用反斜杠，反斜杠是转义字符，两个反斜杠表示一个普通斜杠

### File类的构造方法

```java
//通过将给定路径名称字符串转换为抽象路径名来创建一个File实例
File(String pathname)
File(String parent, String child)
File(File parent, String child)		//路径可以随时变换，父路径是File，可以使用File的方法对路径进行操作再创建路径
```

```java
File t = new File("d:\\","a,txt");
```

### 获取功能的方法

```java
public String getAbsolutePath()	//返回File的绝对路径名字符串
public String getPath()		//将此File转换为路径名字符串（构造方法中的地址）
public String getName()		//返回由此File表示的文件或目录的名称(构造方法中的结尾部分)
public long length()		//返回由此File表示的文件的长度（文件大小，字节为单位）
```

### 判断功能的方法

```java
public boolean exists()		//此File表示的文件或目录是否实际存在
public boolean isDirectory()	//此File表示的是否为目录
public boolean isFile()		//此File表示的是否为文件
//判断文件和地址的前提是路径真实存在
```

### 创建和删除功能的方法

```java
public boolean createNewFile()	//当且仅当具有该名字的文件尚不存在时，创建一个新的空文件
public boolean delete()	//删除由此File表示的文件或目录
public boolean mkdir()	//创建由此File表示的目录
public boolean mkdirs()	//创建由此File表示的目录，包括任何必须但不存在得父目录
```

### 遍历目录的功能

```java
public String[] list()	//返回一个String数组，表示该File目录中所有子文件或目录
public File[] listFiles()	//返回一个File数组，表示该File目录中的所有的子文件或目录
    
/*
注意：
List方法和ListFiles方法遍历的是构造方法中给出的目录
如果构造方法中给出的目录的路径不存在，会抛出空指针异常
如果构造方法中给出的路径不是目录，也会抛出空指针异常
*/
```

### 递归目录输出或查找文件

递归分类：

* 直接递归：调用自身
* 间接递归：A方法调用B，B调用C，C调用A

注意：

* 要有条件限制，可以停止下来
* 次数不能太多，否则容易内存溢出
* 构造方法禁止递归

```java
public class DemoFile {
    public static void main(String[] args) {
        File file = new File("c:\\abc");
        getAllFile(file);
    }

    public static void getAllFile(File dir){
        System.out.println(dir);
        File[] files = dir.listFiles();
        for(File f:files){
            if(f.isDirectory()){
                getAllFile(f);
            }else{
                System.out.println(f);
            }
        }
    }
    
    /*------------------------------------------------*/
    //搜索以xxx结尾的文件并输出
    public static void getAllFile(File dir){
        System.out.println(dir);
        File[] files = dir.listFiles();
        for(File f:files){
            if(f.isDirectory()){
                getAllFile(f);
            }else{
                String s = f.getName();
                s = s.toLowerCase();
                boolean b = s.endsWith(".java");
                if(b){
                    System.out.println(f);
                }
            }
        }
    }
}
```

#### 过滤器

```java
File[] ListFiles(FileFilter filter)		//参数为过滤器
//FileFilter接口：用于抽象路径名（File对象）的过滤器
//接口中含如下用来过滤文件的方法：
boolean accept(File pathname)	//测试指定抽象路径名是否应该包含在某个路径名列表中
    
File[] ListFiles(FilenameFilter filter)	
//FilenameFilter接口：实现此接口的类实例可用于过滤文件名
//接口中含如下用来过滤文件的方法：
boolean accept(File dir, String name)	//测试指定文件是否应该包含在某一文件列表中
/*
参数：
	File dir：构造方法中传递的被遍历的目录
	String name：使用ListFiles方法遍历目录，获取的每一个文件/文件夹的名字
	
两个接口都没有实现类，需要自己写实现类，重写过滤方法accept
*/
```

```java
public class FileFilterImpl implements FileFilter{
    //accept过滤规则：判断是否以xxx结尾，是返回true，不是返回false
	@Override
    public boolean accept(File pathname){
        //如果pathname是一个文件夹，返回true，继续遍历
        if(pathname.isDirectory()){
            return true;
        }
        return pathname.getName().toLowerCase().endswith(".java");
    }
}
```

匿名内部类与lambda表达式写法

```java
public static void getAllFile(File dir){
    //FileFilter接口
    File[] files = dir.listFiles(new FileFilter() {
        @Override
        public boolean accept(File pathname) {
            if(pathname.isDirectory()){
                return true;
            }
            return pathname.getName().toLowerCase().endsWith(".java");
        }
    });
	
    //FilenameFilter接口
    File[] files2 = dir.listFiles(new FilenameFilter() {
        @Override
        public boolean accept(File dir, String name) {
            return new File(dir,name).isDirectory() || name.toLowerCase().endsWith(".java");
        }
    });

    //Lambda表达式
    File[] files3 = dir.listFiles((File d,String name)->{
                return new File(d,name).isDirectory() || name.toLowerCase().endsWith(".java");
    });

    for(File f:files){
        if(f.isDirectory()){
            getAllFile(f);
        }else{
            String s = f.getName();
            s = s.toLowerCase();
            boolean b = s.endsWith(".java");
            if(b){
                System.out.println(f);
            }
        }
    }
}
```



## IO流

![image-20210108142228530](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210108142228530.png)

### 字节流

java.io.OutputStream

* 此抽象类是表示输出字节流的所有类的超类

其定义了一些子类共性的成员方法：

```java
public void close()	//关闭此输出流并释放与此流相关联的任何系统资源
public void flush()	//刷新此输出流并强制任何缓冲的输出字节被写出
public void write(byte[] b)	//将b.length字节从指定的字节数组写入此输出流
public void write(byte[] b, int off, int len)	//从指定的字节数组写入len字节，从偏移量off开始输出到此输出流
public abstract void write(int b)	//将指定的字节输出流
```

------------

#### 字节输出流（FileOutputStream）

java.io.OutputStream的子类：

java.io.FileOutputStream extends OutputStream

FileOutputStream：文件字节输出流

* 作用：把内存中的数据写入到硬盘的文件中

```java
FileOutputStream(String name) //创建一个向具有指定名称文件中写入数据的输出文件流
FileOutputStream(File file)	//创建一个向指定File对象表示的文件中写入数据的文件输出流
/*
* String name: 目的地是一个文件的路径
* File file: 目的地是一个文件
*
* 构造方法的作用：
* 1. 创建一个FileOutputStream对象
* 2. 会根据构造方法中传递的文件/文件路径，创建一个空的文件
* 3. 会把FileOutputStream对象指向创建好的文件
*/
```

-------------

```java
/*
* 写入数据的原理
* java程序-->JVM-->OS-->OS调用写数据的方法-->把数据写入到文件
*/
/*
* 字节输出流的使用步骤
* 1. 创建一个FileOutputStream对象，构造方法中传递写入数据的目的地
* 2. 调用FileOutputStream对象中的方法write，把数据写入到文件中
* 3. 释放资源（流使用会占用一定的内存，使用完毕要把内存清空，提高程序的效率）
*/
public class DemoFile {
    public static void main(String[] args) {
        try {
            FileOutputStream fos = new FileOutputStream("E:\\我的坚果云\\Java\\test.txt");
            fos.write(97);
            fos.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

写数据的过程，会把10进制的整数97转换为二进制数97：97-->1100001

任意的文本编辑器在打开文件的时候，都会查询编码表，把字节转换为字符表示：

* 0-127：查询ASCII表
* 其他值：查询系统默认码表（中文系统GBK）

```java
public class DemoFile {
    public static void main(String[] args) {
        try {
            FileOutputStream fos = new FileOutputStream(new File("E:\\我的坚果云\\Java\\test.txt"));
            //调用FileOutputStream的方法write把字节数组数据写入文件中
            //如果第一个字节是正数（0-127）那么显示的时候会查询ASCII表
            //如果写的第一个字节是负数，那么第一个字节会和第二个字节组成一个中文显示，查询系统默认码表（GBK）
            byte[] btes = {65,66,78,55};
            fos.write(btes);
            
            //btes中第几个开始，写几个
            fos.write(btes,1,2);
            
            //写入字符串的方法，利用byte[] getBytes()
            byte[] bytes2 = "你好".getBytes();
            fos.write(btes2);
            
            fos.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

--------

字节流的续写和换行

```java
//追加写/续写：使用两个参数的构造方法
FileOutputStream(String name, boolean append) //创建一个向具有指定name的文件中写入数据的输出文件流
FileOutputStream(File file, boolean append)	//创建一个向指定File对象表示的文件中写入数据的文件输出流
/*
* 参数
* String name，File file 写入数据的目的地
* boolean append 追加写开关
* 	true：创建对象不会覆盖原文件，继续再文件末尾追加写数据
* 	false：创建一个新文件，覆盖原文件
*/
    
//换行
/* windows:\r\n    
*  linux:/n
*  mac:/r
*/
```

```java
public class DemoFile {
    public static void main(String[] args) throws Exception{
        FileOutputStream fos = new FileOutputStream("E:\\我的坚果云\\Java",true);	//续写
        for(int i = 0; i < 10; i++){
            fos.write("你好".getBytes());
            fos.write("\r\n");	//换行
        }
        fos.close();
    }
}
```



#### 字节输入流（FileInputStream）

java.io.InputStream：字节输入流

* 此抽象类是表示字节输入流的所有类的超类

```java
//定义了所有子类共性的方法
int read()			//从输入流中读取数据的下一个字节
int read(byte[] b)	//从输入流中读取一定数量的字节，并将其存储在缓冲区数组b中
void close()		//关闭此输入流并释放与该流关联的所有系统资源
```

java.io.FileInputStream extends InputStream

FileInputStream：文件字节输入流

* 作用：把硬盘文件中的数据，读取到内存中使用

```java
//构造方法：
FileInputStream(String name)
FileInputStream(File file)
```

参数：读取文件的数据源

* String name：文件的路径
* File file：文件

构造方法的作用：

1. 会创建一个FileInputStream对象
2. 获取FileInputStream对象指定构造方法中要读取的文件

写入数据的原理

* java程序-->JVM-->OS-->OS调用写数据的方法-->把数据写入到文件

字节输入流的使用步骤：

1. 创建FileInputStream对象，构造方法中绑定尧都区的数据源
2. 使用FileInputStream对象中的方法read，读取文件
3. 释放资源

```java
public class DemoFile {
    public static void main(String[] args) throws Exception{
        FileInputStream fis = new FileInputStream("E:\\我的坚果云\\Java\\test.txt");
        int len = fis.read());
        System.out.println(len);	//按顺序得到字节，当得到-1时达到文件结尾
        
        //循环得到所有的内容，注意布尔表达式
        int len_1 = 0;
        while((len = fis.read())!=-1){
            System.out.println((char)len);
        }
        fos.close();
    }
}
```

![image-20210108165932212](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210108165932212.png)

```java
//一次读取多个字节
FileInputStream fis = new FileInputStream("a.txt");	//ABCD
byte[] bytes = new byte[2];		//有效读取的数组长度为2，一次读取2个字节
int len = fis.read(bytes);		

System.out.println(len);
System.out.println(new String(bytes));	//AB

int len = fis.read(bytes);		

System.out.println(len);
System.out.println(new String(bytes));	//CD
//每一次读取都会使得指针向后走两位

//优化后
byte[] bytes2 = new byte[1024];
int len = 0;	//记录有效字节数
while({len = fis.read(bytes2)!=-1}){
    System,out.println(new String(bytes,0,len));
}
```

明确两件事：

1. 方法的参数byte[]的作用：

   起到缓冲作用，存储每次读取到的多个字节

   数组的长度一般定义为1024（1kb）或1024的整数倍

2. 方法的返回值int是什么：

   每次读取到的有效字节个数

### 字符流

#### 字符输入流（Reader）

java.io.Reader

* 字符输入流，是字符输入流的最顶层的父类，定义了一些共性的成员方法
* 是抽象类

```java
//共性的成员方法：
int read()	//读取单个字符并返回
int read(char[] cbuf) //一次读取多歌字符，将字符读入数组
void close() //关闭该流并释放与之关联的所有资源
```



java.io.FileReader extend Reader

* 文件字符输入流
* 作用：把硬盘文件中的数据以字符的方式读取到内存中

构造方法：

```java
FileReader(String fileName)	//参数是路径
FileReader(File file)			//参数是文件
```

```java
public static void main(String[] args) throws Exception{
        FileReader fr = new FileReader("a.txt");
    	//第一种方法
        int len = 0;
        while((len = fr.read())!=-1) {
            System.out.println((char)len);
        }
        fr.close();

    	//第二种方法
        char[] chars = new char[1024];
        int len2 = 0;
        while((len2 = fr.read(chars))!=-1){
            System.out.println(new String(chars,0,len2));
        }
        fr.close();
}
```



#### 字符输出流（Writer）

java.io.Writer

* 字符输出流，是一个抽象类
* 所有字符输出流最顶层的父类方法

```java
//共性方法
void write(int c)	//写入单个字符
void write(char[] cbuf)	//写入字符数组
abstract void write(char[] cbuf, int off, int len)	//写入字符数组的某一部分
void write(String str)	//写入字符串
void write(String str, int off, int len)	//写入字符串的某一部分
void flush()	//刷新流的缓存
void close()	//关闭流，但是会先刷新
```



java.io.FileWriter extends Writer

* 作用：把内存中字符数据写入到文件中

构造方法

```java
FileWriter(String name)	//文件的路径
FileWriter(File file)	//文件
```

字符输出流的使用步骤：

1. 创建FileWriter对象，构造方法中绑定要写入数据的目的地
2. 使用FileWriter中的方法write，把数据写入到内存缓冲区中（字符转换为字节的过程）
3. 使用FileWriter中的方法flush，把内存缓冲区中的数据，刷新到文件中
4. 释放资源（会先把内存缓冲区中的数据刷新到文件中）

🙋‍  字符与字节的区别：字符输出流不是直接把字符写入到文件中，而是写入到内存中

```java
public class DemoFile {
    public static void main(String[] args) throws Exception{
        FileWriter fw = new FileWriter("a.txt");
        fw.write(97);
        fw.flush();
        fw.close();
    }
}
```

🙋‍ flush方法与close方法的区别：

* flush：刷新缓冲区，流对象可以继续使用
* close：先刷新缓冲区，然后通知系统释放资源。流对象就不可以再被使用了。

```java
public class DemoFile {
    public static void main(String[] args) throws Exception{
        FileWriter fw = new FileWriter("a.txt");
        fw.write(97);
		//1.单个字符
        char[] cs = {'a','b','c','d'};
        fw.write(cs);
		//2.从off位开始，读2个
        fw.write(cs,2,2);
		//3.写字符串
        fw.write("Hello world");
        //4.写字符串，从off位开始，读5个
        fw.write("Hello world",2,5);
        fw.flush();
        fw.close();
    }
}
```

##### 续写&追加写

```java
FileWriter(String fileName, boolean append)
FileWriter(File file, boolean append)
```

```java
public class DemoFile {
    public static void main(String[] args) throws Exception{
        FileWriter fw = new FileWriter("a.txt",true);
        for(int i = 0; i<10; i++){
            fw.write("Hello world"+i+"\r\n");
        }
        fw.close();
    }
}
```

### try-catch-finally处理流中的异常

```java
public class DemoFile {
    public static void main(String[] args){
        FileWriter fw = null;  //提高fw的作用域，让finally可以使用
        //变量在定义的时候可以没有值，但是在使用的时候必须有值，所以给一个null
        try{
            fw = new FileWriter("a.txt",true);
            for(int i = 0; i<10; i++){
                fw.write("Hello world"+i+"\r\n");
            }
        }catch (IOException e){
            System.out.println(e);
        }finally {
            //创建对象如果失败，则fw的值为null，null无法调用方法
            //所以进行判断，如果不是null，就处理异常
            if(fw!=null){
                try {
                    fw.close();
                }catch (IOException e){
                    e.printStackTrace();
                }
            }
        }

    }
}
```

🙋‍ JDK7的新特性，try的后边可以增加一个（），在括号中可以定义流对象

那么这个流对象的作用域就在try中有效，try中代码执行完毕自动释放，不用写finally

```java
//格式
try(定义流对象；定义流对象...){
	...
}catch{
	...
}
//Example
try(FileWriter fw = new FileWriter("a.txt",true);){
    for(int i = 0; i<10; i++){
        fw.write("Hello world"+i+"\r\n");
    }
}catch (IOException e){
    System.out.println(e);
}
```

🙋‍ JDK9中可以直接在（）中写入引用变量



### Properties集合

java.util.Properties集合 extends Hashtable<k,v> implements Map<k,v>

Properties类：

* 表示了一个持久的属性集
* 可保存在流中或从流中加载
* 唯一与IO流相结合的集合
  * 可使用store方法，把集合中的临时数据，持久化写入到硬盘中存储
  * 可使用load方法，把硬盘中保存的文件（键值对），读取到集合中使
* 属性表中每个键值都是一个字符串，所以可以不用写泛型
  * 有一些操作字符串的特有方法

```java
Object setProperty(String key, String value) //Hashtable的方法put
String getProperty(String key)	//通过key找到value值，此方法相当于Map集合中的get(key)方法
Set<String> stringPropertyNames()	//返回此属性列表中的键集，其中该键及其对应值是字符串，此方法相当于Map集合中的keySet方法
```

```java
public class DemoProperties {
    public static void main(String[] args) {
        Properties prop = new Properties();
        prop.setProperty("Tom","178");
        prop.setProperty("Tim","123");
        prop.setProperty("John","555");

        Set<String> st = prop.stringPropertyNames();
        for(String s : st){
            String a = prop.getProperty(s);
            System.out.println(a);
        }
    }
}
```

-----------------------------

#### store方法

```java
//把集合中的临时数据，持久化写入硬盘中存储
void store(OutputStream out, String comments)
void store(writer writer, String comments)
```

参数：

* OutputStream out：字节输出流，不能写入中文；字符流可以写中文
  * FileWriter ✔
  * FileOutputStream ❌
* Writer writer：字符输出流，可以写中文
* String comments：注释，用来解释说明保存的文件是做什么用的
  * 不能使用中文，因为默认是unicode编码
  * 一般使用空字符串“”

使用步骤：

1. 创建Properties集合对象，添加数据
2. 创建字节输出流/字符输出流对象，构造方法中绑定要输出的目的地
3. 使用Properties集合中的方法store，把集合中的临时数据，持久化写入到硬盘当中存储
4. 释放资源

------------------

#### load方法

```java
//使用load方法把硬盘中保存的文件（键值对），读取到集合中使用
void load(InputStream inStream)
void load(Reader reader)
```

参数：

1. InputStream - 字节输入流不可以读取含有中文的键值对
2. Reader - 可以读取含有中文的键值对

使用步骤：

1. 创建Properties集合对象
2. 使用Properties集合对象中的方法load读取保存键值对的文件
3. 遍历Properties集合

注意：

1. 存储键值对的文件，key与value的默认连接符号可以使用-，空格
2. 可以使用#进行注释，注释的内容不会被读取
3. key与value默认都是字符串，不用再加上引号

```java
public class DemoProperties {
    public static void main(String[] args) throws IOException {
        Properties prop = new Properties();

        prop.load(new FileReader("a.txt"));
        Set<String> set = prop.stringPropertyNames();
        for(String key:set){
            System.out.println(prop.getProperty(key));
        }
    }
}
```



### 缓冲流

> 对基本流对象的一种增强

缓冲流

* 给基本的流对象增加一个缓冲区（数组）
* 通过缓冲区读写，减少系统IO次数
* 提高基本的字节输入流的读取效率

```java
BufferedInputStream(new FileputStream())

int len = fis.read()
```

![image-20210111110357319](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210111110357319.png)

按数据类型分类：

* 字节缓冲流
  * BufferedInputStream
  * BufferedOutputstream
* 字符缓冲流
  * BufferedReader
  * BufferedWriter



#### 字节缓冲流

##### 字节缓冲输出流（BufferedOutputStream）

> java.io.BufferedOutputStream extends OutputStream

```java
//继承自父类的方法
public void close()	//关闭此输出流并释放与此流相关联的任何系统资源
public void flush()	//刷新此输出流并强制任何缓冲的输出字节被写出
public void write(byte[] b)	//将b.length字节从指定的字节数组写入此输出流
public void write(byte[] b, int off, int len)	//从指定的字节数组写入len字节，从偏移量off开始输出到此输出流
public abstract void write(int b)	//将指定的字节输出流
```

```java
//构造方法
BufferedOutputStream(OutputStream out)	//创建一个新的缓冲输出流，将数据写入指定的底层输出流
BufferedOutputStream(OutputStream out, int size)	//创建一个新的缓冲输出流，以将具有指定缓冲区大小的数据写入底层的输入
```

参数：

* OutputStream out：字节输出流
* int size：指定缓冲流内部缓冲区的大小，不指定默认

使用步骤：

1. 创建FileOutputStream对象，构造方法中绑定要输出的目的地
2. 创建BufferedOutputStream对象，构造方法中传递FileOutputStream对象，提高FileOutputStream对象效率
3. 使用BufferedOutputStream对象中的方法write，把数据写入到内部缓冲区中
4. 使用BufferedOutputStream对象中的方法flush，把内部缓冲区中的数据，刷新到文件中
5. 释放资源（会先调用flush方法刷新数据，第四步可以省略）

```java
public class DemoProperties {
    public static void main(String[] args) throws IOException {
        FileOutputStream fos = new FileOutputStream("a.txt");
        BufferedOutputStream bos = new BufferedOutputStream(fos);
        bos.write("随便写写".getBytes());
        bos.flush();
        bos.close();
    }
}
```



##### 字节缓冲输入流（BufferedInputStream）

> java.io.BufferedInputStream extends InputStream

```java
//继承自父类的方法
int read()			//从输入流中读取数据的下一个字节
int read(byte[] b)	//从输入流中读取一定数量的字节，并将其存储在缓冲区数组b中
void close()		//关闭此输入流并释放与该流关联的所有系统资源
```

```java
//构造方法
BufferedInputStream(InputStream in)	//创建一个新的缓冲输入流，保存期参数，即输出流in，以便将来使用
BufferedInputStream(InputStream in, int size)	//创建一个新的缓冲输出流，以将具有指定缓冲区大小的数据保存其参数以便将来使用
```

参数：

* InputStream in：字节输入流，给FileInputStream增加一个缓冲区，提高效率
* int size：指定缓冲流内部缓冲区的大小，不指定默认

使用步骤：

1. 创建FileInputStream对象，构造方法中绑定要读取的数据源
2. 创建BufferedInputStream对象，构造方法中传递FileInputStream对象，提高FileInputStream对象的读取效率
3. 使用BufferedInputStream对象中的方法read，读取文件
4. 释放资源

```java
public class DemoProperties {
    public static void main(String[] args) throws IOException {
        FileInputStream fis = new FileInputStream("a.txt");
        BufferedInputStream bis = new BufferedInputStream(fis);
        //一个一个读取
        int len = 0;
        while((len = bis.read())!=-1){
            System.out.println(len);
        }
        bis.close();
        
        //按数组读取
        byte[] bytes = new byte[1024];
        int len2 = 0;
        while((len2 = bis.read(bytes))!=-1){
            System.out.println(new String(bytes,0,len2));
        }
        bis.close();
    }
}
```

```java
//复制文件Demo
public class DemoProperties {
    public static void main(String[] args) throws IOException {
        FileInputStream fis = new FileInputStream("a.txt");
        BufferedInputStream bis = new BufferedInputStream(fis);

        FileOutputStream fos = new FileOutputStream("b.txt");
        BufferedOutputStream bos = new BufferedOutputStream(fos);

        byte[] bytes = new byte[1024];
        int len = 0;

        while ((len = bis.read(bytes)) != -1) {
            bos.write(bytes,0,len);
        }
        bos.close();
        bis.close();
    }
}
```

#### 字符缓冲流

##### 字符缓冲输入流（BufferedWriter）

> java.io.BufferedWriter extends Writer

```java
//继承自父类的共性方法
void write(int c)	//写入单个字符
void write(char[] cbuf)	//写入字符数组
abstract void write(char[] cbuf, int off, int len)	//写入字符数组的某一部分
void write(String str)	//写入字符串
void write(String str, int off, int len)	//写入字符串的某一部分
void flush()	//刷新流的缓存
void close()	//关闭流，但是会先刷新
```

```
//构造方法：
BufferedWriter(Writer out)	//创建一个使用默认大小输出缓冲区的缓冲字符输出流
BufferedWriter(Writer out, int sz)	//创建一个使用给定大小输出缓冲区的新缓冲字符输出流
```

参数：

* Writer out：字符输出流
* int sz：指定缓冲区大小

🙋‍ 特有的成员方法

```java
void newLine()	
//写入一个行分隔符，会根据不同的操作系统获取不同的行分隔符
```

使用步骤：

1. 创建字符缓冲输出流对象，构造方法中传递字符输出流
2. 调用字符缓冲输出流中的方法write，把数据写入到内存缓冲区
3. 调用字符缓冲流中的方法flush，把内存缓冲区中的数据，刷新到文件中
4. 释放资源

```java
public class DemoProperties {
    public static void main(String[] args) throws IOException {
        FileInputStream fis = new FileInputStream("a.txt");
        BufferedWriter bw = new BufferedWriter(new FileWriter("a.txt"));
        for(int i = 0; i < 10; i++){
            bw.write("我直接乱写");
            bw.newLine();
        }
        bw.flush();
        bw.close();
    }
}
```



##### 字符缓冲输出流（BufferedReader）

> java.io.BufferedReader extends Reader

```java
//继承自父类的共性方法
int read()			//从输入流中读取数据的下一个字节
int read(byte[] b)	//从输入流中读取一定数量的字节，并将其存储在缓冲区数组b中
void close()		//关闭此输入流并释放与该流关联的所有系统资源
```

```java
//构造方法
BufferedReader(Reader in)
BufferedReader(Reader in, int sz)
```

🙋‍ 特有的方法：

```java
String readLine()	//读取一个文本行
//判断行结束的终止符浩，是换行符\n，回车\r，回车后直接跟着换行\r\n
//返回值：包含该行内容的字符串，不包含
```

使用步骤：

1. 创建字符缓冲输入流对象，构造方法中传递字符输入流
2. 使用字符缓冲输入流对象中的方法read/readLine读取文本
3. 释放资源

```java
public class DemoProperties {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new FileReader("a.txt"));
        String line;
        while ((line = br.readLine()) != null) {
            System.out.println(line);
        }
        br.close();
    }
}
```



### 转换流

> 按照某种规则，按字符存储到计算机中，称为编码
>
> 将存储在计算机中的二进制数按某种规则解析显示，成为解码

计算机要准确的存储和识别字符集符号，需要进行字符编码，一套字符集必然至少有一套字符编码。

常见的字符集有：

* ASCII字符集 🔹 ASCII编码
* GBK字符集 🔹 GBK编码
* Unicode字符集 🔹 UTF8编码 / UTF16编码 / UTF32编码

🙋‍ 制定了编码，所对应的字符集就决定了，编码是最终需要关心的



ANSI指的是系统默认编码，我们的IDEA默认UTF-8，当我们利用IDEA来读取GBK的文件，其编码方式兼容导致乱码，所以在读取不一样的字符集时，需要改变编码表（如FileReader只能使用默认码表）

![image-20210112131006896](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210112131006896.png)

🙋‍ 转换流可以写入和读取任意码表的文件 🙋‍



#### OutputStreamWriter

> java.io.OutputStreamWriter extends Writer

作用：字符流通向字节流的桥梁，可使用指定的charset将要写入流中的字符编码成字节（编码）

方法继承自父类Writer（略）

构造方法：

```
//创建使用默认字符编码的OutputStreamWriter
OutputStreamWriter(OutputStream out)
//创建使用指定字符集的OutputStreamWriter
OutputStreamWriter(OutputStream out, String charsetName)
```

使用步骤：

1. 创建OutputStreamWriter对象，构造方法中传递字节输出流和指定的编码表名称
2. 使用...对象的方法write，把字符转换成字节存储缓冲区中（码表）
3. 使用对象中的方法flush，把内存缓冲区中的字节刷新到文件中（使用字节流写字节的过程）
4. 释放资源

```java
public class DemoProperties {
    public static void main(String[] args) throws IOException {
        write_gbk();
        write_utf_8();
    }

    private static void write_gbk() throws IOException{
        OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("b.txt"),"GBK");
        osw.write("Hello");
        osw.flush();
        osw.close();
    }

    private static void write_utf_8() throws IOException{
        OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("a.txt"),"utf-8");
        osw.write("Hello");
        osw.flush();
        osw.close();
    }
}
```



#### InputStreamReader

> java.io.InputStreamReader extends Reader

作用：字节流通向字符流的桥梁。指定charset读取字节并将其解码为字符。

构造方法：

```java
InputStreamReader(InputStream(in))
InputStreamReader(InputStream in, String charsetName)
```

使用方法：

1. 创建InputStreamReader对象，构造方法中传递字节输入流和指定的编码表名称
2. 使用InputStreamReader对象中的方法read读取文件
3. 释放资源

注意事项：

构造方法中指定的编码表要和文件的编码相同，否则会发生乱码

```java
private static void read_utf_8() throws IOException{
    InputStreamReader isr = new InputStreamReader(new FileInputStream("a.txt"),"utf-8");
    int len = 0;
    while((len = isr.read())!=-1){
        System.out.println((char)len);
    }
    isr.close();
}
```

### 序列化流

> 对java对象的原始数据类型进行读写

![image-20210112144737281](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210112144737281.png)



#### ObjectOutputStream

> java.io.ObjectOutputStream extends OutputStream

构造方法：

```java
ObjectOutputStream(OutputStream out)
//创建写入指定OutputStream的ObjectOutputStream
```

特有的成员方法：

```
void writeObject(Object obj) //将指定的对象写入ObjectOutputStream
```

使用步骤：

1. 创建ObjectOutputStream对象，构造方法中传递字节输出流
2. 使用ObjectOutputStream对象中的方法writeObject，把对象写入到文件中
3. 释放资源

🙋‍ 需要序列化的类，必须实现Serializeable接口：

* public interface Serializable
  * 标记型接口
    * 进行序列化与反序列化的过程会检查该标记
  * 类通过实现java.io.Serializable接口以启用其序列化功能
  * 未实现此接口的类==将无法使其任何状态序列化或反序列化==

若没有实现，则会抛出NotSerializableException异常。

```java
public class Person implements Serializable {
    String name;
    int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    getter&setter;
}
/*-----------------------------------*/
public class DemoProperties {
    public static void main(String[] args) throws IOException {
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("a.txt"));
        oos.writeObject(new Person("TOM",18));
        oos.close();
    }
}
```



#### ObjectInputStream

> java.io.ObjectInputStream extends InputStream

构造方法：

```java
ObjectInputStream（InputStream in） //创建从指定InputStream读取的ObjectInputStream
```

特有的成员方法：

```java
Object readObject()	//从ObjectInputStream读取对象
```

使用步骤：

1. 创建ObjectInputStream对象，构造方法中传递字节输入流
2. 使用ObjectInputStream对象中的方法readObject读取保存对象的文件
3. 释放资源
4. 使用读取出来的对象

readObject方法声明抛出了ClassNotFoundException（class文件找不到异常）

当不存在对象的class文件抛出此异常

![image-20210112154741065](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210112154741065.png)

反序列化的前提：

* 类必须实现Serializable
* 必须存在类对应的class文件

```java
public class DemoProperties {
    public static void main(String[] args) throws IOException, ClassNotFoundException{
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream("a.txt"));
        Object o = ois.readObject();
        ois.close();
        System.out.println(o);
        Person p = (Person) o;
        System.out.println(p.getAge()+p.getAge());
    }
}
```



#### transient关键字

> 瞬态关键字
>
> 被transient修饰的成员变量不能被序列化和反序列化

🙋‍ 序列化与反序列化遭遇static：

* 静态优于非静态加载到内存中（静态优先于对象进入到内存中）
* 被static修饰的成员变量不能被序列化，序列化的都是对象



#### InvaildClassException ( serialVersionUID )

每次修改类的定义，都会给class文件生成一个新的序列号

* 当序列化后，类被修改，则反序列化会失败，因为序列号与之前的序列号不匹配
* 解决方法：
  * 无论是否对类的定义进行修改，都不能重新生成新的序列号
    * 可以手动给该类添加一个序列号

实现：可序列化类可以通过声明名为“serialVersionUID”的字段（该字段必须是static、final的long型字段）显式声明其自己的serialVerisonUID

```java
private static final long serialVersionUID = 1L;
```

-----------------------

#### 存储多个对象

🙋‍ 当我们想在文件中保存多个对象的时候，可以把多歌对象存储到一个集合中，对集合进行序列化和反序列化

步骤：

1. 定义一个存储Person对象的ArrayList集合
2. 往ArrayList集合中存储Person对象
3. 创建一个序列化流ObjectOutputStream对象
4. 使用ObjectOutputStream对象中的方法writeObject，对集合进行序列化
5. 创建一个反序列化ObjectInputStream对象
6. 使用该对象的方法readObejct读取文件中保存的集合
7. 把Obejct类型的集合转换为ArrayList类型
8. 遍历ArrayList集合
9. 释放资源

```java
public class DemoProperties {
    public static void main(String[] args) throws IOException, ClassNotFoundException{
        ArrayList<Person> list = new ArrayList<>();
        list.add(new Person("tom",11));
        list.add(new Person("tim",12));
        list.add(new Person("alice",15));

        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("a.txt"));
        oos.writeObject(list);

        ObjectInputStream ois = new ObjectInputStream(new FileInputStream("a.txt"));
        Object o = ois.readObject();

        ArrayList<Person> p = (ArrayList) o;
        for(Person i:p){
            System.out.println(i);
        }

        ois.close();
        oos.close();
    }
}
```



### 打印流

> java.io.PrintStream extends OutputStream

PrintStream特点：

* 只负责数据的输出，不负责数据的读取
* 永远不会抛出IOException
* 有特有的方法：print，println
  * void print(任意类型的值)
  * void println(任意类型的值)

构造方法：

```java
PrintStream(File file)
PrintStream(OutputStream out)
PrintStream(String fileName)
//参数都是输出的目的地
```

🙋‍ 如果使用继承自父类的方法write方法写数据，那么查看数据的时候会查询编码表：97->a

🙋‍ 如果使用自己特有方法print/printIn方法写数据，写的数据原样输出：97->97

```java
public class DemoProperties {
    public static void main(String[] args) throws IOException, ClassNotFoundException{
        PrintStream ps = new PrintStream("a.txt");
        ps.println("aaa");
        ps.write(97);	//a
        ps.println(97);	//97
    }
}
```

特别方法：

```java
static void setOut(PrintStream out)
//使用System.setOut方法改变输出语句的目的地，改为参数中传递的打印流的目的地
```



## 网络编程

软件结构：

* B/S结构：Browser/Server结构
* C/S结构：Client/Server结构



### 网络通信协议

> 通过计算机网络可以使多台计算机实现链接，位于同一个网络中的计算机在进行连接和通信时需要遵守一定的规则，这就好比在道路中行驶的汽车一定要遵守交通规则一样。
>
> 在计算机网络中，这些连接和通信的规则被成为网路通信协议，它对数据的传输格式、传输速率、传输步骤等做了统一规定，通信双方必须同时遵守才能完成数据交换。

TCP/IP协议

> 传输控制协议/因特网互联协议（Transmission Control Protocol/Internet Protocol），是Internet最基本、最广泛的协议。它定义了计算机如何连入因特网，以及数据如何在他们之间传输的标准。
>
> 他的内部包含一系列的用于处理数据通信的协议，并采用了4层的分层模型，每一层都呼叫他的下一层所提供的协议来完成自己的需求。

![image-20210113082512468](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210113082512468.png)



#### 网络通信协议的分类

* UDP：用户数据报协议（User Datagram Protocol）

  * 是无连接通信协议：在数据传输时，数据的发送端和接收端不建立逻辑连接。
    * 当一台计算机向另一台计算机发送数据时，发送端不会确认接收端是否存在，就会发出数据，同样接收端在收到数据时，也不会向发送端反馈是否收到数据。
  * 消耗资源小，通信效率高
    * 所以常用于音频、视频和普通数据的传输
  * 由于UDP的面向无连接性，不能保证数据的完整性
    * 因此重要数据传输不建议使用UDP
  * 发送的数据被限制在64kb以内，超出则不能发送

* TCP：传输控制协议（Transmission Control Protocol）

  * 是面向连接的通信协议

    * 传输数据之前，在发送端和接收端建立逻辑连接，然后再传输数据，提供了两台计算机之间可靠无差错的数据传输

  * 由客户端向服务器发出连接请求，每次连接的创建都需要经过“3次握手”

    * 三次握手：TCP协议中，在发送数据的准备阶段，客户端和服务器之间的三次交互，以保障连接的可靠。

      * 第一次握手，客户端向服务器发出连接请求，等待服务器确认。
      * 第二次握手，服务器端向客户端回送一个响应，通知客户端收到了连接请求。
      * 第三次握手，客户端再次向服务器端发送确认信息，确认连接，整个交互过程如下图所示。

      ![image-20210113084305595](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210113084305595.png)

  * 可以保证传输数据的安全，应用十分广泛，如下载文件，浏览网页等

#### 网络编程三要素

三要素：

* 协议
* IP地址
* 端口号

-----------------------------

##### IP地址

> * 互联网协议地址（Internet Protocol Address）
> * 用来给网络中的计算机设备做唯一的编号

IP地址分类：

* IPv4：是一个32位的二进制数，通常被分为4个字节，表示成`a.b.c.d`的形式，例如192.168.65.100，其中a\b\c\d都是0~255之间的十进制整数，那么最多可以表示42亿个
* IPv6：IP地址需求量越来越大，分配变得紧张。为了扩大地址空间，通过IPv6重新定义地址空间，采用128位地址长度，每16个字节一组，分成8组十六进制数，表示成`ABCD:EF01:2345:6789:ABCD:EF01:2345:6789`

常用命令：

1. 查看本机IP地址，在控制台输入：

```
ipconfig
```

2. 检查网络是否连通，在控制台输入：

```
ping IP地址
ping 220.181.59.251
```

3. 特殊的IP地址

   本机IP地址：

   * 127.0.0.1
   * localhost

   ```
   ping 127.0.0.1
   ping localhost
   ```



##### 端口号

计算机与计算机之间的连接，依靠IP号进行查找。

连接后计算机1将数据传递给计算机2，但计算机1的QQ数据需要找到计算机2的QQ对应着接收数据，如何保证数据到达另一台计算机可以准确地找到对方计算机上的软件，就需要端口号来实现。

* 端口号是一个逻辑端口，我们无法直接看到，可以使用一些软件查看端口号。
* 当我们使用网络软件，打开的时候**操作系统就会为网络软件分配一个随机的端口号**，或者网络软件**在打开的时候和系统要指定的端口号**。
* 端口号是由两个字节组成，取值范围在0~65535之间

注意：

* 1024之前的端口号已经被系统分配给已知网络软件，不能使用
* 网络软件的端口号不能重复

![image-20210113091456729](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210113091456729.png)

🙋‍ IP地址+端口号，可以保证数据正确无误的发送到对方计算机的指定软件上

🙋‍ 常用端口号：

1. 网络端口 —— 80
   * www.baidu.com:80
2. 数据库：
   * mysql —— 3306
   * oracle —— 1521
3. Tomcat服务器 —— 8080



### TCP通信程序

> TCP通信能实现两台计算机之间的数据交互。
>
> 通信的两端严格区分为：
>
> 1. 客户端（Client）
> 2. 服务端（Server）

两端通信时步骤：

1. 服务端程序，需要事先启动，等待客户端的连接
2. 客户端主动连接服务器端，连接成功才能通信，服务端不可以主动连接客户端

![image-20210113103036070](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210113103036070.png)

![image-20210113103513460](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210113103513460.png)



在Java中，提供了两个类用于实现TCP通信程序：

1. 客户端：`java.net.Socket`类表示，创建`socket`对象，向服务端发出连接请求，服务端响应请求，两者建立连接开始通信
2. 服务端：`java.net.ServerSocket`类表示，创建`ServerSocket`对象，相当于开启一个服务，并等待客户端的连接

#### 表示客户端的类

java.net.Socket

> 此类实现客户端套接字。
>
> 套接字：两台机器间通信的端点，是包含了IP地址和端口号的网络单位。

构造方法：

```java
//创建一个流套接字并将其连接到指定主机上的指定端口号
Socket(String host, int port)
/*
* 参数：
* 	String host：服务器主机的名称/服务器的IP地址
*	int port：服务器的端口号
*/
```

成员方法：

```java
OutputStream getOutputStream()	//返回此套接字的输出流
InputStream getInputStream()	//返回此套接字的输入流
void close()		//关闭此套接字
```

实现步骤：

1. 创建一个客户端对象socket，构造方法绑定服务器的ip地址和端口号
2. 使用socket对象中的方法getOutputStream()获取网络字节输出流OutputStream对象
3. 使用网络字节输出流OutputStream对象中的方法write，给服务器发送数据
4. 使用Socket对象中的方法getInputStream()获取网络字节输入流InputStream对象
5. 使用网络字节输入流InputStream对象中的方法read，读取服务器回写的数据
6. 释放资源（socket）

注意：

1. 客户端和服务端进行交互，必须使用Socket中提供的网络流，不能使用自己创建的流对象
2. 当创建的客户端对象Socket的时候，就回去请求服务器和服务器进行3次握手建立连接通路；如果服务器已经启动，就可以进行交互了

#### 表示服务器的类

java.net.ServerSocket

> 此类实现服务器套接字

构造方法：

```java
//创建绑定到特定端口的服务器套接字
ServerSocket(int port)
```

🙋‍ 服务器端必须明确一件事情，就是知道是哪个客户端请求的服务器

所以存在accept方法获取到请求的客户端对象Socket

成员方法：

```java
//侦听并接收到此套接字的连接
Socket accept()
```

实现步骤：

1. 创建服务器ServerSocket对象和系统要指定的端口号
2. 使用ServerSocket对象中的方法，获取到请求的客户端对象Socket
3. 使用Socket对象中的方法getInputStream()获取网络字节输入流InputStream()
4. 使用网络字节输入流IntputStream()对象中的方法read，读取客户端发送的数据
5. 使用socket对象中的方法getOutputStream()获取网络字节输出流OutputStream对象
6. 使用网络字节输出流OutputStream对象中的方法write，给客户端回写数据
7. 释放资源（Socket，serverSocket）

💦 交互：

```java
//客户端
public class TCPClient {
    public static void main(String[] args) throws IOException {
        Socket socket = new Socket("127.0.0.1",8888);
        OutputStream os = socket.getOutputStream();
        os.write("你好服务器".getBytes());
        InputStream is = socket.getInputStream();
        byte[] bytes = new byte[1024];
        int len = is.read(bytes);
        System.out.println(new String(bytes,0,len));
        socket.close();
    }
}

//服务器
public class TCPServer{
    public static void main(String[] args) throws IOException {
        ServerSocket server = new ServerSocket(8888);
        Socket socket = server.accept();
        InputStream is = socket.getInputStream();
        byte[] bytes = new byte[1024];
        int len = is.read(bytes);
        System.out.println(new String(bytes,0,len));
        OutputStream os = socket.getOutputStream();
        os.write("getInfo".getBytes());
        server.close();
        socket.close();
    }
}
```

#### 案例 —— TCP通信的文件上传

![image-20210113135122241](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210113135122241.png)

```java
//客户端代码
public class TCPClient {
    public static void main(String[] args) throws IOException{
        //创建客户端对象，构造方法绑定服务器ip地址和端口号
        Socket socket = new Socket("127.0.0.1",8888);
        //创建本地字节输入流，绑定数据源
        FileInputStream fis = new FileInputStream("E:\\我的坚果云\\Java\\test.txt");
        //用客户端对象的方法获取网络字节输出流对象
        OutputStream o = socket.getOutputStream();
        //读取本地字节输入流，写入网络字节输出流
        int len = 0;
        byte[] bytes = new byte[1024];
        while ((len = fis.read(bytes)) != -1) {
            o.write(bytes,0,len);
        }
		
        //给服务器一个结束标记
        socket.shutdownOutput();
        //读服务器回写的数据
        InputStream is = socket.getInputStream();
        while ((len = is.read(bytes)) != -1) {
            System.out.println(new String(bytes,0,len));
        }

        fis.close();
        socket.close();
    }
}

//服务器代码
public class TCPServer {
    public static void main(String[] args) throws IOException {
        //创建一个服务器对象
        ServerSocket server = new ServerSocket(8888);
        //使用服务器对象的方法accept获取客户端socket对象
        Socket socket = server.accept();
        //读数据
        InputStream is = socket.getInputStream();
        //建立文件夹
        File file = new File("d:\\upload");
        if(!file.exists()){
            file.mkdir();
        }
        //保存数据
        FileOutputStream fos = new FileOutputStream(file + "\\test.txt");
        int len = 0;
        byte[] bytes = new byte[1024];
        while((len=is.read(bytes))!=-1){
            fos.write(bytes,0,len);
        }
        //回传数据
        OutputStream os = socket.getOutputStream();
        os.write("Upload complete".getBytes());

        fos.close();
        socket.close();
        server.close();
    }
}
```

🙋‍ 客户端与服务器端多次使用了read方法，但

![image-20210113151011363](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210113151011363.png)

![image-20210113151018254](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210113151018254.png)

read进入了阻塞状态，导致回传后的内容无法显示；

❗ 为了解决阻塞问题：

🙋‍ 加入了方法 `void shutdownInput()` ，在上传完文件后，给服务器一个结束标记

```
public void shutdownOutput()
//禁用此套接字的输出流
//进度TCP套接字，任何以前写入的数据都将被发送，并且后跟TCP的正常连接终止序列。如果在套接字上调用shutdownOutput()后写入套接字输出流，则该流将抛出IOException
```

对于该案例的优化：

1. 服务器不应该完成一次工作后就关闭，应该设计为监听状态（死循环accept方法与后面的保存数据等等内容），服务器不用关闭（不用写server.close()）
2. 保存的文件名不应该固定，可以使用当前时间的组合

```java
String fileName = "pic"+System.currentTimeMillis()+new Random().nextInt(999999)+".txt";
```



模拟BS服务器分析：

![image-20210113160142923](C:\Users\79260\AppData\Roaming\Typora\typora-user-images\image-20210113160142923.png)

