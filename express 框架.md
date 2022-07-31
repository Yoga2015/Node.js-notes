### express 框架 

##### 什么是express？

Express 是 一个web应用框架，https://www.expressjs.com.cn/。

Express 是一个基于 Node.js 平台的极简、灵活的 web 应用开发框架，它提供一系列强大的特性，帮助你创建各种 Web 和 移动设备应用。可以快速地搭建一个完整功能的网站。

express是Node.js的一个框架，它并不是直接的，对应 Node.js 做了一个深度的封装，而是你原有的Node.js的一些方法，它可以直接用，express可以简化我们的操作。比方说它通过一个中间件的模式，把好多操作都封装起来。Express本身，给我们提供了很多的一些现成的中间件，比方说：路由中间件，静态目录中间件，错误中间件，以及内置的中间件等等，当然我们通过这个中间件的模式，我们可以使用很多第三方的中间件。

##### **为什么要用express？**

因为使用 express 创建的服务器 比 用纯node.js 要方便 ，如下：

##### express运行原理

**底层：http模块**

Express框架建立在node.js内置的http模块上。

http模块生成服务器的原始代码如下 :

```javascript
var http = require("http");

var app = http.createServer(function (request, response){
  response.writeHead(200, {"Content-Type": "text/plain"});
  response.end("Hello world!");
});

app.listen(8080,()=>{
    console.log("localhost:8080 服务器已启动了")
});
```

上面代码的关键是http模块的createServer方法，表示**生成一个HTTP服务器实例**。该方法接收一个回调函数，该回调函数的参数，分别为代表 HTTP请求 和 HTTP响应 的 request对象 和 response对象。

Express框架的核心是**对http模块的再包装**。上面的代码用Express改写如下 :

```javascript
const express = require("express"); //引入模块，并赋给变量express
const app = express(); //生成express实例，赋给变量app

app.use('/', function (req, res) {
  res.writeHead(200, {"Content-Type": "text/plain"});  
  res.send('Hello world!');
});

app.listen(8080,()=>{
    console.log("localhost:8080 服务器已启动了")
})
```

比较两段代码，可以看到它们非常接近。原来是用`http.createServer`方法新建一个app实例，现在则是用Express的构造方法，生成一个Express实例。两者的回调函数都是相同的。

##### 什么是中间件

简单说，中间件（middleware）就是处理HTTP请求的函数，它最大的特点就是，一个中间件处理完，再传递给下一个中间件。App实例在运行过程中，会调用一系列的中间件。回调函数又被称为 中间件。

每个中间件可以从App实例，接收三个参数，依次为request对象（代表HTTP请求）、response对象（代表HTTP回应），**next回调函数（代表下一个中间件）**。每个中间件都可以对HTTP请求（request对象）进行加工，并且决定是否调用next方法，将request对象再传给下一个中间件。

抛出错误以后，后面的中间件将不再执行，直到发现一个错误处理函数为止。



**app.use方法**

现在开始开发功能，第一步是开始请求一个路由，路由该怎么请求？（路由就是路径）

```javascript
//1、这里用 use去挂载路径，路径解析是从上到下的，如果第一个use()的请求路径匹配了，第二个的use()的内容就没机会显示在页面上了
app.use("/",(req,res，next)=>{  //use是用来注册中间件的，它返回一个函数。
    //可以返回一个值给前端 
    res.send("hello");
    next();
})

app.use("/api",(req,res)=>{  
    res.send("world");
})


//2、回调函数又被称为 中间件 。next 回调函数（代表下一个中间件） 中间件是可以挂载多个路由的（路径）
//中间栈：两个中间件就可以形成一个 中间栈
app.use("/",(req,res,next)=>{
    //可以返回一个值给前端 
    console.log(0)
    next()
},(req,res,next)=>{
    console.log(1);
    next()
},(req,res)=>{
    console.log(2);
},(req,res)=>{
    console.log(3);
})
//在回调函数中加了next，并调用next()，第二个use()的内容就可以显示,由于第二个use() 中的请求路径已匹配上，所以第三个use()的内容就无法显示


//3、可以给数组封装一下   middlewares 中间件，也是函数
const middlewares = [
    (req,res,next)=>{
    //可以返回一个值给前端 
    console.log(0)
    next()
},(req,res,next)=>{
    console.log(1);
    next()
},(req,res)=>{
    console.log(2);
}]

app.use('/',middlewares);


//4、下面代码使用app.use方法，注册了三个中间件。收到HTTP请求后，先调用第一个中间件，在控制台输出 hello，然后通过next方法，将执行权传给第二个中间件，在控制台输出 world 。由于第二个中间件没有调用next方法，所以request对象就不再向后传递了。
app.use("/",(req,res，next)=>{
    //可以返回一个值给控制台 
    console.log("hello");
    next();
})

app.use("/api",(req,res)=>{  
    console.log("world");
})
app.use("/index",(req,res)=>{  
    console.log("world");
})


//5、不写路径的中间件
app.use(()=>{
    console.log(0)
})
//总结：用use() 去挂载的中间件是一定会run 的，和路径没关系
```



##### **新建 express 框架**

首先新建一个项目目录 express-basic，然后**进入此目录**并**将其作为当前工作目录**。

```javascript
$ mkdir express-basic     创建 express-basic 文件夹
$ cd express-basic       进入 express-basic 文件夹
```

接着使用 yarn init -y 命令为 express-basic 项目 创建一个 package.json 文件。

然后安装 express  

```
yarn add express -S
```

在express-basic 项目目录下，新建一个服务器启动文件，叫做server.js。

```javascript
const express = require("express");
const app = express();

const router = require("./router/index.js");

app.use("/",router);

app.listen(8080,()=>{
    console.log("localhost:8080 服务器已启动了")
})
```

实际应用中，可能有多个路由记录，所以最好就把路由放到一个单独的文件中，在express-basic 项目根目录下新建一个router子目录，并在其下 新建 index.js ：

```javascript
const express = require("express");    //要从express 中拿出路由来

//路由中间件
const router = express.Router();
console.log(router)

module.exports = router;   //想要run 一下必须暴露出去
```

nodemon server.js    运行上面的启动脚本，启动脚本server.js的app.use ( )方法，会在本机的8080端口启动一个网站，控制台显示router。

![image-20220422162612620](F:\Typora\typora-user-images\image-20220422162612620.png)

从输出结果看出 router 就是一个 函数 ，是个函数怎么办呢？



**1、get 请求**：获取数据

```javascript
const express =require("express");    //要从express 中拿出路由来

//路由中间件
const router = express.Router();
// console.log(router)

//我们定义了一个路由处理程序 / ，当我们点击我们的网站主页时会调用它。
router.get('/',(req,res,next)=>{ 
    res.send("hello")
})
router.get('/index',(req,res,next)=>{
    res.send("index page")
})
//现在是访问 http://localhost:8080/ 可以匹配
//访问 http://localhost:8080/index  也可以匹配

module.exports = router;   //想要run 一下必须暴露出去
```

现在还有个问题，如何拿到前端 router 数据，如何实现数据获取呢？

前端给我的请求是有get，除了get方法以外，Express还提供post、put、delete方法，即HTTP动词都是Express的方法。最常见的get请求是不是也有往后端送数据的需求啊，get请求怎么给后端送数据？是在地址栏里边写个问号，ID等于2 这样的：
http://localhost:8080/index?id=2

![image-20220505222335315](F:\Typora\typora-user-images\image-20220505222335315.png)

 如果是用纯node.js就麻烦了，纯node.js，你需要自己去判断,是否匹配！现在看 id=2  怎么获取？得从req 里想办法获取id=2 ，现在的req是给express增强了的，所以可以在路由中间件里访问query，访问query就可以拿到query对象。

```javascript
router.get('/index',(req,res,next)=>{
    const query = req.query;
    res.send(query)
})
```

浏览器端访问 http://localhost:8080/index?id=2     显示一个json  数据类型。

**2、post请求**  是添加数据

```javascript
// post 请求的参数在 body 中
router.post('/index',(req,res,next)=>{
    const data = req.body;
    res.send(data)
})
```

由于post的请求参数不在 url 中，所以需要借用 postman 来发起post请求。

现在express  环境缺少一个第三方中间件，第一个中间件是自己定义的，第二个中间件是router 路由中间件，第三个是第三方中间件：

去 https://www.npmjs.com/ 找  body-parser  中间件，body-parser中间件用来解析http请求体，是express默认使用的中间件之一。首先yarn 安装 body-parser ：

```javascript
 yarn add body-parser -S
```

server.js 中引入body-parser

（1）、**解析form表单提交的数据**

```javascript
const express = require("express");
const app = express();

const bodyParser = require('body-parser')

const router = require("./router/index.js");

// bodyParser.urlencoded 是用来解析我们通常的form表单提交的数据，也就是请求头中包含这样的信息 ：
// Content-Type: application/x-www-form-urlencoded
app.use(bodyParser.urlencoded({extended:false}))

app.use("/",router);

app.listen(8080,()=>{
    console.log("localhost:8080 服务器已启动了")
})
```

nodemon server.js    运行，postman 来发起post请求。

![image-20220423181028390](F:\Typora\typora-user-images\image-20220423181028390.png)

![image-20220423181316105](F:\Typora\typora-user-images\image-20220423181316105.png)

服务器端啥也没干，就拿到了下图数据，直接返回给前端

![image-20220423182433737](F:\Typora\typora-user-images\image-20220423182433737.png)

bodyParser.urlencoded模块用于解析req.body的数据，解析成功后覆盖原来的req.body。如果解析失败则为{}。该模块有一个属性extended：extended选项允许配置使用**querystring(false)或qs(true)**来解析数据，默认值是true，但这已经是不被赞成的了。

querystring就是nodejs内建的对象之一，用来字符串化对象或解析字符串。

```
注意：前端和后端合作沟通，讨论前端发什么格式的数据给后端，后端是要表单数据还是字符串。
```

**（2）、解析请求传来的 json数据**

```javascript
// 1、bodyParser.urlencoded 是用来解析form表单提交的数据，也就是请求头中包含这样的信息
app.use(bodyParser.urlencoded({extended:false}))
// 2、bodyParser.json是用来解析json数据格式的
app.use(bodyParser.json())
```

![image-20220424100015126](F:\Typora\typora-user-images\image-20220424100015126.png)

**3、put请求**   是修改数据，是覆盖式修改

put请求 是在x-www-form-urlencoded 里传参的，put请求 和 post 请求都可以从req.body拿数据 ，拿 json 数据、form表单数据

![image-20220424102534803](F:\Typora\typora-user-images\image-20220424102534803.png)

index.js 中添加，就可以发请求，得到结果：put response

```javascript
//put 请求的参数也在body的x-www-form-urlencoded 里传参的
//put 请求是修改数据，是覆盖式修改
router.put('/index',(req,res,next)=>{
    const data  = req.body;
    console.log(data);
    res.send("put response")
})
```

**4、patch 请求  、delete 请求    都不传参看看**：

```javascript
//patch 请求 现在不传参数  ，patch请求是修改数据，是增量修改
router.patch('/index',(req,res,next)=>{ 
    res.send("patch response")
})

//delete 请求 现在不传参数 ，delete请求是删除数据
router.delete('/index',(req,res,next)=>{  
    res.send("delete response")
})
```

这样写请求的话，前端就明确告诉后端，前端要干什么了。用postman工具来反复地切换请求，那前端的代码该怎么写？

```
get  请求 是 获取数据 （查询数据）
post 请求 是 添加数据 ，是增加的意思
put  请求 是 修改数据，是覆盖式修改
patch 请求 是 修改数据，是增量修改
delete 请求 是 删除数据
```

post 请求可以添加数据、修改数据、删除数据，post 请求可以做很多事情，都用post 请求的话，后端就没有确切的语意了。

这些请求方式只是告诉后端  信号而已，真正怎么处理是中间件来处理的。前后交互的时候有什么优化吗，我们会规划地使用各种适合的请求方法

前后端交互的时候有过什么样的优化吗？我们规划了一下这个提交方法，把put、patch以及delete都做了些区分，这样的话，其实上对于什么小量更改数据非常有效。减少我们后来回传数据的一个量。

现在呢，我们又学会了什么。如何的去拿到我们前端给我的数据，数据 包括 数据本身和 可你要操作数据的信息。

报了404说明什么问题？说明你前端请求这个地址，前端给我后端送的数据是错误的，怎么错了？路径可能错了，还有可能是方法错了，怎么样才能对？切换不同的请求方法看看是不是可以拿到数据。或者直接用all方法

```javascript
//不管是什么请求都能接收
//那怎么去判断前端的请求？得在req、res中试图拿到前端到底给我get请求还是post请求
router.all('/index',(req,res,next)=>{
    res.send("hello");
})
```

controller 是中间件。

服务器server.js 不变：

```javascript
const express = require("express");
const app = express();

const bodyParser = require('body-parser')

const router = require("./router/index.js");

// 1、bodyParser.urlencoded 是用来解析form表单提交的数据，也就是请求头中包含这样的信息
app.use(bodyParser.urlencoded({extended:false}))
// 2、bodyParser.json是用来解析json数据格式的
app.use(bodyParser.json())

app.use("/",router);

app.listen(8080,()=>{
    console.log("localhost:8080 服务器已启动了")
})
```

在express-basic 项目根目录下新建一个controller 子目录，并在其下 新建 index.js ：

```javascript
const list = (req,res,next)=>{
    res.send("controller 中间件")
}

//这也是暴露语句
exports.list = list
```

router 目录下 的 index.js ：

```javascript
const express =require("express");    //要从express 中拿出路由来

//路由中间件
const router = express.Router();

const {list} = require("../controller/index.js")

router.get('/', list)

module.exports = router;   //想要run 一下必须暴露出去
```

然后访问 http://localhost:8080/   显示：controller 中间件

以上是 express  web框架 完全手工搭建，一切地一切都是需要自己去构建。自己去引入了express，自己去监听一个端口，启动一个server， 自己去处理从浏览器端或者从前端发送过来的数据，处理的数据包括 表单 数据 和 json字符串，另外还定义了路由，路由的话呢，是通过express自带的路由对象  express.router();   在 router 里去定义路由，也有定义过get、post 、put、patch、delete 请求方法。

而且还有一点就是路由中间件的方法router.get（）中，的第二个参数是回调参数，它会在路径匹配的时候呢，就执行这个函数。其二，这个路径一旦匹配以后呢，这个函数会执行，执行完了以后，哪怕你不进行 请求 json 也进行返回，不会把访问权交给下一个路由，因为下一个路由没有匹配，所以我们现在就了解了整个的这个路由的一个执行流程。

到这里就讲了  R   请求 和  C  控制器，下面开始讲  View  视图 和 Model 模型：数据库

 通过express 创建的 服务器， 主要作用是  根据前端的请求，匹配路由。



有静态资源目录就不需要：

```javascript
app.get('/',(req,res)=>{
    res.sendFile(__dirname + '/index.html')  //有静态资源目录就不需要这行了
})
```

```javascript
htpp服务器 监听3000 端口号 并暴露它，供外界连接
http.listen(3000,()=>{
    console.log("localhost:3000 服务器已启动了")
})


app.listen(3000,()=>{
    console.log("localhost:3000 服务器已启动了")
})
```

