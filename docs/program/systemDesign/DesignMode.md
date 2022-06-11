# 设计模式的六大原则

**1、开闭原则（Open Close Principle）**

开闭原则的意思是：**对扩展开放，对修改关闭**。在程序需要进行拓展的时候，不能去修改原有的代码，实现一个热插拔的效果。简言之，是为了使程序的扩展性好，易于维护和升级。想要达到这样的效果，我们需要使用接口和抽象类，后面的具体设计中我们会提到这点。

**2、里氏代换原则（Liskov Substitution Principle）**

里氏代换原则是面向对象设计的基本原则之一。 里氏代换原则中说，任何基类可以出现的地方，子类一定可以出现。LSP 是继承复用的基石，只有当派生类可以替换掉基类，且软件单位的功能不受到影响时，基类才能真正被复用，而派生类也能够在基类的基础上增加新的行为。里氏代换原则是对开闭原则的补充。实现开闭原则的关键步骤就是抽象化，而基类与子类的继承关系就是抽象化的具体实现，所以里氏代换原则是对实现抽象化的具体步骤的规范。

**3、依赖倒转原则（Dependence Inversion Principle）**

这个原则是开闭原则的基础，具体内容：针对接口编程，依赖于抽象而不依赖于具体。

**4、接口隔离原则（Interface Segregation Principle）**

这个原则的意思是：使用多个隔离的接口，比使用单个接口要好。它还有另外一个意思是：降低类之间的耦合度。由此可见，其实设计模式就是从大型软件架构出发、便于升级和维护的软件设计思想，它强调降低依赖，降低耦合。

**5、迪米特法则，又称最少知道原则（Demeter Principle）**

最少知道原则是指：一个实体应当尽量少地与其他实体之间发生相互作用，使得系统功能模块相对独立。

**6、合成复用原则（Composite Reuse Principle）**

合成复用原则是指：尽量使用合成/聚合的方式，而不是使用继承。

**7.单一职责原则**

 一个类只负责一个功能领域中的相应职责，或者可以定义为：就一个类而言，应该只有一个引起它变化的原因。 

## 创建型模式

### 工厂模式

定义一个创建对象的接口，让其子类自己决定实例化哪一个工厂类，工厂模式使其创建过程延迟到子类进行。

```java
/**
 * Description: 简单工厂模式
 *
 * 优点：实现对象的创建和使用的分离
 * 缺点：工厂类集中了所有产品创建逻辑
 */
public class SimpleFactory {
    public static Product produceProduct(String type){
        Product product = null;
        if(type.equals("A"))
            product =  new ProductA();
        if(type.equals("B"))
            product =  new ProductB();
        return product;
    }

    public static void main(String[] args) {
        Product product = SimpleFactory.produceProduct("A");
        product.produce();
    }

}

abstract class Product{
    public abstract void produce();
}
class ProductA extends Product{

    @Override
    public void produce() {
        System.out.println("产品A");
    }
}
class ProductB extends Product{
    @Override
    public void produce() {
        System.out.println("产品B");
    }
}
```

### 抽象工厂模式

```java
/**
 * 工厂方法模式：每一个具体工厂都负责生产一种具体的产品
 * 抽象工厂：在一个产品族里面，定义多个产品。
 * */
public class FactoryMethod {

    public static void main(String[] args) {
        AbstractFactory bydFactory = new BYDFactory();
        AbstractFactory tslFactory = new TSLFactory();
        bydFactory.produce().generate();
        tslFactory.produce().generate();
        bydFactory.mask().produceMask();
    }
}

interface AbstractFactory{
    Car produce();
    // 可以新增某一大类
    Mask mask();
}


interface Mask{
    void produceMask();
}
interface Car{
    void generate();
}

class BYDFactory implements AbstractFactory{

    @Override
    public Car produce() {
        return new BYD();
    }

    @Override
    public Mask mask() {
        return new MaskWUHAN();
    }
}

class TSLFactory implements AbstractFactory{

    @Override
    public Car produce() {
        return new TSL();
    }

    @Override
    public Mask mask() {
        return new MaskWUHAN();
    }
}

class TSL implements Car{

    @Override
    public void generate() {
        System.out.println("特斯拉");
    }
}

class BYD implements Car{

    @Override
    public void generate() {
        System.out.println("比亚迪");
    }
}

class MaskWUHAN implements Mask{

    @Override
    public void produceMask() {
        System.out.println("还可以生产口罩");
    }
}
```

### 单例模式

单例模式确保某一个类只有一个实例，并提供一个访问它的全局访问点。

代码举例： 

```java
/*
饿汉式:
类加载到内存中，就实例化一个单例，JVM保证线程安全
唯一缺点：不管用到与否，类装载时就完成实例化
*/
public class Singleton {  
    private static Singleton instance = new Singleton();  
    private Singleton (){}  
    public static Singleton getInstance() {  
    return instance;  
    }  
}

/*
懒汉式:
虽然达到了按需加载的目的，但却带来线程不安全的问题
*/
public class Singleton {  
    private static Singleton instance;  
    private Singleton (){}  
  
    public static Singleton getInstance() {  
    if (instance == null) {  
        instance = new Singleton();  
    }  
    return instance;  
    }  
}

/*
双检锁/双重校验锁
*/
public class Singleton {  
    private volatile static Singleton singleton;  
    private Singleton (){}  
    public static Singleton getSingleton() {  
    if (singleton == null) {  
        synchronized (Singleton.class) {  
        if (singleton == null) {  
            singleton = new Singleton();  
        }  
        }  
    }  
    return singleton;  
    }  
}
/*
静态内部类
*/
public class Singleton {  
    private static class SingletonHolder {  
    private static final Singleton INSTANCE = new Singleton();  
    }  
    private Singleton (){}  
    public static final Singleton getInstance() {  
    return SingletonHolder.INSTANCE;  
    }  
}

/*
枚举
*/
public enum Singleton {  
    INSTANCE;  
    public void whateverMethod() {  
    }  
} 
```

### 建造者模式

### 原型模式

Java中的Object类提供了浅克隆的clone()方法，具体原型类只要实现Cloneable接口就可实现对象的浅克隆。

```javascript
public class Location01 implements Cloneable{
    String street;
    int roomNo;
    public Location01(String street, int roomNo){
        this.street = street;
        this.roomNo = roomNo;
    }

    public String toString(){
        return "Location["+"street:"+street+",roomNo:"+roomNo+"]";
    }

    public Object clone() throws CloneNotSupportedException {
        return super.clone();
    }

    public static void main(String[] args) throws CloneNotSupportedException {
        Location01 location01 = new Location01("黄河大街", 1001);
        Location01 clone = (Location01) location01.clone();
        System.out.println(location01);
        System.out.println(clone);
        System.out.println(location01 == clone);
    }
}
```

## 结构型模式 

### 适配器模式

```java
public class Main {
    public static void main(String[] args) throws Exception {
        FileInputStream fileInputStream = new FileInputStream("D:\\file01.txt");
        InputStreamReader reader = new InputStreamReader(fileInputStream);
        BufferedReader br = new BufferedReader(reader);
        String s = br.readLine();
        while (s != null) {
            System.out.println(s);
        }
        br.close();
    }
}
```

### 桥接模式

### 过滤器模式

### 组合模式

### 装饰器模式

### 外观模式

### 享元模式

### 代理模式

```java
public interface Movable {
    void run();
}
public class Task implements Movable {
    public void run() {
        System.out.println("TaskRun...");
        try {
            Thread.sleep(new Random().nextInt(10000));
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

public class StatiProxy02 implements Movable{
    Movable movable;
    public StatiProxy02(Movable movable){
        this.movable = movable;
    }
    public void run() {
        System.out.println("start...");
        movable.run();
        System.out.println("end...");
    }
}

//静态代理
public class StatiProxy01 implements Movable{
    Movable movable;
    public StatiProxy01(Movable movable){
        this.movable = movable;
    }
    public void run() {
        long start = System.currentTimeMillis();
        movable.run();
        long end = System.currentTimeMillis();
        System.out.println(end - start);
    }
    public static void main(String[] args) {
        new StatiProxy01(new StatiProxy02(new Task())).run();
    }
}
```

## 行为型模式

### 观察者模式

定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。主要解决：一个对象状态改变给其他对象通知的问题，而且要考虑到易用和低耦合，保证高度的协作。



### 责任链模式

责任链模式(Chain of Responsibility)使多个对象都有机会处理请求，从而避免请求的发送者和接受者之间的耦合关系。将这些对象连成一条链，并沿着这条链传递该请求，直到有对象能够处理它。

```java
public class Main {
    public static void main(String[] args) {
        LeaveChain leaveChain = new LeaveChain();
        Person person = new Person("张三",16,"生病了");
        leaveChain.addLeave(new GroupLeader())
                .addLeave(new ManagerLeader())
                .addLeave(new BigManager());

        leaveChain.doFilter(person);
    }
}

class Person{
    String name;
    Integer day;
    String content;

    public String getName() {
        return name;
    }

    public Integer getDay() {
        return day;
    }

    public String getContent() {
        return content;
    }

    public Person(String name, Integer day, String content) {
        this.name = name;
        this.day = day;
        this.content = content;
    }
}

interface Leave {
    boolean doFilter(Person person);
}

class ManagerLeader implements Leave{

    @Override
    public boolean doFilter(Person person) {
        if(person.getDay() <= 7 && person.getDay() > 3){
            System.out.println("经理批准");
            return true;
        }
        return false;
    }
}

class GroupLeader implements  Leave{

    @Override
    public boolean doFilter(Person person) {
        if(person.getDay() <= 3){
            System.out.println("组长批准");
            return true;
        }
        return false;
    }
}

class BigManager implements Leave{

    @Override
    public boolean doFilter(Person person) {
        if(person.getDay() >= 15 && person.getDay() <= 60){
            System.out.println("总经理批准");
            return true;
        }else{
            System.out.println("无法处理您的请假要求！");
        }
        return false;
    }
}

class LeaveChain implements Leave{
    List<Leave> list = new ArrayList<>();

    public LeaveChain addLeave(Leave leave){
        list.add(leave);
        return this;
    }

    @Override
    public boolean doFilter(Person person) {
        for (Leave leave : list) {
            if(leave.doFilter(person)){
                return false;
            }
        }
        return true;
    }
}
```

### 命令模式

### 解释器模式

### 迭代器模式

### 中介者模式

### 备忘录模式

### 状态模式

### 空对象模式

### 策略模式

### 模板模式

### 访问者模式











































