# Servlet总结

1. Servlet是javaWeb的三大组件之一，它属于动态资源。Servlet的作用是请求处理，服务器会把接收到的请求交给Servlet来处理，在Servlet中通常需要接收请求数据、处理请求，完成响应。在Java Web程序中，**Servlet**主要负责接收用户请求 `HttpServletRequest`,在`doGet()`,`doPost()`中做相应的处理，并将回应`HttpServletResponse`反馈给用户。**Servlet** 可以设置初始化参数，供Servlet内部使用。一个Servlet类只会有一个实例，在它初始化时调用`init()`方法，销毁时调用`destroy()`方法**。**Servlet需要在web.xml中配置（MyEclipse中创建Servlet会自动配置），**一个Servlet可以设置多个URL访问**。**Servlet不是线程安全**，因此要谨慎使用类变量。

2. Servlet中的大多数方法不由我们来调用，对象也不由我们来创建，而是由Tomcat来调用和创建。

3. 特性：线程不安全，一个类只有一个对象，当然可能存在多个Servlet类。

## Servlet接口中的方法

### 生命周期

**Web容器加载Servlet并将其实例化后，Servlet生命周期开始**，容器运行其**init()方法**进行Servlet的初始化；请求到达时调用Servlet的**service()方法**，service()方法会根据需要调用与请求对应的**doGet或doPost**等方法；当服务器关闭或项目被卸载时服务器会将Servlet实例销毁，此时会调用Servlet的**destroy()方法**。**init方法和destroy方法只会执行一次，service方法客户端每次请求Servlet都会执行**。Servlet中有时会用到一些需要初始化与销毁的资源，因此可以把初始化资源的代码放入init方法中，销毁资源的代码放入destroy方法中，这样就不需要每次处理客户端的请求都要初始化与销毁资源。 

```java
public void destroy(){} //在Servlet被销毁之前调用，只执行一次 

public void init(ServletConfig servletConfig) throws ServletException {}
//在Servlet对象创建之后执行，并且只执行一次

public void service(ServletRequest request, ServletResponse response) throws ServletException, IOException {}
 //会被调用多次，每次处理请求都在调用这个方法

java.lang.String getServletInfo()
ServletConfig getServletConfig()
```

### ServletConfig接口

```java
String getInitParameter(String name)//通过名称获取指定初始化参数的值
Enumeration<String>	getInitParameterNames()//获取所有初始化参数的名称
ServletContext getServletContext()//获取Servlet上下文
String getServletName()//获取所有初始化参数的名称
```

如何让浏览器访问访问Servlet，在Web.xml中配置Servlet

<url-pattern>是<servlet-mapping>的子元素，用来指定servlet的访问路径，即url。它必须是以“/”开头。中间可以加通配符，如/servlet/*（路径匹配）、/*、*.do（扩展名匹配）

```java
<servlet>
	<servlet-name>xxx</servlet-name>
	<servlet-class>cn.web.servlet.Aservlet</servlet-class>
</servlet>
<servlet-mapping>
	<servlet-name>xxx</servlet-name>
	<url-pattern>/Aservlet</url-pattern>
</servlet-mapping>
```

```java
public class Aservlet extends HttpServlet {
	public void doGet(HttpServletRequest req, HttpServletResponse resp)
			throws ServletException, IOException {
		System.out.println("doGet");
	}
}
```

然后在浏览器中访问即可！

` http://localhost:8080/项目名称/Aservlet `

servletContext接口

一个项目只有一个servletContext对象，我们可以在N多个Servlet中获取这唯一的对象，使用它可以给多个servlet传递数据。servletContext对象的作用是在整个Web应用的动态资源之间共享数据！例如在Aservlet中向ServletContext对象中保存一个值，然后在Bservlet中就可以获取这个值，这就是共享数据了。

servletContext是javaWeb四大域对象之一。所有域对象都有存取数据的功能，因为域对象内部有一个Map,用来存储数据。`PageContext`、`ServletRequest`、`HttpSession`、`ServletContext`

```java
public class Aservlet extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
			ServletContext application = this.getServletContext();
			application.setAttribute("name","zhangsan");
	}
}
```

请求响应的流程：创建servlet对象，把请求数据封装到request中，创建response对象，调用Servlet的service()方法传递这两个参数。在servlet.service方法中使用request获取请求数据，使用response完成响应。

### HttpServletResponse接口

5秒后自动跳转

`response.setHeader("Refresh","5;URL=www.baidu.com");`

重定向

`response.setHeader("Location","www.baidu.com");
response.setStatus(302);`

快捷的重定向方法

`response.sendRedirect("www.baidu.com");`

## get与post请求

可以把 get 和 post 当作两个不同的行为，两者并没有什么本质区别，底层都是 TCP 连接。 get请求用来从服务器上获得资源，而post是用来向服务器提交数据。比如你要获取人员列表可以用 get 请求，你需要创建一个人员可以用 post 。这也是 Restful API 最基本的一个要求。什么情况下调用doGet()和doPost()？Form标签里的method的属性为get时调用doGet()，为post时调用doPost()。 

## 转发(Forward)和重定向(Redirect)的区别

**转发是服务器行为，重定向是客户端行为。**

**转发（Forward）** 通过RequestDispatcher对象的forward（HttpServletRequest request,HttpServletResponse response）方法实现的。RequestDispatcher可以通过HttpServletRequest 的getRequestDispatcher()方法获得。例如下面的代码就是跳转到login_success.jsp页面。

```java
request.getRequestDispatcher("login_success.jsp").forward(request, response);
```

**重定向（Redirect）** 是利用服务器返回的状态码来实现的。客户端浏览器请求服务器的时候，服务器会返回一个状态码。服务器通过 `HttpServletResponse` 的 `setStatus(int status)` 方法设置状态码。如果服务器返回301或者302，则浏览器会到新的网址重新请求该资源。

1. **从地址栏显示来说**

forward是服务器请求资源,服务器直接访问目标地址的URL,把那个URL的响应内容读取过来,然后把这些内容再发给浏览器.浏览器根本不知道服务器发送的内容从哪里来的,所以它的地址栏还是原来的地址. redirect是服务端根据逻辑,发送一个状态码,告诉浏览器重新去请求那个地址.所以地址栏显示的是新的URL.

2. **从数据共享来说**

forward:转发页面和转发到的页面可以共享request里面的数据. redirect:不能共享数据.

3. **从运用地方来说**

forward:一般用于用户登陆的时候,根据角色转发到相应的模块. redirect:一般用于用户注销登陆时返回主页面和跳转到其它的网站等

## request.getAttribute()和 request.getParameter()有何区别

**从获取方向来看：**

`getParameter()`是获取 POST/GET 传递的参数值；

`getAttribute()`是获取对象容器中的数据值；

**从用途来看：**

`getParameter()`用于客户端重定向时，即点击了链接或提交按扭时传值用，即用于在用表单或url重定向传值时接收数据用。

`getAttribute()` 用于服务器端重定向时，即在 sevlet 中使用了 forward 函数,或 struts 中使用了 mapping.findForward。 getAttribute 只能收到程序用 setAttribute 传过来的值。

另外，可以用 `setAttribute()`,`getAttribute()` 发送接收对象.而 `getParameter()` 显然只能传字符串。 `setAttribute()` 是应用服务器把这个对象放在该页面所对应的一块内存中去，当你的页面服务器重定向到另一个页面时，应用服务器会把这块内存拷贝另一个页面所对应的内存中。这样`getAttribute()`就能取得你所设下的值，当然这种方法可以传对象。session也一样，只是对象在内存中的生命周期不一样而已。`getParameter()`只是应用服务器在分析你送上来的 request页面的文本时，取得你设在表单或 url 重定向时的值。

**总结：**

`getParameter()`返回的是String,用于读取提交的表单中的值;（获取之后会根据实际需要转换为自己需要的相应类型，比如整型，日期类型啊等等）

`getAttribute()`返回的是Object，需进行转换,可用`setAttribute()`设置成任意对象，使用很灵活，可随时用

## 实现会话跟踪的技术有哪些

1. **使用Cookie** 

Cookie是由http协议制定的，由服务器保存cookie到浏览器，下次浏览器请求服务器把上一次请求得到的cookie归还给服务器。

从客户端发送Cookie

```java
Cookie cookie1 = new Cookie("aaa","AAA");
response.addCookie(cookie1);
//Cookie不止有name和value属性，还有maxAge，表示Cookie保存的最大时长
```

从客户端读取Cookie

```java
 Cookie[] cookies = request.getCookies();
    if(cookies != null){
    	for(Cookie c:cookies){
    		out.print(c.getName()+c.getValue()+"<br/>");
    	}
    }
```

**优点:** 数据可以持久保存，不需要服务器资源，简单，基于文本的Key-Value

**缺点:** 大小受到限制，用户可以禁用Cookie功能，由于保存在本地，有一定的安全风险。

2.  **URL 重写** 

在URL中添加用户会话的信息作为请求的参数，或者将唯一的会话ID添加到URL结尾以标识一个会话。

**优点：** 在Cookie被禁用的时候依然可以使用

**缺点：** 必须对网站的URL进行编码，所有页面必须动态生成，不能用预先记录下来的URL进行访问。

3.  **隐藏的表单域** 

```java
<input type="hidden" name ="session" value="..."/>
```

**优点：** Cookie被禁时可以使用

**缺点：** 所有页面必须是表单提交之后的结果。

4.  **HttpSession** 

HttpSession 是由JavaWeb提供的，用来会话跟踪的类。session是服务器端对象，保存在服务器端。HttpSession是Servlet三大域对象（request、session、application/ServletContext ))之一，所以它也有setAttribute()、getAttribute（）removeAttribute()方法。HttpSesssion底层依赖Cookie,或是URL重写。

 在所有会话跟踪技术中，HttpSession对象是最强大也是功能最多的。当一个用户第一次访问某个网站时会自动创建 HttpSession，每个用户可以访问他自己的HttpSession。可以通过HttpServletRequest对象的getSession方 法获得HttpSession，通过HttpSession的setAttribute方法可以将一个值放在HttpSession中，通过调用 HttpSession对象的getAttribute方法，同时传入属性名就可以获取保存在HttpSession中的对象。与上面三种方式不同的 是，HttpSession放在服务器的内存中，因此不要将过大的对象放在里面，即使目前的Servlet容器可以在内存将满时将HttpSession 中的对象移到其他存储设备中，但是这样势必影响性能。添加到HttpSession中的值可以是任意Java对象，这个对象最好实现了 Serializable接口，这样Servlet容器在必要的时候可以将其序列化到文件中，否则在序列化时就会出现异常。

```java
HttpSession session = request.getSession();
```

### 3.Cookie和Session的的区别

Cookie 和 Session都是用来跟踪浏览器用户身份的会话方式，但是两者的应用场景不太一样。

**Cookie 一般用来保存用户信息** 比如①我们在 Cookie 中保存已经登录过得用户信息，下次访问网站的时候页面可以自动帮你登录的一些基本信息给填了；②一般的网站都会有保持登录也就是说下次你再访问网站的时候就不需要重新登录了，这是因为用户登录的时候我们可以存放了一个 Token 在 Cookie 中，下次登录的时候只需要根据 Token 值来查找用户即可(为了安全考虑，重新登录一般要将 Token 重写)；③登录一次网站后访问网站其他页面不需要重新登录。**Session 的主要作用就是通过服务端记录用户的状态。** 典型的场景是购物车，当你要添加商品到购物车的时候，系统不知道是哪个用户操作的，因为 HTTP 协议是无状态的。服务端给特定的用户创建特定的 Session 之后就可以标识这个用户并且跟踪这个用户了。

Cookie 数据保存在客户端(浏览器端)，Session 数据保存在服务器端。

Cookie 存储在客户端中，而Session存储在服务器上，相对来说 Session 安全性更高。如果使用 Cookie 的一些敏感信息不要写入 Cookie 中，最好能将 Cookie 信息加密然后使用到的时候再去服务器端解密。

## 监听器

ServletContextListener(生命周期监听)它有两个方法，一个在出生时调用，一个在死亡时调用。

ServletContextAttributeListener(属性监听)它有三个方法，一个在添加属性时调用，一个在替换属性时调用，最后一个在移除属性时调用。

## 过滤器Filter

可以拦截请求，它会在一组资源的前面执行。

过滤器的应用场景：执行目标资源前做预处理工作，例如设置编码。通过条件判断是否放行，如检验用户是否已经登录，或者用户IP是否已经被禁用。在目标资源执行后，做一些后续的特殊处理工作，例如把目标资源输出的数据进行处理。

## TCP/IP协议

从字面意义上讲，有人可能会认为 TCP/IP 是指 TCP 和 IP 两种协议。实际生活当中有时也确实就是指这两种协议。然而在很多情况下，它只是利用 IP 进行通信时所必须用到的协议群的统称。具体来说，IP 或 ICMP、TCP 或 UDP、TELNET 或 FTP、以及 HTTP 等都属于 TCP/IP 协议。他们与 TCP 或 IP 的关系紧密，是互联网必不可少的组成部分。TCP/IP 一词泛指这些协议，因此，有时也称 TCP/IP 为网际协议群。 

## TCP 和 UDP

1. **UDP**

- UDP 不提供复杂的控制机制，利用 IP 提供面向无连接的通信服务。
- 并且它是将应用程序发来的数据在收到的那一刻，立即按照原样发送到网络上的一种机制。即使是出现网络拥堵的情况，UDP 也无法进行流量控制等避免网络拥塞行为。
- 此外，传输途中出现丢包，UDP 也不负责重发。
- 甚至当包的到达顺序出现乱序时也没有纠正的功能。
- 如果需要以上的细节控制，不得不交由采用 UDP 的应用程序去处理。
- UDP 常用于一下几个方面：1.包总量较少的通信（DNS、SNMP等）；2.视频、音频等多媒体通信（即时通信）；3.限定于 LAN 等特定网络中的应用通信；4.广播通信（广播、多播）。

2. **TCP**

- TCP 与 UDP 的区别相当大。它充分地实现了数据传输时各种控制功能，可以进行丢包时的重发控制，还可以对次序乱掉的分包进行顺序控制。而这些在 UDP 中都没有。
- 此外，TCP 作为一种面向有连接的协议，只有在确认通信对端存在时才会发送数据，从而可以控制通信流量的浪费。
- 根据 TCP 的这些机制，在 IP 这种无连接的网络上也能够实现高可靠性的通信（ 主要通过检验和、序列号、确认应答、重发控制、连接管理以及窗口控制等机制实现）。

**1. 端口号**

数据链路和 IP 中的地址，分别指的是 MAC 地址和 IP 地址。前者用来识别同一链路中不同的计算机，后者用来识别 TCP/IP 网络中互连的主机和路由器。在传输层也有这种类似于地址的概念，那就是端口号。端口号用来识别同一台计算机中进行通信的不同应用程序。因此，它也被称为程序地址。

**1.1 根据端口号识别应用**

一台计算机上同时可以运行多个程序。传输层协议正是利用这些端口号识别本机中正在进行通信的应用程序，并准确地将数据传输。

**1.3 端口号的确定**

- 标准既定的端口号：这种方法也叫静态方法。它是指每个应用程序都有其指定的端口号。但并不是说可以随意使用任何一个端口号。例如 HTTP、FTP、TELNET 等广为使用的应用协议中所使用的端口号就是固定的。这些端口号被称为知名端口号，分布在 0~1023 之间；除知名端口号之外，还有一些端口号被正式注册，它们分布在 1024~49151 之间，不过这些端口号可用于任何通信用途。
- 时序分配法：服务器有必要确定监听端口号，但是接受服务的客户端没必要确定端口号。在这种方法下，客户端应用程序完全可以不用自己设置端口号，而全权交给操作系统进行分配。动态分配的端口号范围在49152~65535 之间。

## 三次握手

- 第一次握手：客户端将标志位SYN置为1，随机产生一个值seq=J，并将该数据包发送给服务器端，客户端进入SYN_SENT状态，等待服务器端确认。
- 第二次握手：服务器端收到数据包后由标志位SYN=1知道客户端请求建立连接，服务器端将标志位SYN和ACK都置为1，ack=J+1，随机产生一个值seq=K，并将该数据包发送给客户端以确认连接请求，服务器端进入SYN_RCVD状态。
- 第三次握手：客户端收到确认后，检查ack是否为J+1，ACK是否为1，如果正确则将标志位ACK置为1，ack=K+1，并将该数据包发送给服务器端，服务器端检查ack是否为K+1，ACK是否为1，如果正确则连接建立成功，客户端和服务器端进入ESTABLISHED状态，完成三次握手，随后客户端与服务器端之间可以开始传输数据了。

![](D:\blog\source\_posts\J2EE基础知识回顾\三次握手.jpeg)

















