## Node.js 模块化系统 

​		实际中 **开发复杂 Web应用** 的时候，**通常需要把各个功能进行拆分、封装到不同的文件** 并 在需要的时候引用该文件，即进行代码的模块化管理。	

#### 模块化的理解

##### 什么是模块化

**概念**：将一个复杂的程序依据一定的规则（规范）封装成几个块（文件），并组合在一起。

模块的内部数据、实现是私有的, 只是向外部暴露一些接口(方法) 与外部其它模块通信。

最早的时候，我们会把所有的代码都写在一个js文件里，那么，耦合性会很高（关联性强），不利于维护；而且会造成全局污染，很容易命名冲突。

##### 模块化有什么作用？

- 避免命名冲突，减少命名空间污染
- 降低耦合性；更好地分离、按需加载
- **高复用性**：代码方便重用，别人开发的模块直接拿过来就可以使用，不需要重复开发类似的功能。
- **高可维护性**：软件的声明周期中最长的阶段其实并不是开发阶段，而是维护阶段，需求变更比较频繁。使用模块化的开发，方式更容易维护。
- 部署方便

##### 引入模块化规范的原因？

假设我们引入模块化，首先可能会想到的思路是：在一个文件中引入多个js文件。如下：

```text
<body>
    <script src="a.js"></script>
    <script src="b.js"></script>
    <script src="util/c.js"></script>
    <script src="util/d.js"></script>
    <script src="util/e.js"></script>
</body>
```

但是这样做会带来很多问题：

- 请求过多：引入十个js文件，就有十次http请求。
- 依赖模糊：不同的js文件可能会相互依赖，如果改其中的一个文件，另外一个文件可能会报错。

以上两点，最终导致：**难以维护**。

于是，这就引入了模块化规范。

**JavaScript 语言**

JavaScript 语言是没有模块化的概念的，直到 Node.js 的诞生，才把 JavaScript 语言带到服务端后端，面对 **文件系统、网络、操作系统**等等复杂的业务场景，模块化就变得不可或缺。

​	几乎所有的编程语言**都有自己的模块组织方式**，比如Java中的包、C#中的程序集，而**Node.js采用CommonJS模块规范**。Node.js 从诞生到超火，离不开它成熟的模块化实现，Node.js 的模块化是在 CommonJS模块规范的基础上实现的。

**什么是CommonJS模块规范？**

`CommonJS`是社区提出的一种`JavaScript`模块化规范，可以说是`JS`模块化历程中最重要的一块里程碑，它构造了一个美好的愿景——`JS`能够在任何地方运行。

**`CommonJS`的模块组成？**

`CommonJS`的模块主要由**模块引用**、**模块定义**和**模块标识**三部分组成。

**CommonJS模块的特点**

- 所有代码都运行在模块作用域，不会污染全局作用域。

- 模块可以多次加载，但是只会在第一次加载时运行一次，然后运行结果就被缓存了，以后再加载，就直接读取缓存结果。要想让模块再次运行，必须清除缓存。

- 模块加载的顺序，按照其在代码中出现的顺序。

  

**Node 应用由模块组成，采用 CommonJS 模块规范。**

每个文件都是一个独立的模块，有自己的作用域。这个文件可能是JavaScript 代码、JSON 或者编译过的C/C++ 扩展。

在一个文件里面定义的变量、函数、类，都是私有的，对其他文件不可见。

```javascript
// example.js
var x = 5;
var addX = function (value) {
  return value + x;
};
```

上面代码中，变量`x`和函数`addX`，是当前文件`example.js`私有的，其他文件不可见。

如果想在多个文件分享变量、函数，必须暴露这个模块。

CommonJS规范规定，每个模块内部，`module`变量代表当前模块。这个变量是一个对象，它的`exports`属性（即`module.exports`）是对外的接口。加载某个模块，其实是加载该模块的`module.exports`属性。

```javascript
var x = 5;
var addX = function (value) {
  return value + x;
};
module.exports.x = x;
module.exports.addX = addX;
```

上面代码通过`module.exports`输出变量`x`和函数`addX`。

`require`命令用于加载模块。

```javascript
var example = require('./example.js');

console.log(example.x); // 5
console.log(example.addX(1)); // 6
```

#### **require 命令**

`require`命令的基本功能是，读入并执行一个JavaScript文件，然后返回该模块的exports对象。如果没有发现指定模块，会报错。

`require`命令用于加载文件，后缀名默认为`.js`。

```javascript
var foo = require('foo');
//  等同于
var foo = require('foo.js');
```

根据参数的不同格式，`require`命令去不同路径寻找模块文件。

（1）如果参数字符串以“/”开头，则表示加载的是一个位于绝对路径的模块文件。比如，`require('/home/marco/foo.js')`将加载`/home/marco/foo.js`。

（2）如果参数字符串以“./”开头，则表示加载的是一个位于相对路径（跟当前执行脚本的位置相比）的模块文件。比如，`require('./circle')`将加载当前脚本同一目录的`circle.js`。

（3）如果参数字符串不以“./“或”/“开头，则表示加载的是一个默认提供的核心模块（位于Node的系统安装目录中），或者一个位于各级node_modules目录的已安装模块（全局安装或局部安装）。

举例来说，脚本`/home/user/projects/foo.js`执行了`require('bar.js')`命令，Node会依次搜索以下文件。

```javascript
- /usr/local/lib/node/bar.js
- /home/user/projects/node_modules/bar.js
- /home/user/node_modules/bar.js
- /home/node_modules/bar.js
- /node_modules/bar.js
```

这样设计的目的是，使得不同的模块可以将所依赖的模块本地化。

（4）如果参数字符串不以“./“或”/“开头，而且是一个路径，比如`require('example-module/path/to/file')`，则将先找到`example-module`的位置，然后再以它为参数，找到后续路径。

（5）如果指定的模块文件没有发现，Node会尝试为文件名添加`.js`、`.json`、`.node`后，再去搜索。`.js`件会以文本格式的JavaScript脚本文件解析，`.json`文件会以JSON格式的文本文件解析，`.node`文件会以编译后的二进制文件解析。

（6）如果想得到`require`命令加载的确切文件名，使用`require.resolve()`方法。

**目录的加载规则**

通常，我们会把相关的文件会放在一个目录里面，便于组织。这时，最好为该目录设置一个入口文件，让`require`方法可以通过这个入口文件，加载整个目录。

在目录中放置一个`package.json`文件，并且将入口文件写入`main`字段。下面是一个例子。

```javascript
// package.json
{ "name" : "some-library",
  "main" : "./lib/some-library.js" }
```

`require`发现参数字符串指向一个目录以后，会自动查看该目录的`package.json`文件，然后加载`main`字段指定的入口文件。如果`package.json`文件没有`main`字段，或者根本就没有`package.json`文件，则会加载该目录下的`index.js`文件或`index.node`文件。

#### **module对象**

每个模块内部，都有一个`module`对象，代表当前模块。

Node内部提供一个`Module`构建函数。所有模块都是`Module`的实例。

```javascript
function Module(id, parent) {
  this.id = id;
  this.exports = {};
  this.parent = parent;
  // ...
```

**module对象的属性**

```javascript
module.id 模块的识别符，通常是带有绝对路径的模块文件名。
module.filename 模块的文件名，带有绝对路径。
module.loaded 返回一个布尔值，表示模块是否已经完成加载。
module.parent 返回一个对象，表示调用该模块的模块。
module.children 返回一个数组，表示该模块要用到的其他模块。
module.exports 表示模块对外输出的值。
```

##### **module.exports属性**

CommonJS规范规定，每个模块内部，module变量代表当前模块。这个变量是一个对象，它的exports属性（即module.exports）是对外的接口。加载某个模块，其实是加载该模块的module.exports属性。

```javascript
module.exports = {};

1.module.exports
例:example.js
var x = 5;
module.exports.x = x;
module.exports.Name="我是电脑"；
module.exports.Say=function(){
  console.log("我可以干任何事情")；  

require加载模块
var example = require("./example.js");

example.x      //这个值是 5
example.Name      //这个值是 "我是电脑"
example.Say()     //这个是直接调用Say方法，打印出来 "我可以干任何事情"

```

exports 与 module.exports
为了方便，Node为每个模块提供一个exports变量，指向module.exports。这等同在每个模块头部，有一行这样的命令。

```javascript
var exports = module.exports;
```

区别：

require导出的内容是module.exports的指向的内存块内容，并不是exports的。简而言之，区分他们之间的区别就是 **exports 只是 module.exports的引用**，辅助后者添加内容用的。为了避免糊涂，尽量都用 module.exports 导出，然后用require导入。

```javascript
module.exports可以直接导出一个匿名函数或者一个值
module.exports=function(){
  var a="Hello World"  
  return   a;
}
但是exports是不可以的，因为这样等于切断了exports与module.exports的联系。
exports=function(){           //这样写法是错误的
  var a="Hello World"      
  return   a;        
} 
```

##### **exports 与 module.exports 区别：**

**js文件启动时**

在一个 node 执行一个文件时，会给这个文件内生成一个 exports 和 module 对象，
而module又有一个 exports 属性。他们之间的关系如下图，都指向一块{}内存区域。

exports = module.exports = {};

看一张图理解这里更清楚：


![image](https://user-images.githubusercontent.com/94358089/178152306-ef49cd69-33fb-4278-a426-8c4fce4b0fda.png)

如果要对外暴露属性或方法，就用 exports 就行，要暴露对象(类似class，包含了很多属性和方法)，就用 module.exports。

#### **模块引用**

使用`require()`来引用一个模块.

在 Node.js 中，引入一个模块非常简单，如下我们创建一个 **main.js** 文件并引入 hello 模块，代码如下:  （hello 模块是一个js 文件，每个文件就是一个模块）

```javascript
var hello = require('./hello');  //此处免写后缀 .js

hello.world();
```

以上实例中，代码 require('./hello') 引入了当前目录下的 hello.js 文件（./ 为当前目录，node.js 默认后缀为 js）。

Node.js 提供了export 和 require 两个对象，其中 export 是 **模块 **公开的接口，require 用于从外部获取一个 **模块** 的接口，即所获取模块的export对象。

在以上示例中，hello.js 通过 exports 对象把 world 作为模块的访问接口，在 main.js 中通过 require('./hello') 加载这个模块，然后就可以直接访 问 hello.js 中 exports 对象的成员函数了。

有时候我们只是想把一个对象封装到模块中，格式如下：

```javascript
module.exports = function() {
  // ...
}
```

例如:

```javascript
//hello.js 
function Hello() { 
    var name; 
    this.setName = function(thyName) { 
        name = thyName; 
    }; 
    this.sayHello = function() { 
        console.log('Hello ' + name); 
    }; 
}; 
module.exports = Hello;
```

这样就可以直接获得这个对象了：

```javascript
//main.js 
var Hello = require('./hello'); 
hello = new Hello(); 
hello.setName('BYVoid'); 
hello.sayHello(); 
```

模块接口的唯一变化是使用 module.exports = Hello 代替了exports.world = function(){}。 在外部引用该模块时，其接口对象就是要输出的 Hello 对象本身，而不是原先的 exports。

#### 服务端的模块放在哪里

也许你已经注意到，我们已经在代码中使用了模块了。像这样：

```
var http = require("http");

...

http.createServer(...);
```

Node.js 中自带了一个叫做 **http** 的模块，我们在我们的代码中请求它并把返回值赋给一个本地变量。

这把我们的本地变量变成了一个拥有所有 http 模块所提供的公共方法的对象。



服务器端规范：

- **[CommonJS规范](https://link.zhihu.com/?target=http%3A//www.commonjs.org/)**：是 Node.js 使用的模块化规范。

CommonJS 就是一套约定标准，不是技术。用于约定我们的代码应该是怎样的一种结构。CommonJS旨在 将 运行在浏览器 之外的 JavaScript 进行标准化，并已经解决了大量的 JavaScript 问题（如全局污染、命名冲突）。

浏览器端规范：

- **AMD规范**：在推广过程中对模块化定义的规范化产出。

```text
- 异步加载模块；

- 依赖前置、提前执行：require([`foo`,`bar`],function(foo,bar){});   //也就是说把所有的包都 require 成功，再继续执行代码。

- define 定义模块：define([`require`,`foo`],function(){return});
```

- **CMD规范**：是 **[SeaJS](https://link.zhihu.com/?target=http%3A//seajs.org/)** 在推广过程中对模块化定义的规范化产出。淘宝团队开发。

```text
同步加载模块；

  依赖就近，延迟执行：require(./a) 直接引入。或者Require.async 异步引入。   //依赖就近：执行到这一部分的时候，再去加载对应的文件。

  define 定义模块， export 导出：define(function(require, export, module){});
```



**require 重复引入问题**

第一次加载某个模块时，Node.js会缓存该模块。Node.js 默认先从缓存中加载模块，一个模块被加载一次之后，就会在缓存中维持一个副本，如果遇到重复加载的模块会直接提取缓存中的副本，也就是说在任何时候每个模块都只在缓存中有一个实例。

**require 加载模块的时候是同步还是异步？**

1. 一个作为公共依赖的模块，当然想一次加载出来，同步更好
2. 模块的个数往往是有限的，而且 Node.js 在 require 的时候会自动缓存已经加载的模块，再加上访问的都是本地文件，产生的IO开销几乎可以忽略。

**require() 的缓存策略**

Node.js 会自动缓存经过 require 引入的文件，使得下次再引入不需要经过文件系统而是直接从缓存中读取。不过这种缓存方式是经过文件路径定位的，即使两个完全相同的文件，但是位于不同的路径下，会在缓存中维持两份。

如果想要多次执行某个模块，可以让该模块输出一个函数，然后每次`require`这个模块的时候，重新执行一下输出的函数。

所有缓存的模块保存在`require.cache`之中，如果想删除模块的缓存，可以像下面这样写。

```javascript
console.log(require.cache)  //获取目前在缓存中的所有文件。

// 删除指定模块的缓存
delete require.cache[moduleName];

// 删除所有模块的缓存
Object.keys(require.cache).forEach(function(key) {
  delete require.cache[key];
})
```

注意，缓存是根据绝对路径识别模块的，如果同样的模块名，但是保存在不同的路径，`require`命令还是会重新加载该模块。

