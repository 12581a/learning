# Java基础

## 1 JVM 、JDK 和 JRE

* Java虚拟机（JVM）是运行 Java 字节码的虚拟机。JVM有针对不同系统的特定实现（Windows，Linux，macOS），目的是使用相同的字节码，它们都会给出相同的结果。 
* JDK是Java Development Kit，它是功能齐全的Java SDK。它拥有JRE所拥有的一切，还有编译器（javac）和工具（如javadoc和jdb）。它能够创建和编译程序。 
* JRE 是 Java运行时环境。它是运行已编译 Java 程序所需的所有内容的集合，包括 Java虚拟机（JVM），Java类库，java命令和其他的一些基础构件。 

Java程序经过JDK的javac命令编译为字节码文件，即扩展名为.class的文件,然后由JVM加载翻译为机器代码。

Java 语言通过字节码的方式，在一定程度上解决了传统解释型语言执行效率低的问题，同时又保留了解释型语言可移植的特点。

## 2 Java基本数据类型与数组

### 2.1 基本数据类型

| 类型    | 型别   | 字节  | 取值范围                           | 默认值  |
| ------- | ------ | ----- | ---------------------------------- | ------- |
| byte    | 整型   | 1byte | -2<sup>7 </sup>~ 2<sup>7</sup>-1   | 0       |
| short   | 整型   | 2byte | -2<sup>15</sup> ~ 2<sup>15</sup>-1 | 0       |
| int     | 整型   | 4byte | -2<sup>31</sup> ~ 2<sup>31</sup>-1 | 0       |
| long    | 整型   | 8byte | -2<sup>63</sup> ~ 2<sup>63</sup>-1 | 0L      |
| float   | 浮点型 | 4byte | ……                                 | 0.0f    |
| double  | 浮点型 | 8byte | ……                                 | 0.0d    |
| char    | 字符型 | 2byte | ……                                 | 'u0000' |
| boolean | 布尔型 | 1byte | ……                                 | false   |

### 2.2 原码，反码，补码的概念 

>  **原码**:是最简单的机器数表示法。用最高位表示符号位，‘1’表示负号，‘0’表示正号。其他位存放该数的二进制的绝对值。
>
>  **反码：**正数的反码还是等于原码。负数的反码就是他的原码除符号位外，按位取反。
>
>  **补码：**正数的补码等于他的原码。负数的补码等于反码+1。 

计算机没法直接做减法的，它的减法是通过加法来实现的。1010 ：最高位为‘1’,表示这是一个负数，其他三位为‘010’，  即0×2<sup>0</sup>+1×2<sup>1</sup>×+0×2<sup>2</sup>=2 ， 所以1010表示十进制数-2。

正数之间的加法通常是不会出错的，因为它就是一个很简单的二进制加法。而正数与负数相加，或负数与负数相加，就要引起莫名其妙的结果，这都是该死的符号位引起的。0分为`+0`和`-0`也是因他而起。所以原码，虽然直观易懂，易于正值转换。但用来实现加减法的话，运算规则总归是太复杂，于是反码来了。

我们知道，原码最大的问题就在于一个数加上他的相反数不等于零。

例如：`0001+1001=1010 (1+(-1)=-2)` `0010+1010=1100 (2+(-2)=-4)`

于是反码的设计思想就是冲着解决这一点，既然一个负数是一个正数的相反数，那我们干脆用一个正数按位取反来表示负数试试。  

byte 表示一个字节，一个字节是 8 位，最高位是符号位。

那么 8 位能表示的最大值就是 0111 1111，换算成十进制就是 127。

最小的负数就是1000 0000，（最大的负数是 1111 1111 是负数-1的补码），换算成十进制就是 -128， 10000000 是最小负数的补码表示形式，我们把补码计算步骤倒过来就即可。1000 0000 减 1 得  0111 1111 然后取反 1000 0000 因为负数的补码是其绝对值取反，即 1000 0000 为最小负数的绝对值，而 1000 0000 的十进制表示是 128，所以最小负数是 -128

### 2.3 数组

声明并且创建一个长度为2的数组

`int Array[] = new int[2]; `

可以为数组分配元素`Array[0] = 1`,`Array[1] = 2`

在声明数组的同时也可以给每个数组的元素一个初始值

`int Array = {1,2,3,4,5,6,7,8,9}`

java采用”数组的数组“声明多维数组，一个二维数组是由若干个一维数组构成的。

`int a[][] = new int[][];`

## 3 java面向对象编程特性

一个java应用程序是由若干个类所构成的，一个class类里面的成员变量有默认值，而局部变量没有默认值。

如果类中没有编写构造方法，则默认有一个无参数的构造方法, 构造方法主要作用是完成对类对象的初始化工作。 

创建一个对象包括对象的声明和对象变量的分配。当对象声明之后，对象变量的内存中还没有任何的数据，对象变量为一个空对象，利用`new`运算符计算出一个十六进制的引用后，对象就诞生了。

`Object o = new Object();`

### 3.1 封装

> 封装把一个对象的属性私有化，同时提供一些可以被外界访问的属性的方法，如果属性不想被外界访问，我们大可不必提供方法给外界访问. 

### 3.2 继承

>  继承是使用已存在的类的定义作为基础建立新类的技术，新类的定义可以增加新的数据或新的功能，也可以用父类的功能，但不能选择性地继承父类。通过使用继承我们能够非常方便地复用以前的代码。 

子类拥有父类对象所有的属性和方法（包括私有属性和私有方法），但是父类中的私有属性和方法子类是无法访问,只是拥有。

**对象增强的手段：继承、装饰者模式、动态代理**

* **方法重写**

  重写是子类对父类的允许访问的方法的实现过程进行重新编写,发生在子类中，方法名、参数列表必须相同，返回值范围小于等于父类，抛出的异常范围小于等于父类，访问修饰符范围大于等于父类。另外，如果父类方法访问修饰符为 private 则子类就不能重写该方法。也就是说方法提供的行为改变，而方法的外貌并没有改变。

  方法重写的目的可以隐藏继承的方法，子类通过方法的重写可以把父类的状态和行为改变为自身的状态和行为

  **重载**： 发生在同一个类中，方法名必须相同，参数类型不同、个数不同、顺序不同，方法返回值和访问修饰符可以不同。 

  `float hello(int a,int b){return a+b}`

  `float hello(long a,int b){return a-b}`

  `float hello(double a,int b){return a*b}`

* **对象的上转型对象**

  假设Animal是Tiger的父类

  `Animal a = new Tiger();`

  对象的上转型对象的实体是由子类创建的，上转型对象不能操作子类新增的成员变量，不能调用子类新增的方法，可以访问子类继承或隐藏的成员变量。

### 3.3 多态

> 多态就是指程序中定义的引用变量所指向的具体类型和通过该引用变量发出的方法调用在编程时并不确定，而是在程序运行期间才确定，即一个引用变量到底会指向哪个类的实例对象，该引用变量发出的方法调用到底是哪个类中实现的方法，必须在由程序运行期间才能决定。
>
> 在Java中有两种形式可以实现多态：继承（多个子类对同一方法的重写）和接口（实现接口并覆盖接口中同一方法）

## 4 接口与抽象类

1. 接口的方法默认是 public，所有方法在接口中不能有实现(Java 8 开始接口方法可以有默认实现），而抽象类可以有非抽象的方法。
2. 接口中除了static、final变量，不能有其他变量，而抽象类中则不一定。
3. 一个类可以实现多个接口，但只能实现一个抽象类。接口自己本身可以通过extends关键字扩展多个接口。
4. 接口方法默认修饰符是public，抽象方法可以有public、protected和default这些修饰符（抽象方法就是为了被重写所以不能使用private关键字修饰！也不允许使用static修饰）。
5. 从设计层面来说，抽象是对类的抽象，是一种模板设计，而接口是对行为的抽象，是一种行为的规范。

接口中的常量与方法

`public static final int MAX = 100`

`public abstract int sum(int a,int b)`

**接口回调**

接口是java中的一种重要的数据类型，用接口声明的变量称作接口变量。接口属于引用型变量，接口变量中可以存放实现该接口的类的实例的引用。

`Com object = new ImpleCom();`

## 5 常见关键字总结

### final

> final关键字主要用在三个地方：变量、方法、类。
>
> 1. 对于一个final变量，如果是基本数据类型的变量，则其数值一旦在初始化之后便不能更改；如果是引用类型的变量，则在对其初始化之后便不能再让其指向另一个对象。
> 2. 当用final修饰一个类时，表明这个类不能被继承。final类中的所有成员方法都会被隐式地指定为final方法。
> 3. 使用final方法的原因有两个。第一个原因是把方法锁定，以防任何继承类修改它的含义；第二个原因是效率。在早期的Java实现版本中，会将final方法转为内嵌调用。但是如果方法过于庞大，可能看不到内嵌调用带来的任何性能提升（现在的Java版本已经不需要使用final方法进行这些优化了）。类中所有的private方法都隐式地指定为final。

### static

> static 关键字主要有以下四种使用场景：
>
> 1. 修饰成员变量和成员方法: 被 static 修饰的成员属于类，不属于单个这个类的某个对象，被类中所有对象共享，可以并且建议通过类名调用。被static 声明的成员变量属于静态成员变量，静态变量存放在 Java 内存区域的方法区。调用格式：类名.静态变量名类名.静态方法名()
> 2. 静态代码块: 静态代码块定义在类中方法外, 静态代码块在非静态代码块之前执行(静态代码块—>非静态代码块—>构造方法)。 该类不管创建多少对象，静态代码块只执行一次.
> 3. 静态内部类（static修饰类的话只能修饰内部类）： 静态内部类与非静态内部类之间存在一个最大的区别: 非静态内部类在编译完成之后会隐含地保存着一个引用，该引用是指向创建它的外围类，但是静态内部类却没有。没有这个引用就意味着：1. 它的创建是不需要依赖外围类的创建。2. 它不能使用任何外围类的非static成员变量和方法。
> 4. 静态导包(用来导入类中的静态资源，1.5之后的新特性): 格式为：import static 这两个关键字连用可以指定导入某个类中的指定静态资源，并且不需要使用类名调用类中静态成员，可以直接使用类中静态成员变量和成员方法。

下面的运行结果是：静态代码块！--非静态代码块！--默认构造方法！--静态方法中的内容! --静态方法中的代码块

```java
public class Test {
    public Test() {
        System.out.print(默认构造方法！--);
    }

     非静态代码块
    {
        System.out.print(非静态代码块！--);
    }
     静态代码块
    static {
        System.out.print(静态代码块！--);
    }

    public static void test() {
        System.out.print(静态方法中的内容! --);
        {
            System.out.print(静态方法中的代码块！--);
        }

    }
    public static void main(String[] args) {

        Test test = new Test();   
        Test.test();静态代码块！--静态方法中的内容! --静态方法中的代码块！--
    }
```

### this

> this关键字用于引用类的当前实例。 例如：
>
> **class** **Manager** {   
>
> ​	Employees[] employees;         
>
> ​	**void** **manageEmployees**() {
>
> ​			**int** totalEmp = **this**.employees.length;
>
> ​	       System.out.println("Total employees: " + totalEmp); 
>
> ​	       **this**.report();
>
> }         
>
> ​	**void** **report**() { } }
>
> 在上面的示例中，this关键字用于两个地方：
>
> * this.employees.length：访问类Manager的当前实例的变量。
>
> - this.report（）：调用类Manager的当前实例的方法。
>
> 此关键字是可选的，这意味着如果上面的示例在不使用此关键字的情况下表现相同。 但是，使用此关键字可能会使代码更易读或易懂。

### super

> super关键字用于从子类访问父类的变量和方法。 例如：
>
> **public** **class** **Super** {    
>
> ​		**protected** **int** number; 
>
> ​        **protected** **showNumber**() {        
>
> ​				System.out.println("number = " + number);    } 
>
> ​					}  
>
> ​	**public** **class** **Sub** **extends** Super {    
>
> ​	**void** **bar**() {        
>
> ​		**super**.number = 10;        
>
> ​		**super**.showNumber();    } 
>
> }
>
> 在上面的例子中，Sub 类访问父类成员变量 number 并调用其其父类 Super 的 showNumber（） 方法。

使用 this 和 super 要注意的问题：

- 在构造器中使用 super（） 调用父类中的其他构造方法时，该语句必须处于构造器的首行，否则编译器会报错。另外，this 调用本类中的其他构造方法时，也要放在首行。
- this、super不能用在static方法中。

[参考链接]( https://gitee.com/SnailClimb/JavaGuide/blob/master/docs/java/Basis/final、static、this、super.md )

## 6 自动拆装箱

**装箱**：将基本类型用它们对应的引用类型包装起来；

`Integer i = 10 //自动装箱`

**拆箱**：将包装类型转换为基本数据类型；

`int b = i //自动装箱`

自动装箱都是通过包装类的`valueOf()`方法来实现的.自动拆箱都是通过包装类对象的`xxxValue()`来实现的。`Integer integer=Integer.valueOf(1);` 

`int i=integer.intValue();` 

Java提供了与基本的数据类型相关的类，实现了对基本数据类型的封装。这些类在java.lang包中，分别是Byte、Boolean、Integer、Short、Long、Float、Double、Character。

为什么还要提供包装类呢？因为Java是一种面向对象语言，很多地方都需要使用对象而不是基本数据类型。比如，在集合类中，我们是无法将int 、double等类型放进去的。因为集合的容器要求元素是Object类型。为了让基本类型也具有对象的特征，就出现了包装类型，它相当于将基本类型“包装起来”，使得它具有了对象的性质，并且为其添加了属性和方法，丰富了基本类型的操作。

## 7.访问权限

Java 中一共有四种访问权限控制，其权限控制的大小情况是这样的：public > protected > default(包访问权限) > private  

| 权限      | 同类 | 同包 | 不同包子类 | 不同包非子类 |
| --------- | ---- | ---- | ---------- | ------------ |
| public    | √    | √    | √          | √            |
| protected | √    | √    | √          | ×            |
| default   | √    | √    | ×          | ×            |
| private   | √    | ×    | ×          | ×            |

## 8 == 与 equals

**==** : 它的作用是判断两个对象的地址是不是相等。即判断两个对象是不是同一个对象(基本数据类型==比较的是值，引用数据类型==比较的是内存地址)。

**equals()** : 它的作用也是判断两个对象是否相等。但它一般有两种使用情况：

- 情况1：类没有覆盖 equals() 方法。则通过 equals() 比较该类的两个对象时，等价于通过“==”比较这两个对象。
- 情况2：类覆盖了 equals() 方法。一般，我们都覆盖 equals() 方法来比较两个对象的内容是否相等；若它们的内容相等，则返回 true (即，认为这两个对象相等)。

> ```java
> public boolean equals(Object obj) {
> 	return (this== obj);
> }
> ```

## 9 常用实用类

### 9.1 String类

在Java中，String是一个引用类型，它本身也是一个class。Java把String类定义为final类，因此用户不能扩展String类，即String类不可以有子类。 

`String s1 = "Hello!";`

 实际上字符串在`String`内部是通过一个`char[]`数组表示的，因此，按下面的写法也是可以的： 

`String s2 = new String(new char[] {'H', 'e', 'l', 'l', 'o', '!'});`

* **String类常用的方法：**

（1）是否包含字串	`"Hello".contains("ll"); // true`

（2）判断当前的String对象的字符序列前缀是否是参数指定的String对象的字符序列

`"Hello,world!".startsWith("He"); // true`

`"Hello,world!".endsWith("!"); // true`

（3）`public boolean contains(String s)`

（4）indexOf(String str)从当前String对象的字符序列的0索引位置开始检索首次出现str的字符序列的位置，并返回该位置

`String s = "I am a good cat";`

`s.indexof("a"); // 值是2`

`s.lastIndexOf("a"); // 值是13`

（5）复制得到字符序列中的start位置至end-1位置上的字符

`"Hello".substring(2, 4); "ll"`

（6）使用trim()方法可以移除字符串首尾空白字符。空白字符包括空格，\t，\r，\n：

`"  \tHello\r\n ".trim(); // "Hello"`

String还提供了isEmpty()和isBlank()来判断字符串是否为空和空白字符串：

`"".isEmpty(); // true，因为字符串长度为0`
`"  ".isEmpty(); // false，因为字符串长度不为0`
`"  \n".isBlank(); // true，因为只包含空白字符`
 Hello ".isBlank(); // false，因为包含非空白字符`

（7）替换子串

`String s = "hello";`
`s.replace('l', 'w'); // "hewwo"，所有字符'l'被替换为'w'`

可以通过正则表达式来替换

`String s = "A,,B;C ,D";`
`s.replaceAll("[\\,\\;\\s]+", ","); // "A,B,C,D"`

（8）分割字符串

`String s = "A,B,C,D";`
`String[] ss = s.split("\\,"); // {"A", "B", "C", "D"}`

* **字符串与基本数据类型的转化**

要把任意基本类型或引用类型转换为字符串，可以使用静态方法`valueOf()`。这是一个重载方法，编译器会根据参数自动选择合适的方法。

`String s = String.valueOf(123456.78);`

 要把字符串转换为其他类型，就需要根据情况。例如，把字符串转换为`int`类型：

 `int n1 = Integer.parseInt("123"); // 123`

String和char[]类型可以互相转换，方法是：

`char[] cs = "Hello".toCharArray(); // String -> char[]`
`String s = new String(cs); // char[] -> String`

#### String类的特性

> 1. String声明为final，不可以被继承，实现了Serializable、Comparable接口。在JDK8以前定义了char[]，存储字符串数据。JDK9改为byte[]
>
> 2. String具有不可变性。对String字符串进行操作时，需要重新指定内存区域。通过字面量的方式给一个字符串赋值，此时的字符串声明在字符串常量池中。
>
>    注：String Pool是一个固定大小的HashTable，可以使用 **-XX:StringTableSize**进行调节。相当于缓存，JDK1.7之后在堆中存储。
>
> 3. 面试题
>
>    String str = new String(“ab”)创建了几个对象？ 
>
>    答案：2个。一个在字符串常量池中，一个在堆中
>
>    String str = new String(“a”) + new String(“b”)创建了几个对象？
>
>    答案： 5个。  通过分析字节码文件可知;
>
>    ①new StringBuilder()
>
>    ②new String("a")
>
>    ③常量池中的"a"
>
>    ④new String("b")
>
>    ⑤常量池中的"b"
>
> 4.  **intern()**方法
>
>    调用这个方法之后就是去看当前字符串是否在常量池中存在，如果存在，直接返回该字符串在字符串常量池中所对应的地址给栈中要引用这个字符串的变量。如果不存在， 直接将堆中（不是字符串常量池中）该字符串的地址复制到字符串常量池中，这样字符串常量池就有了该字符串的地址引用，也可以说此时字符串常量池中的字符串只是一个对 堆中字符串对象的引用，它们两个的地址相同，然后再把这个地址返回给栈中要引用这个字符串的变量。 

### 9.2 StringBuilder类

Java编译器对String做了特殊处理，使得我们可以直接用+拼接字符串。虽然可以直接拼接字符串，但是，在循环中，每次循环都会创建新的字符串对象，然后扔掉旧的字符串。这样，绝大部分字符串都是临时对象，不但浪费内存，还会影响GC效率。为了能高效拼接字符串，Java标准库提供了`StringBuilder`，它是一个可变对象，可以预分配缓冲区，这样，往`StringBuilder`中新增字符时，不会创建新的临时对象：

String对象的字符序列是不可修改的，与String类不同的是，StringBuffer类的对象的实体的内存空间可以自动的改变大小，便于存放一个可变的字符序列。

* **常用方法**

（1）append方法

`StringBuffer s = new StringBuffer("我喜欢");`

`s.append("打篮球");`

（2）将参数str指定的字符序列插入到参数index指定的位置

`insert(int index, String str)`

（3）将对象实体中的字符序列翻转，并返回当前对象的引用

`StringBuffer s = new StringBuffer("Hello"); `

`s.reverse(); //olleH`

（4）替换

`StringBuffer replace(int startIndex,int endIndex, String str);`

### 9.3 BigInteger

在Java中，由CPU原生提供的整型最大范围是64位long型整数。使用long型整数可以直接通过CPU指令进行计算，速度非常快。如果我们使用的整数范围超过了long型怎么办？这个时候，就只能用软件来模拟一个大整数。java.math.BigInteger就是用来表示任意大小的整数。BigInteger内部用一个int[]数组来模拟一个非常大的整数。

`BigInteger bi = new BigInteger("1234567890");`
`System.out.println(bi.pow(5)); // 2867971860299718107233761438093672048294900000`

 对`BigInteger`做运算的时候，只能使用实例方法，例如，加法运算： 

`BigInteger i1 = new BigInteger("1234567890");`
`BigInteger i2 = new BigInteger("12345678901234567890");`
`BigInteger sum = i1.add(i2); // 12345678902469135780`

和`long`型整数运算比，`BigInteger`不会有范围限制，但缺点是速度比较慢。也可以把`BigInteger`转换成`long`型

`BigInteger i = new BigInteger("123456789000");`
`System.out.println(i.longValue()); // 123456789000`

可以把BigInteger转换成基本类型。如果BigInteger表示的范围超过了基本类型的范围，转换时将丢失高位信息，即结果不一定是准确的。如果需要准确地转换成基本类型，可以使用intValueExact()、longValueExact()等方法，在转换时如果超出范围，将直接抛出ArithmeticException异常。

### 9.4 BigDecimal

和`BigInteger`类似，`BigDecimal`可以表示一个任意大小且精度完全准确的浮点数。

```java
BigDecimal bd = new BigDecimal("123.4567");
System.out.println(bd.multiply(bd)); // 15241.55677489
```

 `BigDecimal`用`scale()`表示小数位数，例如 

`BigDecimal d1 = new BigDecimal("123.4500"); //4,4位小数 `

《阿里巴巴Java开发手册》中提到：浮点数之间的等值判断，基本数据类型不能用==来比较，包装数据类型不能用 equals 来判断。 具体原理和浮点数的编码方式有关，出现了精度丢失，我们下面直接上实例：

```java
float a = 1.0f - 0.9f;
float b = 0.9f - 0.8f;
System.out.println(a);// 0.100000024
System.out.println(b);// 0.099999964
System.out.println(a == b);// false
```

对BigDecimal做加、减、乘时，精度不会丢失，但是做除法时，存在无法除尽的情况，这时，就必须指定精度以及如何进行截断。通过 setScale方法设置保留几位小数以及保留规则。保留规则有挺多种，不需要记，IDEA会提示。

```java
BigDecimal m = new BigDecimal("1.255433");
BigDecimal n = m.setScale(3,BigDecimal.ROUND_HALF_DOWN);
System.out.println(n);// 1.255
```

* **比较BigDecimal**

在比较两个`BigDecimal`的值是否相等时，要特别注意，使用`equals()`方法不但要求两个`BigDecimal`的值相等，还要求它们的`scale()`相等

```java
BigDecimal d1 = new BigDecimal("123.456");
BigDecimal d2 = new BigDecimal("123.45600");
System.out.println(d1.equals(d2)); // false,因为scale不同
System.out.println(d1.equals(d2.stripTrailingZeros())); // true,因为d2去除尾部0后scale变为2
System.out.println(d1.compareTo(d2)); // 0
```

 必须使用`compareTo()`方法来比较，它根据两个值的大小分别返回负数、正数和`0`，分别表示小于、大于和等于 

如果查看`BigDecimal`的源码，可以发现，实际上一个`BigDecimal`是通过一个`BigInteger`和一个`scale`来表示的，即`BigInteger`表示一个完整的整数，而`scale`表示小数位数：

```
public class BigDecimal extends Number implements Comparable<BigDecimal> {
    private final BigInteger intVal;
    private final int scale;
}
```

`BigDecimal`也是从`Number`继承的，也是不可变对象。

* **总结**

BigDecimal 主要用来操作（大）浮点数，BigInteger 主要用来操作大整数（超过 long 类型）。

BigDecimal 的实现利用到了 BigInteger, 所不同的是 BigDecimal 加入了小数位的概念

比较`BigDecimal`的值是否相等，必须使用`compareTo()`而不能使用`equals()`。 

### 9.5 Arrays

Arrays类的常见操作

1. 排序 : sort()
2. 查找 : binarySearch()
3. 比较: equals()
4. 填充 : fill()
5. 转列表: asList()
6. 转字符串 : toString()
7. 复制: copyOf()

`Arrays.asList()`在平时开发中还是比较常见的，我们可以使用它将一个数组转换为一个List集合。 `Arrays.asList()`是泛型方法，传入的对象必须是对象数组。 

```java
String[] myArray = { "Apple", "Banana", "Orange" }； 
List<String> myList = Arrays.asList(myArray);
//上面两个语句等价于下面一条语句
List<String> myList = Arrays.asList("Apple","Banana", "Orange");
```

**如何将数组转换为ArrayList**

1. 最简便的方法

`List list = new ArrayList<>(Arrays.asList("a", "b", "c"))`

2. 使用 Java8 的Stream

```java
Integer [] myArray = { 1, 2, 3 };
List myList = Arrays.stream(myArray).collect(Collectors.toList());
//基本类型也可以实现转换（依赖boxed的装箱操作）
int [] myArray2 = { 1, 2, 3 };
List myList = Arrays.stream(myArray2).boxed().collect(Collectors.toList());
```

### 9.6 Collections

```java
void reverse(List list)//反转
void shuffle(List list)//随机排序
void sort(List list)//按自然排序的升序排序
void sort(List list, Comparator c)//定制排序，由Comparator控制排序逻辑
void swap(List list, int i , int j)//交换两个索引位置的元素
void rotate(List list, int distance)//旋转。当distance为正数时，将list后distance个元素整体移到前面。当distance为负数时，将 list的前distance个元素整体移到后面。

```

## 10集合

### 10.1 ArrayList

 `ArrayList`在内部使用了数组来存储所有元素。例如，一个ArrayList拥有5个元素，实际数组大小为`6`（即有一个空位，当添加一个元素并指定索引到`ArrayList`时，`ArrayList`自动移动需要移动的元素， 然后，往内部指定索引的数组位置添加一个元素，然后把`size`加`1`， 继续添加元素，但是数组已满，没有空闲位置的时候，`ArrayList`先创建一个更大的新数组，然后把旧数组的所有元素复制到新数组，紧接着用新数组取代旧数组，现在，新数组就有了空位，可以继续添加一个元素到数组末尾，同时`size`加`1`。

`List`接口，可以看到几个主要的接口方法：

- 在末尾添加一个元素：`void add(E e)`
- 在指定索引添加一个元素：`void add(int index, E e)`
- 删除指定索引的元素：`int remove(int index)`
- 删除某个元素：`int remove(Object e)`
- 获取指定索引的元素：`E get(int index)`
- 获取链表大小（包含元素的个数）：`int size()`

**List的遍历**

* **RandomAccess接口**

> `ArrayList` 实现了 `RandomAccess` 接口， 而 `LinkedList` 没有实现。为什么呢？我觉得还是和底层数据结构有关！`ArrayList` 底层是数组，而 `LinkedList` 底层是链表。数组天然支持随机访问，时间复杂度为 O（1），所以称为快速随机访问。链表需要遍历到特定位置才能访问特定位置的元素，时间复杂度为 O（n），所以不支持快速随机访问。`ArrayList` 实现了 `RandomAccess` 接口，就表明了他具有快速随机访问功能。 `RandomAccess` 接口只是标识，并不是说 `ArrayList` 实现 `RandomAccess` 接口才具有快速随机访问功能的！ 

实现了 `RandomAccess` 接口的list，优先选择普通 for 循环 ，其次 foreach,

未实现 `RandomAccess`接口的list，优先选择iterator遍历（foreach遍历底层也是通过iterator实现的,），大size的数据，千万不要使用普通for循环

普通for循环、foreach遍历、 iterator 遍历

```java
List<String> list = Arrays.asList("a","b","c");
for(int i = 0;i<list.size();i++){
	System.out.println(list.get(i));
}

for (String s: list) {
    System.out.println(s);
}

Iterator iterator = list.iterator();
while(iterator.hasNext()){
    String s = (String)iterator.next();
	System.out.println(s);
}
```

### 10.2 HashMap

`Map`是一种键-值映射表，当我们调用`put(K key, V value)`方法时，就把`key`和`value`做了映射并放入`Map`。当我们调用`V get(K key)`时，就可以通过`key`获取到对应的`value`。如果`key`不存在，则返回`null`。和`List`类似，`Map`也是一个接口，最常用的实现类是`HashMap`。 

**Map的遍历**

```java
Map<Object,String> map =new HashMap();
        map.put(null,"hello");
        map.put("k1","v1");
        Set set = map.entrySet();
        Iterator its = set.iterator();
        while(its.hasNext()){
            Map.Entry entry = (Map.Entry)its.next();
            Object key = entry.getKey();
            Object value = entry.getValue();
            System.out.println(key +" "+value);
}
```

```java
Set keySet = map.keySet();//获取键的集合
Iterator it = keySet.iterator();//迭代键的集合
    while(it.hasNext()) {
        Object key = it.next();
        Object value = map.get(key);//获取每个键所对应的值
        System.out.println(key+" "+value);
}
```

### 10.3 HashSet

* **HashSet如何检查重复**
  当你把对象加入HashSet时，HashSet会先计算对象的hashcode值来判断对象加入的位置，同时也会与其他加入的对象的hashcode值作比较，如果没有相符的hashcode，HashSet会假设对象没有重复出现。但是如果发现有相同hashcode值的对象，这时会调用equals（）方法来检查hashcode相等的对象是否真的相同。如果两者相同，HashSet就不会让加入操作成功。

**hashCode（）与equals（）的相关规定：**

1. 如果两个对象相等，则hashcode一定也是相同的
2. 两个对象相等,对两个equals方法返回true
3. 两个对象有相同的hashcode值，它们也不一定是相等的
4. 综上，equals方法被覆盖过，则hashCode方法也必须被覆盖
5. hashCode()的默认行为是对堆上的对象产生独特值。如果没有重写hashCode()，则该class的两个对象无论如何都不会相等（即使这两个对象指向相同的数据）。

### 10.4 List,Set,Map三者的区别？

- **List(对付顺序的好帮手)：** List接口存储一组不唯一（可以有多个元素引用相同的对象），有序的对象
- **Set(注重独一无二的性质):** 不允许重复的集合。不会有多个元素引用相同的对象。
- **Map(用Key来搜索的专家):** 使用键值对存储。Map会维护与Key有关联的值。两个Key可以引用相同的对象，但Key不能重复，典型的Key是String类型，但也可以是任何对象。

### 10.5 Arraylist 与 LinkedList 区别?

- **1. 是否保证线程安全：** `ArrayList` 和 `LinkedList` 都是不同步的，也就是不保证线程安全；

- **2. 底层数据结构：** `Arraylist` 底层使用的是 **`Object` 数组**；`LinkedList` 底层使用的是 **双向链表** 数据结构（JDK1.6之前为循环链表，JDK1.7取消了循环。注意双向链表和双向循环链表的区别）

- **3. 插入和删除是否受元素位置的影响：**

   ① **`ArrayList` 采用数组存储，所以插入和删除元素的时间复杂度受元素位置的影响。** 比如：执行`add(E e) `方法的时候， `ArrayList` 会默认在将指定的元素追加到此列表的末尾，这种情况时间复杂度就是O(1)。但是如果要在指定位置 i 插入和删除元素的话（`add(int index, E element) `）时间复杂度就为 O(n-i)。因为在进行上述操作的时候集合中第 i 和第 i 个元素之后的(n-i)个元素都要执行向后位/向前移一位的操作。

   ② **`LinkedList` 采用链表存储，所以对于`add(E e)`方法的插入，删除元素时间复杂度不受元素位置的影响，近似 O（1），如果是要在指定位置`i`插入和删除元素的话（`(add(int index, E element)`） 时间复杂度近似为`o(n))`因为需要先移动到指定位置再插入。**

- **4. 是否支持快速随机访问：** `LinkedList` 不支持高效的随机元素访问，而 `ArrayList` 支持。快速随机访问就是通过元素的序号快速获取元素对象(对应于`get(int index) `方法)。

- **5. 内存空间占用：** ArrayList的空 间浪费主要体现在在list列表的结尾会预留一定的容量空间，而LinkedList的空间花费则体现在它的每一个元素都需要消耗比ArrayList更多的空间（因为要存放直接后继和直接前驱以及数据）。

### 10.6 equals和hashCode

HashMap之所以能根据key直接拿到value，原因是它内部通过空间换时间的方法，用一个大数组存储所有value，并根据key直接计算出value应该存储在哪个索引。

我们放入`Map`的`key`是字符串`"a"`，但是，当我们获取`Map`的`value`时，传入的变量不一定就是放入的那个`key`对象。换句话讲，两个`key`应该是内容相同，但不一定是同一个对象。

```java
String key1 = "a";
Map<String, Integer> map = new HashMap<>();
map.put(key1, 123);
String key2 = new String("a");
map.get(key2); // 123
System.out.println(key1 == key2); // false
System.out.println(key1.equals(key2)); // true
```

在Map的内部，对key做比较是通过equals()实现的，这一点和List查找元素需要正确覆写equals()是一样的，即正确使用Map必须保证：作为key的对象必须正确覆写equals()方法。我们经常使用String作为key，因为String已经正确覆写了equals()方法。但如果我们放入的key是一个自己写的类，就必须保证正确覆写了equals()方法。

通过key计算索引的方式就是调用key对象的hashCode()方法，它返回一个int整数。HashMap正是通过这个方法直接定位key对应的value的索引，继而直接返回value。

因此，正确使用`Map`必须保证：

1. 作为`key`的对象必须正确覆写`equals()`方法，相等的两个`key`实例调用`equals()`必须返回`true`；
2. 作为`key`的对象还必须正确覆写`hashCode()`方法，且`hashCode()`方法要严格遵循以下规范：

- 如果两个对象相等，则两个对象的`hashCode()`必须相等；
- 如果两个对象不相等，则两个对象的`hashCode()`尽量不要相等；

即对应两个实例`a`和`b`：

- 如果`a`和`b`相等，那么`a.equals(b)`一定为`true`，则`a.hashCode()`必须等于`b.hashCode()`；
- 如果`a`和`b`不相等，那么`a.equals(b)`一定为`false`，则`a.hashCode()`和`b.hashCode()`尽量不要相等。

上述第一条规范是正确性，必须保证实现，否则`HashMap`不能正常工作。

而第二条如果尽量满足，则可以保证查询效率，因为不同的对象，如果返回相同的`hashCode()`，会造成`Map`内部存储冲突，使存取的效率下降。

编写`equals()`和`hashCode()`遵循的原则是：

`equals()`用到的用于比较的每一个字段，都必须在`hashCode()`中用于计算；`equals()`中没有使用到的字段，绝不可放在`hashCode()`中计算。

编写`equals()`方法如下：

```java
public boolean equals(Object o) {
    if (o instanceof Person) {
        Person p = (Person) o;
        return Objects.equals(this.name, p.name) && this.age == p.age;
    }
    return false;
}
```

因此，我们总结一下`equals()`方法的正确编写方法：

1. 先确定实例“相等”的逻辑，即哪些字段相等，就认为实例相等；
2. 用`instanceof`判断传入的待比较的`Object`是不是当前类型，如果是，继续比较，否则，返回`false`；
3. 对引用类型用`Objects.equals()`比较，对基本类型直接用`==`比较。

使用`Objects.equals()`比较两个引用类型是否相等的目的是省去了判断`null`的麻烦。两个引用类型都是`null`时它们也是相等的。

编写`hashCode()`方法：

```java
int hashCode() {
    return Objects.hash(firstName, lastName, age);
}
```

所以，编写`equals()`和`hashCode()`遵循的原则是：

`equals()`用到的用于比较的每一个字段，都必须在`hashCode()`中用于计算；`equals()`中没有使用到的字段，绝不可放在`hashCode()`中计算。

另外注意，对于放入`HashMap`的`value`对象，没有任何要求。

### 10.7 HashMap 和 HashSet区别

如果你看过 `HashSet` 源码的话就应该知道：HashSet 底层就是基于 HashMap 实现的。（HashSet 的源码非常非常少，因为除了 `clone() `、`writeObject()`、`readObject()`是 HashSet 自己不得不实现之外，其他方法都是直接调用 HashMap 中的方法。

| HashMap                          | HashSet                                                      |
| -------------------------------- | ------------------------------------------------------------ |
| 实现了Map接口                    | 实现Set接口                                                  |
| 存储键值对                       | 仅存储对象                                                   |
| 调用 `put（）`向map中添加元素    | 调用 `add（）`方法向Set中添加元素                            |
| HashMap使用键（Key）计算Hashcode | HashSet使用成员对象来计算hashcode值，对于两个对象来说hashcode可能相同，所以equals()方法用来判断对象的相等性， |

## 11 泛型

 泛型就是定义一种模板，例如`ArrayList`，然后在代码中为用到的类创建对应的`ArrayList<类型>`，

```java
ArrayList<String> strList = new ArrayList<String>();
```

 这样一来，既实现了编写一次，万能匹配，又通过编译器保证了类型安全：这就是泛型。 

 注意泛型的继承关系：可以把`ArrayList`向上转型为`List`（`T`不能变！)

### 11.1 泛型类

```java
public class Generic<T>{ 
    //key这个成员变量的类型为T,T的类型由外部指定  
    private T key;
    public Generic(T key) { //泛型构造方法形参key的类型也为T，T的类型由外部指定
        this.key = key;
    }
    public T getKey(){ //泛型方法getKey的返回值类型为T，T的类型由外部指定
        return key;
    }
}
```

### 11.2 泛型接口

除了`ArrayList`使用了泛型，还可以在接口中使用泛型。例如，`Arrays.sort(Object[])`可以对任意数组进行排序，但待排序的元素必须实现`Comparable`这个泛型接口。

```
public interface Comparable<T> {
    /**
     * 返回-1: 当前实例比参数o小
     * 返回0: 当前实例与参数o相等
     * 返回1: 当前实例比参数o大
     */
    int compareTo(T o);
}
```

泛型是一种类似”模板代码“的技术，不同语言的泛型实现方式不一定相同。

Java语言的泛型实现方式是擦拭法（Type Erasure）。

所谓擦拭法是指，虚拟机对泛型其实一无所知，所有的工作都是编译器做的。

 Java的泛型是由编译器在编译时实行的，编译器内部永远把所有类型`T`视为`Object`处理，但是，在需要转型的时候，编译器会根据`T`的类型自动为我们实行安全地强制转型。 

要实例化`T`类型，我们必须借助额外的`Class`参数：

```
public class Pair<T> {
    private T first;
    private T last;
    public Pair(Class<T> clazz) {
        first = clazz.newInstance();
        last = clazz.newInstance();
    }
}
```

上述代码借助`Class`参数并通过反射来实例化`T`类型，使用的时候，也必须传入`Class`。例如：

```
Pair<String> pair = new Pair<>(String.class);
```

因为传入了`Class`的实例，所以我们借助`String.class`就可以实例化`String`类型。

### 11.3 泛型方法

泛型方法是在调用方法的时候指明数据类型

语法：

> 修饰符 <T，E, ...> 返回值类型 方法名(形参列表) { 方法体... } 

- 泛型方法能使方法独立于类而产生变化
- 如果static方法要使用泛型能力，就必须使其成为泛型方法

### 11.4 泛型继承

 一个类可以继承自一个泛型类。  在继承了泛型类型的情况下，子类可以获取父类的泛型类型。

如果父类是泛型类，子类是泛型类，子类和父类的泛型要保持一致

```java
class Child<T> extends Parent<T>
```

子类不是泛型类，父类要明确泛型的数据类型

```java
class Child extends Parent<String>
```

### 11.5 类型通配符

类型通配符一般是使用“?”代替的具体的实参类型

```java
class Test{
    main{
        Box<Number> box = new Box<>();
        Box<Integer> box = new Box<>();
        //使用类型统配符，就可以传参了,如果不使用的话，使用show(Box<Number> box)
        show(box);
    }
    public static void show(Box<?> box){
        Object object = box.getValue()
    }
}
```

### 11.6 上下界通配符

使用`<? extends Number>`的泛型定义称之为上界通配符，即把泛型类型的上界限定在`Number`了，除了可以传入`Pair<Number>`类型，我们还可以传入`Pair<Double>`类型，`Pair<BigDecimal>`类型等等，因为`Double`和`BigDecimal`都是`Number`的子类。 

 使用`extends`通配符表示可以读，不能写。 

使用类似`<? super Integer>`通配符作为方法参数时表示：

- 方法内部可以调用传入`Integer`引用的方法，例如：`obj.setFirst(Integer n);`；
- 方法内部无法调用获取`Integer`引用的方法（`Object`除外），例如：`Integer n = obj.getFirst();`。

即使用`super`通配符表示只能写不能读。

为什么super叫下限？因为要从指定的参数往上看；extends为何叫上限，因为要从参数往下看。

### 11.7类型擦除

泛型是Java 1.5版本才引进的概念，在这之前是没有泛型的，但是泛型代码能够很好地和之前版本的代码兼容。那是因为，泛型信息只存在于代码编译阶段，在进入JVM之前，与泛型相关的信息会被擦除掉，我们称之为–类型擦除。 

### 11.8泛型与数组

可以声明带泛型的数组引用，但是不能直接创建带泛型的数组对象

## 12 反射

反射的原理

应用在一些通用性比较高的代码中，后面学习到的框架，大多数都是使用反射来实现的。在框架开发中，都是基于配置文件来开发的。在配置文件中配置了类，可以通过反射得到类中的所有的内容，可以让类中的某个方法执行。

将java文件保存到硬盘(.java文件），编译java文件，得到(.class)文件，使用JVM，把class文件通过类加载器加载到内存中。万事万物皆对象，class文件在内存中使用class类表示。当使用反射时，首先需要获取到class类，得到了这个类之后，就可以得到Class文件里面的所有的内容，包括属性、构造方法、普通方法。属性通过一个类File、构造方法通过Constructor、普通方法通过Method。 

使用反射首先需要得到Class类：类名.class、对象.getClass()、使用Class.forName("路径")

```java
//获取class类
Class c1 = Person.class;
Class c2 = new Person().getClass();
Class c3 = Class.forName("cn.text.text01.Person");
```

可以不通过new，得到实例 `Person p = (Person)c3.newInstance();` 

## 13 Java8新特性

### 13.1 接口变化

接口中除了定义全局常量和抽象方法外，还可以定义静态方法、默认方法

接口中定义的静态方法，只能通过接口来调用

如果子类（或实现类）继承的 父类和实现的接口中声明了同名同参数的方法，那么在子类没有重写的情况下，默认调用父类中的方法

```java
public interface TestInterface {
    static String method1(){
        return "Hello";
    }
    default String method2(){
        return "World";
    }
}
```

### 13.2 Lambda 表达式

```java
匿名内部类:
格式：new 类名/接口｛
    ｝
```

Lambda 允许把函数作为一个方法的参数（函数作为参数传递到方法中）。

```java
//所谓的函数式接口，就是接口中只有一个抽象方法
@FunctionalInterface
public interface LambdaInterface {
    int add(int a, int b);
}
```

我们可以这样使用

```
LambdaInterface lambdaInterface = (a,b) -> { return a + b;};
```

可以将lambda表达式当作任意只包含一个抽象方法的接口类型，确保你的接口一定达到这个要求，你只需要给你的接口添加 `@FunctionalInterface `注解，编译器如果发现你标注了这个注解的接口有多于一个抽象方法的时候会报错的。

 ```java
//转换器
@FunctionalInterface
interface Converter<T,V>{
	T convert(V from);
}
Converter<Integer,String> converter = from -> Integer.parseInt(from);
Integer c = converter.convert("123");
System.out.println(c);
 ```

### 13.3 函数式接口

Java内置的函数式接口

Predicate<T> 接口、Consumer<T>接口、Supplier<T>接口、Function<T,R>接口

### 13.4 方法引用

若Lambda体内的内容已经有方法实现了，我们可以使用方法引用。

使用要求：要求接口中的抽象方法的形参列表和返回值类型与方法引用的方法的形参列表和返回值类型相同。

```java
/**
Consumer中的void accept(T t)
PrintStream中的void println(T t)
*/
Consumer<String> consumer = (x) ->{
	System.out.println(x);
};
consumer.accept("Hello");

PrintStream printStream = System.out;
Consumer<String> con = printStream::println;
con.accept("Hello");
```

方法引用通过方法的名字来指向一个方法

方法引用可以使语言的构造更紧凑简洁，减少冗余代码

方法引用使用一对冒号 **::** 

三种语法格式：

对象::实例方法名

类::静态方法名

Java 8 允许你使用 :: 关键字来传递方法或者构造函数引用 , 上面的代码还可以通过静态方法引用来表示：

```java
Converter<Integer,String> converter = Integer::parseInt;
```

类::实例方法名

```java
/**
Compartor: int compare(T t1, T t2)
String: int t1.compareTo(t2)
*/
Comparator<String> com1 = (s1, s2)-> {
	return s1.compareTo(s2);
};
System.out.println(com1.compare("abc", "abd"));

Comparator<String> com2 = String::compareTo;
System.out.println(com2.compare("abd", "abv"));
```

接下来看看构造函数是如何使用::关键字来引用的

和方法引用类似，函数式接口的抽象方法的形参列表和构造器的形参列表一致，抽象方法的返回类型即为构造器所属的类的类型

```java
// T get()
// Person中的空参构造器Person()
Supplier<Person> p1 = () -> new Person();
Supplier<Person> p2 = Person :: new;
```

**lambda 表达式的局部变量可以不用声明为 final，但是必须不可被后面的代码修改（即隐性的具有 final 的语义）** 

### 13.5 Stream流

Stream（流）是一个来自数据源的元素队列并支持聚合操作

- 元素是特定类型的对象，形成一个队列。 Java中的Stream并不会存储元素，而是按需计算。
- 数据源 流的来源。 可以是集合，数组，I/O channel， 产生器generator 等。
- 聚合操作类似SQL语句一样的操作， 比如filter, map, reduce, find, match, sorted等。

和以前的Collection操作不同， Stream操作还有两个基础的特征：

- **Pipelining**: 中间操作都会返回流对象本身。 这样多个操作可以串联成一个管道， 如同流式风格（fluent style）。 这样做可以对操作进行优化， 比如延迟执行(laziness)和短路( short-circuiting)。
- **内部迭代**： 以前对集合遍历都是通过Iterator或者For-Each的方式, 显式的在集合外部进行迭代， 这叫做外部迭代。 Stream提供了内部迭代的方式， 通过访问者模式(Visitor)实现。

>  创建流：可以通过集合、数组、Stream.of()

> 中间操作：filter根据条件过滤、limit(n) 截断流：使其元素不超过给定数量、skip(n)： 扔掉了前n个元素 、 distinct 去调重复的数据、sorted排序
>
> 终止操作：allMatch检查匹配所有元素、reduce可以将流中的元素反复结合起来，得到一个值
>
> collect: 收集

map接受lambda 将元素转换为其他形式或提取信息,接受一个函数作为参数，该函数会被应用到每个元素上，并将其映射成一个新元素。

```java
// 获取名字长度大于2的人的姓名
list.stream().
    map(p -> p.getName()).
    filter(name -> name.length() > 2).
    forEach(System.out::println);
//获取年龄大于20的人
list.stream().
    filter(p -> p.getAge() > 20).
    collect(Collectors.toList()).
    forEach(System.out::println);
```

### 13.6 Optional类

> Optional类是Java8为了解决null值判断问题，借鉴google guava类库的Optional类而引入的一个同名Optional类，使用Optional类可以避免显式的null值判断（null的防御性检查），避免null导致的NPE（NullPointerException）。总而言之，就是对控制的一个判断，为了避免空指针异常。

常用方法：

| 方法        | 描述                                                         |
| :---------- | :----------------------------------------------------------- |
| empty       | 放回一个值为空的Optional实例                                 |
| filter      | 如果值存在并且满足提供的谓词，就返回包含该Optional对象；否则返回一个空的Optional对象 |
| flatMap     | 如果值存在，就对该值执行提供的mapping函数，将mapping函数返回值用Optional封装并返回，否则就返回一个空的Optional对象 |
| get         | 如果值存在就返回该Optional对象，否则就抛出一个 NoSuchElementException异常 |
| ifPresent   | 如果值存在就对该值执行传入的方法，否则就什么也不做           |
| isPresent   | 如果值存在就返回true，否则就返回false                        |
| map         | 如果值存在，就对该值执行提供的mapping函数调用，将mapping函数返回值用Optional封装并返回 |
| of          | 如果传入的值存在，就返回包含该值的Optional对象，否则就抛出NullPointerException异常 |
| ofNullable  | 如果传入的值存在，就返回包含该值的Optional对象，否则返回一个空的Optional对象 |
| orElse      | 如果值存在就将其值返回，否则返回传入的默认值                 |
| orElseGet   | 如果值存在就将其值返回，否则返回一个由指定的Supplier接口生成的值 |
| orElseThrow | 如果值存在就将其值返回，否则返回一个由指定的Supplier接口生成的异常 |

示例：

```java
/**
原来代码的编写
*/
if (user != null) {
    Address address = user.getAddress();
    if (address != null) {
        Country country = address.getCountry();
        if (country != null) {
            String isocode = country.getIsocode();
            if (isocode != null) {
                isocode = isocode.toUpperCase();
            }
        }
    }
}
String result = Optional.ofNullable(user)
      .flatMap(u -> u.getAddress())
      .flatMap(a -> a.getCountry())
      .map(c -> c.getIsocode())
      .orElse("default");
```

### 13.7 日期API

* **Clock时钟**

Clock类提供了访问当前日期和时间的方法，Clock是时区敏感的，可以用来取代 System.currentTimeMillis() 来获取当前的微秒数。某一个特定的时间点也可以使用Instant类来表示，Instant类也可以用来创建老的java.util.Date对象。 

```java
Clock clock = Clock.systemDefaultZone();
long millis = clock.millis();
System.out.println(millis);
```

*  **Timezones 时区** 

在新API中时区使用ZoneId来表示。时区可以很方便的使用静态方法of来获取到。 时区定义了到UTS时间的时间差，在Instant时间点对象到本地日期对象之间转换的时候是极其重要的。 

```java
ZoneId currentZone = ZoneId.systemDefault();
System.out.println("当期时区: " + currentZone);
```

* **LocalDateTime**

```java
LocalDateTime currentTime = LocalDateTime.now();
System.out.println("当前时间: " + currentTime);
```

## 14 内部类

### 14.1 匿名内部类

匿名内部类也就是没有名字的内部类

正因为没有名字，所以匿名内部类只能使用一次，它通常用来简化代码编写

但使用匿名内部类还有个前提条件：必须继承一个父类或实现一个接口

我们编写一个类去继承或实现一个接口，有时候只需要使用一次，就可以使用匿名内部类

```java
public class test{
    public static void main(String[] args) {
        Thread t = new Thread(new Runnable() {
            @Override
            public void run() {
                
            }
        });
    }
}
```

### 14.2 成员内部类

基本格式如下，成员内部类可以无条件访问外部类的属性和方法，但是外部类想要访问内部类属性或方法时，必须要创建一个内部类对象，然后通过该对象访问内部类的属性或方法

```java
class C{
    class D{

    }
}
```

### 14.3 局部内部类

 局部内部类存在于方法中。
他和成员内部类的区别在于局部内部类的访问权限仅限于方法或作用域内。 

```java
class K{
    public void say(){
        class J{
            
        }
    }
}
```

### 14.4 静态内部类

静态内部类和成员内部类相比多了一个static修饰符。它与类的静态成员变量一般，是不依赖于外部类的。同时静态内部类也有它的特殊性。因为外部类加载时只会加载静态域，所以静态内部类不能使用外部类的非静态变量与方法。同时可以知道成员内部类里面是不能含静态属性或方法的。

## 15 枚举类

**什么情况下使用枚举类？** 

类的对象只有有限个，确定的。如订单的状态、性别、星期……

当定义一组常量时，强烈建议使用枚举。

在没有枚举类型时定义对象常见的方式。

```java
public final static Weekday SUN = new Weekday("1", "星期一");
public final static Weekday MON = new Weekday("2", "星期二");
```

定义枚举类示例：

```java
public enum Weekday {
    // 提供当前枚举类的对象，多个对象直接用逗号分隔，继承Enum
    SUN("1", "星期一"),
    MON("2", "星期二");
}
```

**使用枚举类的优点：**

枚举类更加直观，类型安全。 

**枚举类的常用方法**

```java
/**
values():返回枚举对象的对象数组
valueOf(String obj):根据Name查找对象，没有出现IllegalArgumentException异常
toString():返回当前枚举类对象常量的名称
*/
Weekday[] values = Weekday.values();
Weekday ok = Weekday.valueOf("SUN");
```

**枚举类实现接口的情况**：

> 情况一：实现接口，在enum类中实现抽象方法
>
> 情况二：让枚举类中的对象分别实现接口中的抽象方法

代码示例

```java
 interface InterfaceTest{
     String getValue();
 }
public enum Weekday {
    SUN("1", "星期一"){
        @Override
        public String getValue() {
            return null;
        }
    },
    MON("2", "星期二"){
        @Override
        public String getValue() {
            return null;
        }
    };
}
```

## 16 IO流



## 17 网络编程

































