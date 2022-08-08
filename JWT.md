## cookie

#### Cookie是什么

Cookie就是一些数据，用于存储服务器返回给客服端的信息，客户端进行保存。在下一次访问该网站时，客户端会将保存的cookie一同发给服务器，服务器再利用cookie进行一些操作。利用cookie我们就可以实现自动登录，保存游览历史，身份验证等功能。

#### Cookie有什么用

当我们打开一个网站时，如果这个网站我们曾经登录过，那么当我们再次打开网站时，发现就不需要再次登录了，而是直接进入了首页。例如bilibili，csdn等网站。其实就是游览器保存了我们的cookie，里面记录了一些信息，当然，这些**cookie是服务器创建后返回给游览器的。游览器只进行了保存。**下面展示bilibili网站保存的cookie。

![image-20220624100117120](F:\Typora\typora-user-images\image-20220624100117120.png)

**识别返回用户包括三个步骤**：服务器脚本向浏览器发送一组 Cookies。例如：姓名、年龄或识别号码等。 

- 浏览器将这些信息存储在本地计算机上，以备将来使用。 
- 当下一次浏览器向 Web 服务器发送任何请求时，浏览器会把这些 Cookies 信息发送到服务器，
- 服 务器将使用这些信息来识别用户。 

#### Cookie使用的常见方法

​        下面，我对java中Cookie对象的方法进行讲解

```java
new Cookie(String name, String value)：创建一个Cookie对象，必须传入cookie的名字和cookie的值
getValue()：得到cookie保存的值
getName()：获取cookie的名字
setMaxAge(int expiry)：设置cookie的有效期，默认为-1。这个如果设置负数，表示客服端关闭，cookie就会删除。0表示马上删除。正数表示有效时间，单位是秒。
setPath(String uri)：设置cookie的作用域

HttpServletRequest和HttpServletResponse对Cookie进行操作的常见方法

response.addCookie(Cookie cookie)：将cookie给客户端进行保存
resquest.getCookies()：得到客服端传过来的所有cookie对象
```

**会话：** 我 和 张三 之间 说话通信交流  ；

​			客户端 和 服务器端 网络通信交流。

现在 客户端 和 服务器端 进行通信，只要客户端不关闭 ，并且服务器端也不关闭 ，那么就在一次会话中。

一次会话中可以包含多次请求和响应。

一次会话 ：浏览器第一次给服务器资源发送请求，会话建立，直到有一方断开为止。

**现在 客户端 和 服务器端 之间发送了多次请求和多次响应，能干什么？**

其实就干一件事情，就是 共享数据，会话的功能 是 共享数据。

**怎么共享数据？在哪共享数据？**

**会话**技术 的功能就是在一次会话的范围内多次 请求间，来共享数据。

了解过HTTP协议的，都知道 HTTP协议 是无状态的，无状态的意思就是我客户端发了多次请求，然后服务器给我了多次响应，**每一次请求、响应之间** 和 **其他的请求、响应之间** 都是相互独立的。

**他们之间并不能进行数据的交流和交换，那么我们要进行数据的交流和交换，怎么办？**

只能使用 **会话**技术 来解决这么一个问题。**会话**技术 的功能就是在一次会话的范围内多次 请求间，来共享数据。

举个例子，演示一下这个过程，比如说我打开浏览器，进入京东买东西，看见一个好物就加入购物车，又看见一个好物，又就加入购物车，又看见一个好物，又就加入购物车。每点击一次 “加入购物车” 都发起了一个新的请求，然后最终我去点击 “去购物车结算”，点击 “去购物车结算”的时候，你想，那么我最终怎么能知道刚才那些请求买了什么数据？

因为点击 “去购物车结算 ” 又是一次新的请求了，那这个数据要不要共享，那肯定要，对不对，那么现在京东怎么解决这个问题的？京东也使用的是 **会话**技术。所以，你要知道 **会话**技术 就是来干这个事儿的，他的功能就是在一次会话的范围内去共享数据，说得更准确一点是在一次会话的范围内多次 请求间，来共享数据。

 **会话技术 分两类，共享数据有两种方式**：客户端会话技术、服务器端会话技术

**客户端会话技术：Cookie**  
 将来是把数据存储在客户端的，叫客户端会话技术

浏览器第一次来请求访问百度服务器端时，百度服务器端 发现是第一次来访问（因为刚发来的请求中的header请求头没捎带cookies）然后就创建一个cookie对象，并且在 响应 浏览器请求 的同时捎带刚新建cookie一并发送给浏览器。

![image-20220623141753794](F:\Typora\typora-user-images\image-20220623141753794.png)

当浏览器再来请求时，请求中的请求头包含cookie，所以服务器就能辨别曾经访问过。当浏览器再一次发来请求时，服务器发现 请求中的请求头包含cookie，服务器就获取cookie，看看是谁来访问，服务器在 响应 浏览器请求 的同时继续捎带cookie一并发送给浏览器。

```java
1、若浏览器第一次来请求访问服务器端时，服务器端就 创建一个 Cookie对象，必须传入cookie的名字和cookie的值：
new Cookie(String name,String value)   

//设置path，让当前服务器下部署的所有项目共享cookie信息
c.setPath("/");
//如果这样设置，那么未来只有在/cookieSession项目下的资源，才能够获取c 对应的cookie信息
c.setPath("/cookieSession"); 

2、服务器端发送Cookie 给客户端 : 
response.addCookie(Cookie cookie);

3、客户端再次请求访问服务器端时，请求里携带着Cookie 一起发送给服务器端，服务器端获取Cookie ，拿到数据，遍历Cookies：
// 获取 Cookie
Cookie[] cookies = request.getCookies();
//遍历cookie数组
if (cookies != null && cookies.length > 0 ){
   for (Cookie cookie : cookies) {  //遍历了之后，每个cookie都知道了
    //3 获取cookie的名称
    String name = cookie.getName();   
    // 4 判断名称是否是：lastTime
     if ("lastTime".equals(name)){
        // 有该cookies,不是第一次访问
        // 1 响应数据
        String value = cookie.getValue();   //得到cookie保存的值
        response.getWriter().write("<h1>欢迎回来，您上次访问的时间为：" + value+"</h1>");  //由于是中文，需要编码
         //2 写回Cookie : lastTime=2019-11-25-21.44
       }
   }
}

```

换一个浏览器访问 ，也已经不是同一个会话。

#### Cookie 实现原理： 

基于响应头set-cookie 和请求头cookie实现

假设 CookieDemo1、CookieDemo2是百度服务器里边的两个资源，两个不同的请求路径



![image-20220623133550384](F:\Typora\typora-user-images\image-20220623133550384.png)

因为浏览器和服务器之间是有一个HTTP请求协议的约束，HTTP协议里边响应头规定了，如果浏览器收到一个set-cookie 头，浏览器会自动的干一件事 , 浏览器会将这个头里边携带的这些数据，这个数据是啥呢？就是这么一串数据  msg=hello，将这些数据保存到客户端浏览器上，保存了之后，并且下一次再次向服务器发送请求的时候，它会将这个数据 cookie : msg=hello带过去的。

**怎么带过去？**

我们就来说一下第二次请求的时候，这个数据会被放到请求的消息头里边，使用一个消息头，这个消息头叫做cookie，这个头携带着cookie : msg=hello。带过去了，这都是浏览器自动做的事，带过去了之后，在服务器里边，我们可以写代码来获取这么一个请求头里边的数据，但是我们使用的是 JavaWeb给我们封装好的API来做的这个操作，不需要我们单独再去以这个请求头 来操作，简化我们的开发步骤而已。所以，服务器帮我们办了很多的事，或者 HTTP协议 和 浏览器 帮我们干了很多事。我们需要关注的其实非常的少，只是通过API去发送cookie，以及通过API来获取cookie，那么现在我们可以来通过抓包工具来看一下。

**一次可不可以发送多个cookie?**  

 可以 ,可以创建多个Cookie对象，使用response 调用多次addCookie方法发送cookie即可。

我们在发送HTTP请求时，发现游览器将我们的cookie都进行了携带**(注意：游览器只会携带在当前请求的url中包含了该cookie中path值的cookie)**，并且是以key：value的形式进行表示的。多个cookie用；进行隔开。 

```
创建两个 Cookie 对象：
您可以调用带有 cookie 名称和 cookie 值的 Cookie 构造函数，cookie 名称和 cookie 值都是字符串。
```

```java
//        1 创建 Cookie对象
        Cookie c1 = new Cookie("msg","hello");
        Cookie c2 = new Cookie("name","zhangsan");
//        2 发送Cookie
        response.addCookie(c1);
        response.addCookie(c2);
```

![image-20220623134704704](F:\Typora\typora-user-images\image-20220623134704704.png)

在服务器设置了2个cookie，返回给游览器。通过抓包，我们发现在HTTP响应中， cookie的表示形式是，Set-Cookie：cookie的名字，cookie的值。如果有多个cookie，那么在HTTP响应中就使用多个Set-Cookie进行表示。

**Cookie在浏览器中保存多长时间？**

cookie有2种存储方式，一种是会话性，一种是持久性。

**会话性：**如果cookie为会话性，那么cookie仅会保存在客户端的内存中，当我们关闭客服端时cookie也就失效了
**持久性：**如果cookie为持久性，那么cookie会保存在用户的硬盘中，直至生存期结束或者用户主动将其销毁。

cookie我们是可以进行设置的，我们可以人为设置cookie的有效时间，什么时候创建，什么时候销毁。

默认情况下，当浏览器关闭后，Cookie数据就被销毁，因为Cookie数据是存储在浏览器内存里的

还有不默认情况，我们经常会希望Cookie信息，它在浏览器关闭后仍然能够被保存下来，那么在内存中的数据怎么能够被保存下来？那么这就牵扯到我们的文件，在文件或者硬盘上的文件里面的数据是可以被持久化的存储，所以，接下来我们就可以来去设置cookie的生命周期，来去让它持久化的存储，那么持久化存储这个地方就需要去使用一个方法：

```
setMaxAge(int seconds)
```

那么这个方法啥意思？这个方法是cookie对象的方法，那么这个方法可以传递一个int 的数字。传递的数字，可以代表不同的含义，那么int类型的数字还有三种的取值情况：

第一种是正数，第二种是负数 ：默认值，第三种是零：删除cookie信息。

那么这三种情况代表的含义不一样，如果你这个方法传递一个正数。那么它代表是持久化将cookie数据写到硬盘的文件中，那么也相当于持久化存储。这个正数是这个意思，设置cookie的存活时间。

```Java
//        1 创建 Cookie对象
        Cookie c1 = new Cookie("msg","setMaxage");
//        2 设置cookie的存活时间
        c1.setMaxAge(30);  //将cookie持久化到硬盘，30秒后自动删除cookie文件
//      c1.setMaxAge(-1);  //负数
//     c1.setMaxAge(0);   //零

//        3 发送Cookie
        response.addCookie(c1);
```

**cookie 能不能存中文？**

在Tomcat 8之前cookie中不能直接存储中文数据，需要将中文数据转码----一般采用URL编码（%E3）

 在Tomcat 8之后cookie支持存储中文数据。

```Java
 //1 创建 Cookie对象
     Cookie c = new Cookie("msg","你好");
     Cookie c1 = new Cookie("hello","您好");
     c.setMaxAge(60*60);   //60秒 * 60分钟 = 一小时
//2 发送Cookie
     response.addCookie(c);
```



在服务器中servlet判断是否有一个名为LastTime的cookie

 * 1、有，不是第一次访问

 * 1 响应数据：欢迎回来，您上次访问的时间为：2019-11-25-21.44

 * *      2 写回Cookie : lastTime=2019-11-25-21.44

 * 2、没有，第一次访问

 * 1 响应数据：你好，欢迎首次访问

 * 2 写回Cookie : lastTime=2019-11-25-21.44

   

有些人说，cookie是客户端的缓存，在实际工作中，也有很多人真的就把cookie当缓存来使用，网页的界面上需要显示的一些内容，又不想重复去服务器端获取，那就先放到cookie，既方便又快捷。

**那么cookie到底是不是缓存？**

我们来看一下Cookie的定义，Cookie其实也叫http Cookie。他是客户端用来存储会话ID，也叫 SessionID 的一种机制。

比如我们打开一个网页的时候，浏览器和服务器就建立了一次会话。这里需要注意的是，不管用户有没有登录，都是有建立会话的。在这次会话当中，浏览器向服务器请求数据，都会在请求头里自动带上，这个会话ID也是这个SessionID，浏览器关闭之后，这个Cookie 还存在硬盘上，下次打开的时候还可以继续使用。看完上面的说明，我们知道了，Cookie，主要是用来解决HTTP无状态，这个问题的，而且Cookie里面的内容，会随着HTTP请求的请求头，自动发送到服务器上。

**为什么说 http 协议是无状态协议**

因为它的每个请求都是完全独立的，每个请求包含了处理这个请求所需的完整的数据，发送请求不涉及到状态变更。即使在 HTTP/1.1 上，同一个连接允许传输多个 HTTP 请求的情况下，如果第一个请求出错了，后面的请求一般也能够继续处理

（当然，如果导致协议解析失败、消息分片错误之类的自然是要除外的）。

可以看出，这种协议的结构是要比有状态的协议更简单的，一般来说实现起来也更简单，不需要使用状态机，一个循环就行了。

**为什么不改进 http 协议使之有状态**

最初的 http 协议只是用来浏览静态文件的，无状态协议已经足够，这样实现的负担也很轻（相对来说，实现有状态的代价是很高的，要维护状态，根据状态来操作。）。随着 web 的发展，它需要变得有状态，但是不是就要修改 http 协议使之有状态呢？是不需要的。因为我们经常长时间逗留在某一个网页，然后才进入到另一个网页，如果在这两个页面之间维持状态，代价是很高的。其次，历史让 http 无状态，但是现在是对 http 提出了新的要求，按照软件领域的通常做法是，保留历史经验，在 http 协议上再加上一层实现我们的目的（“再加上一层，你可以做任何事”）。所以引入了其他机制来实现这种状态的连接。

**无状态协议的优缺点**

会话（Session）支持其实并不是一个缺点，反而是无状态协议的优点，因为对于有状态协议来说，如果将会话状态与连接绑定在一起，那么如果连接意外断开，整个会话就会丢失，重新连接之后一般需要从头开始（当然这也可以通过吸收无状态协议的某些特点进行改进）；

而 HTTP 这样的无状态协议，使用元数据（如 Cookies 头）来维护会话，使得会话与连接本身独立起来，这样即使连接断开了，会话状态也不会受到严重伤害，保持会话也不需要保持连接本身。另外，无状态的优点还在于对中间件友好，中间件无需完全理解通信双方的交互过程，只需要能正确分片消息即可，而且中间件可以很方便地将消息在不同的连接上传输而不影响正确性，这就方便了负载均衡等组件的设计。

无状态协议的主要缺点在于，单个请求需要的所有信息都必须要包含在请求中一次发送到服务端，这导致单个消息的结构需要比较复杂，必须能够支持大量元数据，因此 HTTP 消息的解析要比其他许多协议都要复杂得多。同时，这也导致了相同的数据在多个请求上往往需要反复传输，例如同一个连接上的每个请求都需要传输 Host、Authentication、Cookies、Server 等往往是完全重复的元数据，一定程度上降低了协议的效率。

**那么，什么情况下需要把数据存到Cookie里？什么情况下不应该存在cookie里？使用Cookie有哪些注意事项？**

第一，就是需要在HTTP请求里，自动发送给服务器的数据，可以放到Cookie里。

第二，就是不需要发送给服务器的数据，不应该放到Cookie里，这一点，是大家最容易忽略的。工作中尤其要注意。

第三，就是cookie存储在用户的硬盘上，没那么安全，所以需要保密的数据，不应该放到Cookie里。我曾经见到有人把用户名和密码都存到Cookie里的，这个就是不对的。

第四，就是Cookie有容量限制，要注意不能放太多东西。

第五，就是尽量不要让前端JavaScript 去写Cookie。尽量在后端写。

**关于cookie 底层的理论知识 ?**

**cookie 的传递会经过下边这 4 步：**

1. Client 发送 HTTP 请求给 Server
2. Server 响应，并附带 Set-Cookie:msg=hello 的头部信息
3. Client 保存 Cookie，之后请求 Server 会附带 Cookie 的头部信息
4. Server 从 Cookie 知道 Client 是谁了，返回相应的响应

> Cookie 中文翻译是甜品，使用 Cookie 可以自动填写用户名、密码等，是给用户的一点甜头。

Server 拿到 Cookie 后，通过什么信息才能判断是哪个 Client 呢？是通过服务器生成的 SessionID。



## session

#### **Session是什么？**

概念：服务器端会话技术。在一次会话的多次请求间共享数据，将数据保存在服务器端的对象( HttpSession )中  。

在网络应用中被称为 会话控制 。**Session**对象，是存储在服务器端的，主要**用来存储用户会话所需的属性数据和配置**。**SessionID **需要 **存储在浏览器端**，浏览器发送接口请求的时候，需要带着这个SessionID。这样，服务器端就可以根据这个SessionID找出当前请求的用户是谁了。

#### **session是怎么用的？session共享是干嘛用的？**

```java
SessionDemo1.java
//使用session共享数据
    
// 1 获取HttpSession 对象
HttpSession session = request.getSession();
//2 使用HttpSession 对象，来储存数据
session.setAttribute("msg","hello session ");
```

```java
SessionDemo2.java
//使用session获取数据

//1 获取 HttpSession 对象
HttpSession session = request.getSession();
//2 使用HttpSession 对象，来获取数据
Object msg = session.getAttribute("msg");
System.out.println(msg);

//删除session中存储的验证码
session.removeAttribute("checkCode_session");  //以防登录成功后，立即返回上一页面 验证不变的情况。

```

一个客户端 访问  百度服务器，发送过 两次请求：

第一次请求访问 SessionDemo1，然后 获取了Session

第一次请求访问 SessionDemo2， 然后 也获取了Session

一共获取了两次Session，获取的都是同一个Session，那么是同一个Session ，我们才可以在这里边共享数据。

**服务器如何确保在一次会话范围内，多次获取的Session对象是同一个？？**

回到Session的原理来分析 : Session是依赖于Cookie的，那Cookie是怎么实现的？请求头和响应头来实现的。

第一次获取Session ，没有Cookie，会在内存中创建一个新的Session对象，并且这个Session对象有一个唯一的ID , 即SessionID。假设 SessionID= 12345abc789 ，有了这个值之后，服务器做响应的时候，会去发送一个响应头：set-cookie ,  set-cookie的键值是 SessionID= 12345abc789 ，这个SessionID= 12345abc789 对应着一个Session对象。客户端浏览器收到了 这个 set-cookie 响应头，就把cookie的信息存储到浏览器的内部。

下一次，当客户端浏览器 再去访问 服务器

再去访问当前这个项目里边的其他资源的时候，会携带着这个cookie头。

**怎么携带着这个cookie头给服务器端呢？**

它是通过了一个请求头，那么请求里携带 set-cookie 头，通过这个set-cookie 头 带过去了这么一个Session ID之后，那么携带过去这个Session ID之后，服务器会自动的获取这么一个cookie信息，然后，他去根据这个cookie信息，去查找内存中有没有一个ID为这么长一串的 “SessionID= 12345abc789” 的Session对象，一找发现找到了，所以，getSession()方法就找到了这么一个Session对象，并且返回给这个Session标记：

```
获取session
HttpSession session = request.getSession();
```

所以这两次请求获取的Session就是同一个的，那么，服务器就是通过cookie的方式来确保Session在一次范围内多次获取是同一个的。

我们可以通过浏览器抓包验证一下这个问题，重新打开一次浏览器是一次新的会话。

![image-20220622180207341](F:\Typora\typora-user-images\image-20220622180207341.png)



**当客户端关闭后，服务器不关闭，两次获取session是否为同一个？**

默认情况下，不是。因为客户端关闭了，那么意味着会话结束了，会话结束了之后，他在获取时就没有对应的 cookie 头了。

**客户端不关闭后，服务器关闭后，两次获取的session是同一个吗？**



#### **Session 的失效时间？**

Session 一般都会配置一个过期时间，比如30分钟等等。Session过期之后，用户就需要重新登录。

如果把用户名、密码等重要隐私都存到客户端的 Cookie 中，还是有泄密风险。为了更安全，把机密信息保存到服务器上，这就是 Session。Session 是服务器上维护的客户档案，可以理解为服务器端数据库中有一张 user 表，里面存放了客户端的用户信息。SessionID 就是这张表的主键 ID。

Session 信息存到服务器，必然占用内存。用户多了以后，开销必然增大。为了提高效率，需要做分布式，做负载均衡。因为认证的信息保存在内存中，用户访问哪台服务器，下次还得访问相同这台服务器才能拿到授权信息，这就限制了负载均衡的能力。而且 SeesionID 存在 Cookie，还是有暴露的风险，比如 CSRF(Cross-Site Request Forgery，跨站请求伪造)。

**如何解决这些问题呢？基于 Token 令牌鉴权。**



## Token

Token 其实就是访问资源对凭证。一般是用户通过用户名和密码登录成功之后，服务器将登录凭证做数字签名，加密之后得到的字符串作为token。

**它在用户登录成功之后会返回给客户端，客户端主要以下几种存储方式：**

1、存储在localStorage中，每次调用接口的时候都把它当成一个字段传给后台

2、存储在cookie中，让它自动发送，不过缺点就是不能跨域

3、拿到之后存储在localStorage中，每次调用接口的时候放在HTTP请求头的Authorization字段里面。token 在客户端一般存放于localStorage、cookie、或sessionStorage中。

首先，Token 不需要再存储用户信息，节约了内存。其次，由于不存储信息，客户端访问不同的服务器也能进行鉴权，增强了扩展能力。然后，Token 可以采用不同的加密方式进行签名，提高了安全性。

Token 就是一段字符串，Token 传递的过程跟 Cookie 类似，只是传递对象变成了 Token。用户使用用户名、密码请求服务器后，**服务器就生成 Token**，在响应中返给客户端，客户端再次请求时附带上 Token，服务器就用这个 Token 进行认证鉴权。

Token 虽然很好的解决了 Session 的问题，但仍然不够完美。服务器在认证 Token 的时候，仍然需要去数据库查询认证信息做校验。为了不查库，直接认证，JWT 出现了。

XSS攻击：Cross-Site Scripting（跨站脚本攻击）简称XSS，是一种代码注入攻击。攻击者通过在目标网站注入恶意脚本，使之在用户的浏览器上运行。利用这些恶意脚本，攻击者可以获取用户的敏感信息如Cookie、SessionID等，进而危害数据安全。



## JWT

#### JWT 是什么？

JWT 的英文全称是 JSON Web Token。JWT 把所有信息都存在自己身上了，包括用户名密码、加密信息等，且以 JSON 对象存储的。

主要是在网络应用中, 传递一些小批量的安全数据时，使用这个token的特点，是紧凑并且安全。JWT 的结构，主要是分三部分，第一部分就是Header 头部，第二部分就是 payload 负载，第三部分是 signature签名，且结构看起来就是这样的，就是这种结构：

JWT 长相是 xxxxx.yyyyy.zzzzz，极具艺术感。包括三部分内容：

**1、Header** 

Header 部分 是一个JSON对象，描述JWT的元数据。包括 token 类型和加密算法(HMAC SHA256 RSA)。

```
{  "alg": "HS256",  "typ": "JWT"}

alg属性表示签名的算法，默认是 HMAC SHA256 (缩写为：HS256)；

typ属性表示这个令牌(token)的类型为JWT，最后将上面的JSON对象使用 Base64URL的算法转成字符串。

base64在线调试
```

**2、Payload 传入内容**

Payload部分也是一个JSON对象，用来存放实际需要传递的数据，官方提供了7个字段，如下：

```
iss(issuer): 签发人
exp (expiration time): 过期时间
sub (subject): 主题
aud (audience): 受众
nbf (Not Before): 生效时间
iat (Issued At): 签发时间
jti (JWT ID): 编号
```

payload中文含义是载荷，它可以理解为存放有效信息的地方。这些有效信息一般包含如下三个部分：

```
标准中注册的声明：(如上就是官方提供的7个字段)。
公共的声明：公共的声明可以添加任何的信息，一般添加用户的相关信息或其他业务需要的必要信息，但是不建议添加敏感信息，因为该部分在客户单可解密。
私有的声明：私有声明是提供者和消费者所共同定义的声明，一般不建议存放敏感信息，因为base64是对称解密的，该部分信息可以理解为明文信息。
```

简单的 payload 如下结构：

```
{  "sub": "1234567890",  "name": "John Doe",  "admin": true}
```

**3、Signature 签名**

Signature 是对前面两部分的签名，防止数据被篡改。

签名原理：Header和payload对应的json结构进行base64 编码之后得到的两个串用英文句点号拼接起来的，然后根据header里面的alg指定的前面算法(默认算法是 HMAC SHA256)生成出来的。

如上header部分使用的是 HS256(即HMAC和SHA256) , HMAC是用于生成摘要的，SHA256是用于对摘要进行数字签名的。因此使用HMACSHA256实现signature实现的算法如下：

Signature 签名，把 header 和 payload 用 base64 编码后"."拼接，加盐 secret(服务器私钥)。

```
HMACSHA256(base64UrlEncode(header) + "." + base64UrlEncode(payload), secret);
```

如上是 Signature 签名算法，最后一个 secret 是加密的密钥的含义。

通过如上的用法就可以拿到JWT了。

JWT在线调试工具： https://jwt.io/

![image-20220621112757769](F:\Typora\typora-user-images\image-20220621112757769.png)

**它的主要应用场景，就是有两个应用场景：**

第一个应用场景：就是身份认证，在身份认证的功能实现上，传统的方法，是在服务器端存储一个session，给客户端返回一个cookie，把SessionID 存储在cookie里。

使用 JWT，就是当用户登录系统之后，后台会返回一个 JWT 给到用户浏览器端，用户只需要本地保存这个token即可。

如果是web应用，通常使用cookie或者local storage来存储。

如果是APP，就使用APP自己的存储机制来存储。用户请求后台资源的时候，每次都要带上这个token。后台会对这个token进行验证。

第二个应用场景：就是信息交换，由于它的信息，是经过签名的，可以确保发送的信息是真实的。所以用这种方法来进行 小批量数据 的交换，但是这种应用场景比较少。



#### 跨域问题是怎么产生的？

跨域请求，就是 当前发起请求的域 与 该请求指向的资源所在的域不一样，就是跨域请求。

跨域，就包括 协议、 域名、端口，只要有一项不同，就是跨域。简单点儿说，那就是浏览器的地址栏里边的协议、域名、端口和你网页中 JS请求的接口的协议、域名、端口，任意一项不同就是跨域。当然，你页面如果用了，这个判断方法就不适用。

**面试中最常见的错误？**

第一就是说不清楚，跨域到底是谁和谁的域不同。

第二种错误，就是说只要服务器不同，就是跨域，或者说，这个IP地址不同，就是跨越，这都是不准确的。

第三种错误就是只说了域名，忽略了协议和端口。

第四种，就是不知道怎么解决跨域问题。

**怎么解决跨域问题？**

通过 JWT，JSON Web Token（缩写 JWT）是目前最流行的跨域认证解决方案

#### 一、跨域认证的问题

互联网服务离不开用户认证。一般流程是下面这样。

1、用户向服务器发送用户名和密码。

2、服务器验证通过后，在当前对话（session）里面保存相关数据，比如用户角色、登录时间等等。

3、服务器向用户返回一个 session_id，写入用户的 Cookie。

4、用户随后的每一次请求，都会通过 Cookie，将 session_id 传回服务器。

5、服务器收到 session_id，找到前期保存的数据，由此得知用户的身份。

这种模式的问题在于，扩展性（scaling）不好。单机当然没有问题，如果是服务器集群，或者是跨域的服务导向架构，就要求 session 数据共享，每台服务器都能够读取 session。

举例来说，A 网站和 B 网站是同一家公司的关联服务。现在要求，用户只要在其中一个网站登录，再访问另一个网站就会自动登录，请问怎么实现？

一种解决方案是 session 数据持久化，写入数据库或别的持久层。各种服务收到请求后，都向持久层请求数据。这种方案的优点是架构清晰，缺点是工程量比较大。另外，持久层万一挂了，就会单点失败。

另一种方案是服务器索性不保存 session 数据了，所有数据都保存在客户端，每次请求都发回服务器。JWT 就是这种方案的一个代表。

#### 二、JWT 的原理

JWT 的原理是，服务器认证以后，生成一个 JSON 对象，发回给用户，就像下面这样。

{
“姓名”: “张三”,
“角色”: “管理员”,
“到期时间”: “2018年7月1日0点0分”
}
以后，用户与服务端通信的时候，都要发回这个 JSON 对象。服务器完全只靠这个对象认定用户身份。为了防止用户篡改数据，服务器在生成这个对象的时候，会加上签名（详见后文）。

服务器就不保存任何 session 数据了，也就是说，服务器变成无状态了，从而比较容易实现扩展

#### 三、JWT 的数据结构

实际的 JWT 大概就像下面这样。

它是一个很长的字符串，中间用点（.）分隔成三个部分。注意，JWT 内部是没有换行的，这里只是为了便于展示，将它写成了几行。

JWT 的三个部分依次如下。

Header（头部）
Payload（负载）
Signature（签名）
写成一行，就是下面的样子。

Header.Payload.Signature

下面依次介绍这三个部分。

3.1 Header
Header 部分是一个 JSON 对象，描述 JWT 的元数据，通常是下面的样子。

```
{
“alg”: “HS256”,
“typ”: “JWT”
}
```

上面代码中，alg属性表示签名的算法（algorithm），默认是 HMAC SHA256（写成 HS256）；typ属性表示这个令牌（token）的类型（type），JWT 令牌统一写为JWT。

最后，将上面的 JSON 对象使用 Base64URL 算法（详见后文）转成字符串。

3.2 Payload
Payload 部分也是一个 JSON 对象，用来存放实际需要传递的数据。JWT 规定了7个官方字段，供选用。

```
iss (issuer)：签发人
exp (expiration time)：过期时间
sub (subject)：主题
aud (audience)：受众
nbf (Not Before)：生效时间
iat (Issued At)：签发时间
jti (JWT ID)：编号
```

除了官方字段，你还可以在这个部分定义私有字段，下面就是一个例子。

```
{
“sub”: “1234567890”,
“name”: “John Doe”,
“admin”: true
}
```

注意，JWT 默认是不[加密](https://so.csdn.net/so/search?q=加密&spm=1001.2101.3001.7020)的，任何人都可以读到，所以不要把秘密信息放在这个部分。

这个 JSON 对象也要使用 Base64URL 算法转成字符串。

3.3 Signature
Signature 部分是对前两部分的签名，防止数据篡改。

首先，需要指定一个密钥（secret）。这个密钥只有服务器才知道，不能泄露给用户。然后，使用 Header 里面指定的签名算法（默认是 HMAC SHA256），按照下面的公式产生签名。

```
HMACSHA256(
base64UrlEncode(header) + “.” +
base64UrlEncode(payload),
secret)
```

算出签名以后，把 Header、Payload、Signature 三个部分拼成一个字符串，每个部分之间用"点"（.）分隔，就可以返回给用户。

3.4 Base64URL
前面提到，Header 和 Payload 串型化的算法是 Base64URL。这个算法跟 Base64 算法基本类似，但有一些小的不同。

JWT 作为一个令牌（token），有些场合可能会放到 URL（比如 [api.example.com/?token=xxx）。Base64](http://api.example.com/?token=xxx）。Base64) 有三个字符+、/和=，在 URL 里面有特殊含义，所以要被替换掉：=被省略、+替换成-，/替换成_ 。这就是 Base64URL 算法。

#### 四、JWT 的使用方式

客户端收到服务器返回的 JWT，可以储存在 Cookie 里面，也可以储存在 localStorage。

此后，客户端每次与服务器通信，都要带上这个 JWT。你可以把它放在 Cookie 里面自动发送，但是这样不能跨域，所以更好的做法是放在 HTTP 请求的头信息Authorization字段里面。

Authorization: Bearer
另一种做法是，跨域的时候，JWT 就放在 POST 请求的数据体里面。

#### 五、JWT 的几个特点

（1）JWT 默认是不加密，但也是可以加密的。生成原始 Token 以后，可以用密钥再加密一次。

（2）JWT 不加密的情况下，不能将秘密数据写入 JWT。

（3）JWT 不仅可以**用于认证**，也可以**用于交换信息**。有效使用 JWT，可以降低服务器查询数据库的次数。

（4）JWT 的最大缺点是，由于服务器不保存 session 状态，因此无法在使用过程中废止某个 token，或者更改 token 的权限。也就是说，一旦 JWT 签发了，在到期之前就会始终有效，除非服务器部署额外的逻辑。

（5）JWT 本身包含了认证信息，一旦泄露，任何人都可以获得该令牌的所有权限。为了减少盗用，JWT 的有效期应该设置得比较短。对于一些比较重要的权限，使用时应该再次对用户进行认证。

（6）为了减少盗用，JWT 不应该使用 HTTP 协议明码传输，要使用 HTTPS 协议传输。



####  JWT 详情介绍 在 npmjs.com 网站  https://www.npmjs.com/package/jsonwebtoken

express-basic 项目 根目录下 下载安装

 yarn add jsonwebtoken -S

#### JWT 怎么用 ？

**JWT 有两种使用方式：**

#### （1）对称加密   HMAC SHA256

是指在加密和解密时使用同一密钥的加密方式  ，但是不安全

  对称加密    **HMAC SHA256** 

eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImFkbWluIiwiaWF0IjoxNjU0NzY2NDAyfQ.ROP0-JpF3RIUB-376cAyXcHF8jA1SMp9AfQy58zE1po

对称加密的token是怎么被验证的？

```javascript
const token =(req,res,next)=>{
    //对称加密
    const tk = jwt.sign({username:'admin'},'love you')
	//验证
    const decoded = jwt.verify(tk,'love you')
    
    res.send(decoded)
}

exports.token = token;
```

在postman中 输入 http://localhost:8080/api/token 访问



#### （2）非对称加密     RSA SHA256 

首先得有两个秘钥，需要生成两个秘钥，

express-basic 项目 根目录下新建 keys 目录，

进入 keys 目录    cd  keys ，

输入 **openssl**  进入

OpenSSL> 

再输入  **genrsa -out rsa_private_key.pem 2048**

这样就在 keys 目录下生成了 私钥 文件 ：rsa_private_key.pem

再根据 私钥 生成 公钥  ，请输入 

**rsa -in rsa_private_key.pem -pubout -out rsa_public_key.pem**

这样就在 keys 目录下生成了 公钥 文件  rsa_public_key.pem

由于私钥文件 和 公钥文件 不一样，所以不对称，所以又称之为 非对称加密。

就用这 私钥文件 和 公钥文件生产token，私钥文件就用文件读取的功能读取文件。

token是通过私钥生成的。

```javascript
var path = require ( 'path' ); 
var fs = require ( 'fs' ); 
var jwt = require('jsonwebtoken');

//非对称加密 通过私钥加密，验证通过公钥
    const privateKey = fs.readFileSync(path.join(__dirname,'../keys/rsa_private_key.pem'));

    const tk = jwt.sign({username:'admin'},privateKey);
    res.send(tk);
```

 非对称加密   **RSA SHA256 **  

eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImFkbWluIiwiaWF0IjoxNjU0NzcwMTczfQ.3n8hkul4qOiNyfkyM-Z3TzRJbYK4NP7ajnUID-ThHcY

非对称加密的token是怎么被验证的？

```javascript
 //非对称加密
    const privateKey = fs.readFileSync(path.join(__dirname,'../keys/rsa_private_key.pem'));
    //加密
    const tk = jwt.sign({username:'admin'},privateKey,{ algorithm: 'RS256'})

    const publicKey =  fs.readFileSync(path.join(__dirname,'../keys/rsa_public_key.pem'));
//在代码里面验证
    const result = jwt.verify(tk,publicKey);
    res.send(result);
```

在postman中 输入 http://localhost:8080/api/token 访问

git 中登录 GitHub，需要在本地生产一个公钥，再生成一个 私钥 文件 ，然后在本地通过  私钥 生成一个 token，然后把token发送给GitHub，在 GitHub的后台，再把公钥传给它，拿到公钥就可以做验证了，就不需要你再通过输入用户 和密码登录了

https://www.npmjs.com/   



https://jwt.io/   验证jwt地方
