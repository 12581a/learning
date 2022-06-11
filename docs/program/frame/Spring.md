# Spring概述

## spring的IOC

### springIOC原理

```java
<bean id="helloWorld" class="com.ioc.HelloWorld">
	<property name="message" value="Hello World!"/>
</bean>
```

首先加载并解析上面的配置文件，获取标签中的id和class属性，根据class属性利用反射来创建类对象和设置类属性，最后将Bean注入到Bean容器中。

**什么是POJO，JavaBean？**

POJO：
 一个简单的Java类，这个类没有实现/继承任何特殊的java接口或者类，不遵循任何主要java模型，约定或者框架的java对象，在理想情况下，POJO不应该有注解。
JavaBean：

- JavaBean是可序列化的，实现了serializable接口
- 具有一个无参构造器
- 有按照命名规范的set和gett，is（可以用于访问布尔类型的属性）方法

**ApplicationContext容器**

```java
public interface ApplicationContext extends EnvironmentCapable, ListableBeanFactory, 	HierarchicalBeanFactory,MessageSource, ApplicationEventPublisher,
ResourcePatternResolver {
```

ApplicationContext是一个接口，最常被使用的 ApplicationContext 接口实现： 

* **FileSystemXmlApplicationContext**：该容器从 XML 文件中加载已被定义的 bean。在这里，你需要提供给构造器 XML 文件的完整路径

- **ClassPathXmlApplicationContext**：该容器从 XML 文件中加载已被定义的 bean。在这里，你不需要提供 XML 文件的完整路径，只需正确配置 CLASSPATH 环境变量即可，因为，容器会从 CLASSPATH 中搜索 bean 配置文件。
- **WebXmlApplicationContext**：该容器会在一个 web 应用程序的范围内加载在 XML 文件中已被定义的 bean。

### springBean的生命周期

- Bean容器找到配置文件中 Spring Bean 的定义。
- Bean容器利用Java Reflection API创建一个Bean的实例。
- 如果涉及到一些属性值,利用set方法设置一些属性值。
- 如果Bean实现了BeanNameAware接口，调用setBeanName()方法，传入Bean的名字。
- 如果Bean实现了BeanClassLoaderAware接口，调用setBeanClassLoader()方法，传入ClassLoader对象的实例。
- 如果Bean实现了BeanFactoryAware接口，调用setBeanClassLoader()方法，传入ClassLoader对象的实例。
- 与上面的类似，如果实现了其他*Aware接口，就调用相应的方法。
- 如果有和加载这个Bean的Spring容器相关的BeanPostProcessor对象，执行postProcessBeforeInitialization()方法
- 如果Bean实现了InitializingBean接口，执行afterPropertiesSet()方法。
- 如果Bean在配置文件中的定义包含init-method属性，执行指定的方法。
- 如果有和加载这个Bean的Spring容器相关的BeanPostProcessor对象，执行postProcessAfterInitialization()方法
- 当要销毁Bean的时候，如果Bean实现了DisposableBean接口，执行destroy()方法。
- 当要销毁Bean的时候，如果Bean在配置文件中的定义包含destroy-method属性，执行指定的方法。

**基于注解的配置**

* **@Required** 注释应用于 bean 属性的 setter 方法，它表明受影响的 bean 属性在配置时必须放在 XML 配置文件中，否则容器就会抛出一个 BeanInitializationException 异常。 
* **@Autowired** 注释可以在 setter 方法中被用于自动连接 bean，在属性上面使用来除去setter方法
*  **@Resource** ,如 @Resource(name="userDao") 在 private UserDao userdao上面
* **@Controller**对应表现层的Bean
* **@Service**对应的是业务层Bean ，如@Service("user")相当于`<id="user" class="...">`
* **@Scope**,如在类上加@Scope(value="prototype")注解表示多实例。 
* **@Repository**对应数据访问层Bean  

### bean的作用域

在 Spring 中，那些组成应用程序的主体及由 Spring IOC 容器所管理的对象，被称之为 bean。简单地讲，bean 就是由 IOC 容器初始化、装配及管理的对象，除此之外，bean 就与应用程序中的其他对象没有什么区别了。而 bean 的定义以及 bean 相互间的依赖关系将通过配置元数据来描述。 

| 类别          | 说明                                                         |
| ------------- | ------------------------------------------------------------ |
| singleton     | 单例，是默认值。可以指定Bean节点的 `lazy-init=”true”` 来延迟初始化bean，这时候，只有在第一次获取bean时才会初始化bean，即第一次请求该bean时才初始化。 |
| prototype     | 定义对应多个对象实例。 prototype 作用域的 bean 会导致在每次对该 bean 请求时都会创建一个新的 bean 实例。 |
| request       | 每一次HTTP请求都会产生一个新的 bean，bean仅在当前HTTP request内有效。request只适用于Web程序，每一次 HTTP 请求都会产生一个新的bean，同时该bean仅在当前HTTP request内有效，当请求结束后，该对象的生命周期即告结束。 |
| session       | 每一次HTTP请求都会产生一个新的 bean，该bean仅在当前 HTTP session 内有效。session只适用于Web程序，session 作用域表示该针对每一次 HTTP 请求都会产生一个新的 bean，同时该 bean 仅在当前 HTTP session 内有效。 |
| globalSession | 在一个全局的HTTP Session中，一个bean定义对应一个实例。典型情况下，仅在使用portlet context的时候有效。该作用域仅在基于 web的Spring ApplicationContext情形下有效。 |

## spring的AOP

### 原理概述

动态代理的两类实现：Jdk动态代理与基于继承的代理，两类代理的实现：Jdk代理与Cglib代理。Jdk动态代理只能 基于接口进行动态代理。Cdlib基于继承实现来实现代理，无法对static、final类进行代理,无法对private、static方法进行代理。

静态代理：代理类和被代理类都会实现同一个接口，代理类里面把被代理类的对象作为一个属性，然后就可以用在被代理类的前后加方法。改进：代理类里面把接口对象作为一个属性。

动态代理：jdk动态代理的代理类自动生成

![](D:\TyporaFile\图片\Proxy.PNG)

动态代理案例：

首先定义一个接口

```java
public interface Book { 
    void add();
}
```

接口实现

```java
public class BookImpl implements Book{
    public void add() {
        System.out.println("add......");
    }
}
```

JDK动态代理

```java
public class JdkProxy implements InvocationHandler {
    private BookImpl bookImpl;

    public JdkProxy(BookImpl bookImpl) {
        this.bookImpl = bookImpl;
    }

    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("proxy......");
        Object book = null;
        book = (BookImpl) method.invoke(bookImpl,args);
        return book;
    }
}
public class TestAop {
    public static void main(String[] args) {
        Book book = (Book) Proxy.newProxyInstance(TestAop.class.getClassLoader(),
                new Class[]{Book.class},new JdkProxy(new BookImpl()));
        book.add();
    }
//proxy......
//add......   
```

使用Cglib实现动态代理

```java
public class DemoMethodInterceptor implements MethodInterceptor {
    public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
        System.out.println("beforeCglib......");
        Object book = null;
        book = methodProxy.invokeSuper(o,objects);
        System.out.println("beforeCglib......");
        return book;
    }
}
public class TestAop {
    public static void main(String[] args) {
		Enhancer enhancer = new Enhancer();
        enhancer.setSuperclass(BookImpl.class);
        enhancer.setCallback(new DemoMethodInterceptor());
        Book book = (Book) enhancer.create();
        book.add();
    }
//beforeCglib......
//add......
//beforeCglib......
```

### 表达式

**1.within表达式**

```java
@Aspect
@Component
public class YourBook {
    @Pointcut("within(com.spring.aop.Book)")
    public void matchType(){}

    @Before("matchType()")
    public void before(){
        System.out.println("yourbefore......");
    }
}
```

**2.对象表达式**

```java
@Aspect
@Component
public class YourBook {
    //Book是一个接口，会拦截所有实现了这个接口的类
    @Pointcut("this(com.spring.aop.Book)")
    public void matchType(){}
	......
}
//@Pointcut("target(com.spring.aop.Book)")
//@Pointcut("Bean(BookImpl)")
```

**3.execution表达式，execution(<访问修饰符><返回类型><方法名(<参数>)<异常>)**

```java
@Aspect
@Component
public class YourBook {
    @Pointcut("execution(* cn.itcast.aspectj.Book.*(..))")
    public void matchType(){}
	......
}
//@Before(value="execution(* cn.itcast.aspectj.Book.*(..))")
```

## 动手实现IOC与AOP

[博文链接](https://www.tianxiaobo.com/2018/01/18/%E8%87%AA%E5%B7%B1%E5%8A%A8%E6%89%8B%E5%AE%9E%E7%8E%B0%E7%9A%84-Spring-IOC-%E5%92%8C-AOP-%E4%B8%8B%E7%AF%87/)

[github代码](https://github.com/greyireland/tiny-spring)

思路：

1. 加载xml配置文件，遍历其中的标签
2. 获取标签中的id和class属性，加载class属性对应的类，并创建bean
3. 遍历标签中的标签，获取属性值，并将属性值填充到bean中
4. 将bean注册到bean容器中

```java
public class SimpleIOC {

    private Map<String, Object> beanMap = new HashMap<>();

    public SimpleIOC(String location) throws Exception {
        loadBeans(location);
    }

    public Object getBean(String name) {
        Object bean = beanMap.get(name);
        if (bean == null) {
            throw new IllegalArgumentException("there is no bean with name " + name);
        }

        return bean;
    }

    private void loadBeans(String location) throws Exception {
        // 加载 xml 配置文件
        InputStream inputStream = new FileInputStream(location);
        DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
        DocumentBuilder docBuilder = factory.newDocumentBuilder();
        Document doc = docBuilder.parse(inputStream);
        Element root = doc.getDocumentElement();
        NodeList nodes = root.getChildNodes();

        // 遍历 <bean> 标签
        for (int i = 0; i < nodes.getLength(); i++) {
            Node node = nodes.item(i);
            if (node instanceof Element) {
                Element ele = (Element) node;
                String id = ele.getAttribute("id");
                String className = ele.getAttribute("class");
                
                // 加载 beanClass
                Class beanClass = null;
                try {
                    beanClass = Class.forName(className);
                } catch (ClassNotFoundException e) {
                    e.printStackTrace();
                    return;
                }

                   // 创建 bean
                Object bean = beanClass.newInstance();

                // 遍历 <property> 标签
                NodeList propertyNodes = ele.getElementsByTagName("property");
                for (int j = 0; j < propertyNodes.getLength(); j++) {
                    Node propertyNode = propertyNodes.item(j);
                    if (propertyNode instanceof Element) {
                        Element propertyElement = (Element) propertyNode;
                        String name = propertyElement.getAttribute("name");
                        String value = propertyElement.getAttribute("value");

                            // 利用反射将 bean 相关字段访问权限设为可访问
                        Field declaredField = bean.getClass().getDeclaredField(name);
                        declaredField.setAccessible(true);

                        if (value != null && value.length() > 0) {
                            // 将属性值填充到相关字段中
                            declaredField.set(bean, value);
                        } else {
                            String ref = propertyElement.getAttribute("ref");
                            if (ref == null || ref.length() == 0) {
                                throw new IllegalArgumentException("ref config error");
                            }
                            
                            // 将引用填充到相关字段中
                            declaredField.set(bean, getBean(ref));
                        }

                        // 将 bean 注册到 bean 容器中
                        registerBean(id, bean);
                    }
                }
            }
        }
    }

    private void registerBean(String id, Object bean) {
        beanMap.put(id, bean);
    }
}
```

容器测试使用的bean代码： 

```java
public class Car {
    private String name;
    private String length;
    private String width;
    private String height;
    private Wheel wheel;
    
    // 省略其他不重要代码
}

public class Wheel {
    private String brand;
    private String specification ;
    
    // 省略其他不重要代码
}
```

bean配置文件ioc.xml内容: 

```java
<beans>
    <bean id="wheel" class="com.titizz.simulation.toyspring.Wheel">
        <property name="brand" value="Michelin" />
        <property name="specification" value="265/60 R18" />
    </bean>

    <bean id="car" class="com.titizz.simulation.toyspring.Car">
        <property name="name" value="Mercedes Benz G 500"/>
        <property name="length" value="4717mm"/>
        <property name="width" value="1855mm"/>
        <property name="height" value="1949mm"/>
        <property name="wheel" ref="wheel"/>
    </bean>
</beans>
```

IOC测试类SimpleIOCTest：

```java
public class SimpleIOCTest {
    @Test
    public void getBean() throws Exception {
        String location = SimpleIOC.class.getClassLoader().getResource("spring-test.xml").getFile();
        SimpleIOC bf = new SimpleIOC(location);
        Wheel wheel = (Wheel) bf.getBean("wheel");
        System.out.println(wheel);
        Car car = (Car) bf.getBean("car");
        System.out.println(car);
    }
} 
```

## springMVC

### 架构原理分析

* 发起请求到前端控制器（DispatcherServlet）

* 前端控制器请求HandlerMapping查找Handler，可以根据xml配置、注解方式查找

* 处理器映射器HandlerMapping向前端控制器返回Handler

* 前端控制器调用处理器适配器去执行Handler,Handler执行完成给适配器返回ModeAndView

* 处理器适配器向前端控制器返回ModeAndView。ModeAndView是springMVC框架对象的一个底层的对象，包括Model和view。

* 前端控制器请求视图解析器去进行视图的解析，根据逻辑视图解析成真正的视图	（jsp）

* 视图解析器向前端控制器返回view,前端控制器进行视图的渲染。视图渲染将模型数据（在ModelAndView对象中）填充到request域中。

### 配置文件

```properties
<!--在web.xml中配置前端控制器-->
<servlet>  
        <servlet-name>springmvctest</servlet-name>  
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>  
       	<init-param>  
	        <param-name>contextConfigLocation</param-name>  
	        <param-value>classpath:springmvc.xml</param-value>  
   		 </init-param>
        <load-on-startup>1</load-on-startup>  
</servlet>  
    <servlet-mapping>  
        <servlet-name>springmvctest</servlet-name>  
        <url-pattern>/</url-pattern>  
	</servlet-mapping>  
```

```properties
<!--视图解析器-->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
			 <property name="prefix" value="/WEB-INF/jsp/"/>
    		 <property name="suffix" value=".jsp"/>
</bean>
<mvc:annotation-driven></mvc:annotation-driven>
```

### SpringMVC常见注解

* **@RequestMapping**

是一个用来处理请求地址映射的注解,适用于类、方法。用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径。

* **@PathVariable**

用于将请求URL中的模板变量映射到功能处理方法的参数上，即取出uri模板中的变量作为参数。 

```java
@RequestMapping("/delete/{id}")
public String deleteTest(@PathVariable(value="id") Integer id){}
```

* **@RequestParam**

用于将请求参数区数据映射到功能处理方法的参数上 

 value/name: 两个属性都指代参数名字，即入参的请求参数名字(通常表单name属性)
 required: 是否必须，默认是true，表示请求中一定要有相应的参数，否则将抛出异常
 defaultValue: 默认值，表示如果请求中没有同名参数时的默认值，设置该参数时，自动将required设为false

```java
@RequestMapping("/test")
public String deleteTest(@RequestParam(value="username") String user,@RequestParam(value="password",required=false,defaultValue="0") Integer passWord){}
```

* **@CookieValue**

 可以把Request header中关于cookie的值绑定到方法的参数上 

```java
public String index(@CookieValue("JSESSIONID") String cookie){}
```

* **@SessionAttributes**

只能作用在类上，作用是将指定的Model中的键值对添加至session中，方便在下一次请求中使用。 

```java
@Controller
@RequestMapping("/testSessionAttribute")
@SessionAttributes(value = {"user"},types={String.class})
public class TestSessionAttributeController {
    @RequestMapping("/testHandler")
    public String testHandler(Model model, String age, String name){
       User user = new User(age,name);
       model.addAttribute("user",user); 
       model.addAttribute("age",age);
       return "result";
	}
}
```

**@modelAttribute**

应用在方法上: 被`@ModelAttribute`注解的方法会在`Controller`每个方法执行之前都执行，因此对于一个`Controller`中包含多个URL的时候，要谨慎使用。 

## SpringBoot

### Springboot的优点

Spring Boot遵循“固执己见的默认配置”，以减少开发工作（默认配置可以修改）。 Spring Boot 项目所需的开发或工程时间明显减少，通常会提高整体生产力。 

### SpringBoot配置

通过`@Value`来读取配置信息，还可以通过`@ConfigurationProperties`读取并与 bean 绑定。它们之间的比较：

|                      | @ConfigurationProperties | @value     |
| -------------------- | ------------------------ | ---------- |
| 功能                 | 批量注入配置文件中的属性 | 一个个指定 |
| 松散绑定（松散语法） | 支持                     | 不支持     |
| SpEL                 | 不支持                   | 支持       |
| JSR303数据校验       | 支持                     | 不支持     |
| 复杂类型封装         | 支持                     | 不支持     |

使用原则：在某个业务逻辑中需要获取一下配置文件中的某项值，使用@Value；如果专门编写了一个javaBean来和配置文件进行映射，我们就直接使用@ConfigurationProperties； 

```java
/*
* 配置文件中的属性与类对应的属性绑定,默认从全局配置文件中获取值
*/
@Component
@ConfigurationProperties(prefix = "person")
public class Person {
    //@Value("${person.username}")
    private String username;
    private Integer password;
    
//yml配置文件
person:
	username: zhangsan
	password: 123456
```

```xml
//pom文件配置可以有提示
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
```

```
yml语法：
k:v：字面直接写
	字符串默认不加上单引号或双引号；
	"":双引号，不会转义字符串里的特殊字符；'':会转义特殊字符，特殊字符最终只是一个普通的字符串数据
对象属性直接写，还有一种行内写法，user: {name: zhangsan,age: 13}
数组、list的写法：行内写法：zoo: [cat,dog,pig]
zoo:
 - cat
 - dog
 - pig
map的写法：maps: {k1: v1,k2: v2}
```

**@PropertySource**与**@ImportResource**

可以将元数据的配置内容单独的建一个文件，如`person.properties`文件

```java
/*
* @PropertySource,加载指定的配置文件
*/
@Component
@PropertySource(value="{classpath:person.properties}")
public class Person {
    private String username;
    private Integer password;
```

```java
/*
*@ImportResource,导入自己写的spring的配置文件
*<id="" class="...">会被注入
*/
@ImportResource(locations = {"classpath:bean.xml"})
@SpringBootApplication
public class DemoApplication {
    
}
```

springBoot推荐给容器中添加组件的方式，推荐使用全注解的方式

**@Conﬁguration**注解：指明当前类是一个配置类，可以替代之前的Spring配置文件

```java
@Configuration
public class MyConfig {
    @Bean
    public User provideUser(){
        return new User();
    }
}
```

**Profile**是Spring对不同的环境提供不同功能的支持，可以通过激活、指定参数快速的切换环境

（1）多profile文件形式：application-{profile}.properties/yml

spring 的配置文件有两种形式 一种是properties 文件 ，一种是 yaml 文件 ，不管哪一种都可以用文件命名的形式区分不同环境的配置 。如：开发环境 ：application-dev.properties，生产环境：application-prod.properties
然后在 application.properties 文件中激活，当前的环境 ：spring.profiles.active = dev ,激活开发环境 

（2）多profile文档块模式： 这个只针对yml文件格式 ，方便写在一个文件中 ，使用`---`分隔

（3）命令行激活：--spring.profiles.active=dev

**SpringBoot配置文件的加载位置和优先级**：

SpringBoot启动会扫描以下位置的application.yml或者 application.properties文件作为SpringBoot的默认配置文件。

-file:./config/   

-file:./

-classpath:/config/

-classpath:/

即根目录下的config目录下，然后是 根目录下，然后是classpath路径下的config目录下，最后是classpath路径下。

优先级由高到低，高优先级的配置会覆盖低优先级的配置。

**自动配置的原理**

Spring Boot的启动类上有一个`@SpringBootApplication`注解，这个注解是Spring Boot项目必不可少的注解,它是一个复合注解或派生注解，在`@SpringBootApplication`中有一个注解`@EnableAutoConfiguration `，这个注解开启了自动配置的功能。

`@EnableAutoConfiguration `这个注解也是一个派生注解，其中的关键功能由@Import提供，其导入的`AutoConfigurationImportSelector`的

```
List<String> configurations = getCandidateConfigurations(annotationMetadata, attributes);
```

```
List<String> configurations = SpringFactoriesLoader.loadFactoryNames(getSpringFactoriesLoaderFactoryClass(),      getBeanClassLoader());
```

`SpringFactoriesLoader.loadFactoryNames()`扫描所有具有META-INF/spring.factories的jar包。 这个**spring.factories**文件也是一组一组的key=value的形式，其中一个key是EnableAutoConfiguration类的全类名，而它的value是一个xxxxAutoConfiguration的类名的列表，这些类名以逗号分隔。

```properties
# Auto Configure
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.springframework.boot.autoconfigure.admin.SpringApplicationAdminJmxAutoConfiguration,\
org.springframework.boot.autoconfigure.aop.AopAutoConfiguration,\
org.springframework.boot.autoconfigure.amqp.RabbitAutoConfiguration,\
```

每一个这样的xxxAutoConfiguration类都是容器的一个组件，都加入到容器中，用它们来做自动配置。

自动配置信息有了，那么自动配置还差什么呢？ 

 `@Conditional` 注解。 当这个注解所指定的条件成立，才给容器中添加组件，配置里面的内容才生效。

 以**HttpEncodingAutoConfiguration（Http编码自动配置）**为例解释自动配置原理 

```java
//表示这是一个配置类，以前编写的配置文件一样，也可以给容器中添加组件
@Configuration(proxyBeanMethods=false)

//启动指定类的ConfigurationProperties功能；将配置文件中对应的值和HttpEncodingProperties绑定起来；
@EnableConfigurationProperties(HttpProperties.class)

	[
	@ConfigurationProperties(prefix = "spring.http")
	public class HttpProperties {
	]
	
//Spring底层@Conditional注解（Spring注解版），根据不同的条件，如果满足指定的条件，
//整个配置类里面的配置就会生效；判断当前应用是否是web应用，如果是，当前配置类生效
@ConditionalOnWebApplication(type=ConditionalOnWebApplication.Type.SERVLET)
        
//判断当前项目有没有这个类CharacterEncodingFilter；SpringMVC中进行乱码解决的过滤器；        
@ConditionalOnClass(CharacterEncodingFilter.class)
        
//判断配置文件中是否存在某个配置spring.http.encoding.enabled；如果不存在，判断也是成立的
//即使我们配置文件中不配置pring.http.encoding.enabled=true，也是默认生效的；
@ConditionalOnProperty(prefix = "spring.http.encoding", value = "enabled", matchIfMissing = true)
```

可以通过在yum文件中`debug = true`属性，来让控制台打印自动配置报告。

**SpringBoot自动装配的原理：**

SpringBoot启动的时候会通过@EnableAutoConfiguration注解找到META-INF/spring.factories配置文件中的所有自动配置类，并对其进行加载，而这些自动配置类都是以AutoConfiguration结尾来命名的，它实际上就是一个JavaConfig形式的Spring容器配置类，它能通过以Properties结尾命名的类中取得在全局配置文件中配置的属性如：server.port，而XxxxProperties类是通过@ConfigurationProperties注解与全局配置文件中对应的属性进行绑定的。 

### SpringBoot启动流程分析

### 日志

日志门面：SLFJ(Simple Logging Facade for Java)

日志实现：Log4j、Logback

可以直接调用日志抽象层里面的方法

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
public class HelloWorld {
  public static void main(String[] args) {
    Logger logger = LoggerFactory.getLogger(HelloWorld.class);
    logger.info("Hello World");
  }
}
```

如果想把其它的框架转换成SLF4J,先将系统中其他框架排除出去，用中间包替换原有的日志框架，导入slf4j其他实现。引入其他框架时，只需要把这个框架依赖的日志框架排除掉，SpringBoot能自动适配所有的框架。

SpringBoot默认配置

```java
//日志的级别
//由低到高，可以调整输出的日志级别，默认使用info级别
logger.trace("trace日志");
logger.debug("debug日志");
logger.info("info日志");
logger.warn("warn日志");
logger.error("errors");
```

```properties
logging.level.com = trace
logging.file.path = D:/springboot.log
```

指定配置，给类路径下放上每个日志框架自己的配置文件即可；SpringBoot就不使用其他默认配置的了。

定义自己的日志配置文件

| Logging System          | Customization                                                |
| :---------------------- | :----------------------------------------------------------- |
| Logback                 | `logback-spring.xml`, `logback-spring.groovy`, `logback.xml`, or `logback.groovy` |
| Log4j2                  | `log4j2-spring.xml` or `log4j2.xml`                          |
| JDK (Java Util Logging) | `logging.properties`                                         |

`logback.xml`:直接就被日志框架识别了

`logback-spring.xml`:可以指定在某个环境下生效

```properties
<springProfile name="dev">
    <!-- configuration to be enabled when the "dev" or "staging" profiles are active -->
</springProfile>
```

## Spring Data

Spring Data是Spring的一个子项目，用于简化数据库的访问，支持NoSQL和关系型数据库。其主要的目标是使数据库的访问变得方便快捷。

JPA Spring Data致力于减少数据库访问层的开发量，开发者唯一要做的只是声明持久层的接口。

（1）编写实体类和数据表映射

```java
@Entity//表明是一个实体类，和数据表映射的类
@Table(name="book")//指定和哪个数据表对应；如果省略，默认表名就是book
public class Book {
    @Id //标明主键
    @GeneratedValue(strategy = GenerationType.AUTO) //自增主键
    private Integer id;
    
    @Column(name = "last_Name",length = 20)//和数据表对应的列，省略默认name为表名
    private String name;
```

（2）编写Dao接口来操作实体类对应的数据表

```java
public interface BookRepository extends JpaRepository<Book,Integer>{
    
}
```

（3) 编写Controller层

```java
public class BookController {
    @Autowired
    BookRepository bookRepository;

    @GetMapping("/book")
    public List<Book> getBooks(){
        List<Book> all = bookRepository.findAll();
        return all;
    }
```

(4)application中的配置

```properties
spring:
  datasource:
    username: root
    password: 123456
    url: jdbc:mysql://localhost:3306/test?serverTimezone=UTC  
  jpa:
    hibernate:
      ddl-auto: update #更新或者创建数据表结构
```

`Repository<T,ID>`，Repository是一个空接口，即是一个标记接口，若我们定义的接口继承了Repository,则该接口会被IOC容器识别为一个Repository Bean。

```java
//还可以通过注解实现
@RepositoryDefinition(domainClass = Book.class,idClass = Integer.class)
public interface BookRepository {
    
}
```

Repository查询方法定义规范：

按照Spring Data规范，查询方法以`find`,`read`,`get`开头。涉及条件查询时，条件的属性用条件关键字连接。

```java
//WHERE lastName LIKE ?% AND id < ?
List<Person> getByLastNameStartingWithAndIdLessThan(String lastName, Integer id)
```

上面的这个不好使，我们来使用`@Query`注解灵活查询。

```java
//传递参数的方式：使用占位符、命名参数
@Query("SELECT b FROM Book b WHERE b.id>?1 And b.name Like ?2%")
List<Book> getBookByIdAndName(Integer id,String name);

@Query("update Book b set b.author=:author where b.id=:id")
void updateBook(@Param("id") Integer id,@Param("author") String author);

//nativeQuery = true使用原生的SQL查询
@Query(value="select count(id) from book",nativeQuery = true)
long getTotalcount();
```

可以通过自定义JPOL完成UPDATE和DELETE操作，JPQL不支持INSERT

```java
//使用时在Controller方法上要加一个@Transactional注解
@Modifying
@Query("update Book b set b.author=:author where b.id=:id")
void updateBook(@Param("id") Integer id,@Param("author") String author);
```

# Spring重点理解

## 自定义Spring注解

[博文链接](https://blog.csdn.net/xsp_happyboy/article/details/80987484)

注解其实就是一种标记，可以在程序代码中的关键节点（类、方法、变量、参数、包）上打上这些标记，然后程序在编译时或运行时可以检测到这些标记从而执行一些特殊操作。在底层实现上，所有定义的注解都会自动继承`java.lang.annotation.Annotation`接口。

注意事项

> 访问修饰符必须为public，不写默认为public
>
> 元素的类型只能是基本数据类型、String、Class、枚举类型、注解类型。default代表默认值。

常用的元注解

` @Target`： 专门用来限定某个自定义注解能够被应用在哪些Java元素上面的。限定它可以用在类上、接口上、方法上还是其他的。

`@Retention`：限定注解的生命周期。如Java源文件阶段、编译到class文件阶段、运行期阶段。

`@Documented`： 用来指定自定义注解是否能随着被定义的java文件生成到JavaDoc文档当中 

`@Inherited`： 指定某个自定义注解如果写在了父类的声明部分，那么子类的声明部分也能自动拥有该注解。@Inherited注解只对那些@Target被定义为ElementType.TYPE的自定义注解起作用。

 ```java
@Target(value = {ElementType.TYPE,ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface MyAnnotation {
    String name();
    int age() default 18;
    int[] score();
}
 ```

获得注解的信息

```java
public class TestMyAnnotation {
    
    @MyAnnotation(name = "zhangsan",age = 15,score = {88,55,22})
    public void test(){
        System.out.println("Good Good Study, Day Day Up!");
    }
    
    public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, IllegalAccessException, InstantiationException, InvocationTargetException {
        Class c = Class.forName("com.spring.zhujie.TestMyAnnotation");
        TestMyAnnotation o = (TestMyAnnotation) c.newInstance();
        Method test = c.getMethod("test");
        test.invoke(o);
        if(test.isAnnotationPresent(MyAnnotation.class)){
            System.out.println("配置了注解");
            MyAnnotation annotation = test.getAnnotation(MyAnnotation.class);
            System.out.println(annotation.age() + annotation.name() + annotation.score()[0]);
        }
    }
}
```

从上面的实现逻辑我们不能发现，借助于java的反射我们可以直接拿到一个类里所有的方法，然后再拿到方法上的注解，当然，我们也可以拿到字段上的注解。借助于反射我们可以拿到几乎任何属于一个类的东西。 

### @Autowired注解实现逻辑

[博文链接](https://mp.weixin.qq.com/s/gRqZwUV791RtCI1xCoV3Qw)

知道了上面的知识，我们不难想到，上面的注解虽然简单，但是@Autowired和他最大的区别应该仅仅在于注解的实现逻辑，其他利用反射获取注解等等步骤应该都是一致的。

先来看一下@Autowired这个注解在spring的源代码里的定义是怎样的，如下所示：

```java
package org.springframework.beans.factory.annotation;
 
import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
 
@Target({ElementType.CONSTRUCTOR, ElementType.METHOD, ElementType.PARAMETER, ElementType.FIELD, ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Autowired {
    boolean required() default true;
}
```

阅读代码我们可以看到，Autowired注解可以应用在构造方法，普通方法，参数，字段，以及注解这五种类型的地方，它的保留策略是在运行时。

使用@Autowired注入的bean对于目标类来说，从代码结构上来讲也就是一个普通的成员变量，@Autowired和spring一起工作，通过反射为这个成员变量赋值，也就是将其赋为期望的类实例。 

### @Autowired和@Resouce的区别

[博文链接](https://www.zhihu.com/question/39356740)

@Autowired功能虽说非常强大，但是也有些不足之处。比如：比如它跟spring强耦合了，如果换成了JFinal等其他框架，功能就会失效。而@Resource是JSR-250提供的，它是Java标准，绝大部分框架都支持。

除此之外，有些场景使用@Autowired无法满足的要求，改成@Resource却能解决问题。接下来，我们重点看看@Autowired和@Resource的区别。

- @Autowired默认按byType自动装配，而@Resource默认byName自动装配。
- @Autowired只包含一个参数：required，表示是否开启自动准入，默认是true。而@Resource包含七个参数，其中最重要的两个参数是：name 和 type。
- @Autowired如果要使用byName，需要使用@Qualifier一起配合。而@Resource如果指定了name，则用byName自动装配，如果指定了type，则用byType自动装配。
- @Autowired能够用在：构造器、方法、参数、成员变量和注解上，而@Resource能用在：类、成员变量和方法上。
- @Autowired是spring定义的注解，而@Resource是JSR-250定义的注解。

## 自定义Stater

starter的命名有一种习惯，官方的starter一般都是spring-boot-starter-xxx，而我们自定义的starter一般都是xxx-spring-boot-starter。

## Spring-bean的循环依赖以及解决方式

[博文链接](https://zhuanlan.zhihu.com/p/84267654)、[博文链接](https://blog.csdn.net/u010853261/article/details/77940767)

* Spring是通过递归的方式获取目标bean及其所依赖的bean的
* Spring实例化一个bean的时候，是分两步进行的，首先实例化目标bean，然后为其注入属性

























