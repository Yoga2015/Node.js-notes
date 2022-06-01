### Node.js 模块化系统 

JavaScript 语言是没有模块化的概念的，直到 Node.js 的诞生，**把 JavaScript 语言带到服务端后，面对文件系统、网络、操作系统等等复杂的业务场景，模块化就变得不可或缺**。实际中 开发复杂 Web应用 的时候，通常需要把各个功能进行拆分、封装到不同的文件 并 在需要的时候引用该文件，即进行代码的模块化管理。几乎所有的编程语言都有自己的模块组织方式，比如Java中的包、C#中的程序集，而Node.js采用CommonJS模块规范。Node.js 从诞生到超火，离不开它成熟的模块化实现，Node.js 的模块化是在 CommonJS 规范的基础上实现的。

```
CommonJS模块规范

CommonJS 模块规范 用于约定我们的代码应该是怎样的一种结构。，CommonJS旨在 将 运行在浏览器 之外的 JavaScript 进行标准化，并已经解决了大量的 JavaScript 问题（如全局污染、命名冲突）。Node.js 对CommonJS的实现中，每个模块都会被封装在一个单独的JS文件中，即一个文件就是一个模块，而文件路径就是模块名。

想了解 CommonJS模块规范,请移步： https://zhuanlan.zhihu.com/p/446513619
```

#### 模块化的理解

##### 什么是模块化

**概念**：将一个复杂的程序依据一定的规则（规范）封装成几个块（文件），并组合在一起。

模块的内部数据、实现是私有的, 只是向外部暴露一些接口(方法)与外部其它模块通信。

最早的时候，我们会把所有的代码都写在一个js文件里，那么，耦合性会很高（关联性强），不利于维护；而且会造成全局污染，很容易命名冲突。

##### 模块化的好处

- 避免命名冲突，减少命名空间污染
- 降低耦合性；更好地分离、按需加载
- **高复用性**：代码方便重用，别人开发的模块直接拿过来就可以使用，不需要重复开发类似的功能。
- **高可维护性**：软件的声明周期中最长的阶段其实并不是开发阶段，而是维护阶段，需求变更比较频繁。使用模块化的开发，方式更容易维护。
- 部署方便

#### 模块化规范

##### 模块化规范的引入

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

#### 模块化规范

服务器端规范：

- **[CommonJS规范](https://link.zhihu.com/?target=http%3A//www.commonjs.org/)**：是 Node.js 使用的模块化规范。

CommonJS 就是一套约定标准，不是技术。用于约定我们的代码应该是怎样的一种结构。

浏览器端规范：

- **[AMD规范](https://link.zhihu.com/?target=https%3A//github.com/amdjs/amdjs-api)**：是 **[RequireJS](https://link.zhihu.com/?target=http%3A//requirejs.org/)** 在推广过程中对模块化定义的规范化产出。

```text
- 异步加载模块；

- 依赖前置、提前执行：require([`foo`,`bar`],function(foo,bar){});   //也就是说把所有的包都 require 成功，再继续执行代码。

- define 定义模块：define([`require`,`foo`],function(){return});
```

- **[CMD规范](https://link.zhihu.com/?target=http%3A//whyknown.com/detail.html%3Fid%3DU2FsdGVkX1%2BOBcMljOBDRu%2Bc7gUmv9Yplijinwo1Irg%3D)**：是 **[SeaJS](https://link.zhihu.com/?target=http%3A//seajs.org/)** 在推广过程中对模块化定义的规范化产出。淘宝团队开发。

```text
同步加载模块；

  依赖就近，延迟执行：require(./a) 直接引入。或者Require.async 异步引入。   //依赖就近：执行到这一部分的时候，再去加载对应的文件。

  define 定义模块， export 导出：define(function(require, export, module){});
```

PS：面试时，经常会问AMD 和 CMD 的区别 ，以及 ES6规范。



为了让Node.js的文件可以相互调用，Node.js提供了一个简单的模块系统。

模块是Node.js 应用程序的基本组成部分，文件和模块是一一对应的。换言之，一个 Node.js 文件就是一个模块，这个文件可能是JavaScript 代码、JSON 或者编译过的C/C++ 扩展。

#### 引入模块

在 Node.js 中，引入一个模块非常简单，如下我们创建一个 **main.js** 文件并引入 hello 模块，代码如下:

```javascript
var hello = require('./hello');
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

```
exports 和 module.exports 的使用

如果要对外暴露属性或方法，就用 exports 就行，要暴露对象(类似class，包含了很多属性和方法)，就用 module.exports。
```

