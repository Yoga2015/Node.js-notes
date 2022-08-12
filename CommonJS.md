### CommonJS

JavaScript 语言是没有模块化的概念的，直到 Node.js 的诞生，**把 JavaScript 语言带到服务端后，面对文件系统、网络、操作系统等等复杂的业务场景，模块化就变得不可或缺**。实际中 开发复杂 Web应用 的时候，通常需要把各个功能进行拆分、封装到不同的文件 并 在需要的时候引用该文件，即进行代码的模块化管理。几乎所有的编程语言都有自己的模块组织方式，比如Java中的包、C#中的程序集，而Node.js采用CommonJS模块规范。Node.js 从诞生到超火，离不开它成熟的模块化实现，Node.js 的模块化是在 CommonJS 规范的基础上实现的。 CommonJS是 node.js 的很重要的规范，CommonJS 的模块化是使用在后端的，前端是不支持的（浏览器不支持）。

```
CommonJS模块规范
	CommonJS旨在 将 运行在浏览器 之外的 JavaScript 进行标准化，并已经解决了大量的 JavaScript 问题（如全局命名冲突）。Node.js 对CommonJS的实现中，每个模块都会被封装在一个单独的JS文件中，即一个文件就是一个模块，而文件路径就是模块名。
```

#### CommonJS 是什么？

CommonJS 是一个项目，其目标是为 JavaScript 在网页浏览器之外**创建模块约定**。创建这个项目的主要原因是当时**缺乏普遍可接受形式**的 **JavaScript 脚本模块单元**，模块在与运行JavaScript 脚本的常规网页浏览器所提供的不同的环境下可以重复使用。

我们知道，很长一段时间 JavaScript 语言是没有模块化的概念的，直到 Node.js 的诞生，**把 JavaScript 语言带到服务端后，面对文件系统、网络、操作系统等等复杂的业务场景，模块化就变得不可或缺**。于是 Node.js 和 CommonJS 规范就相得益彰、相映成辉，共同走入开发者的视线。

<img src="F:\Typora\typora-user-images\image-20220307105739197.png" alt="image-20220307105739197" style="zoom: 50%;" />



**CommonJS 最初是服务于服务端的，所以我说 CommonJS 不是前端，但它的载体是前端语言 JavaScript，为后面前端模块化的盛行产生了深远的影响，奠定了结实的基础。CommonJS：不是前端却革命了前端 !**

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
    <script src="zepto.js"></script>
    <script src="fastClick.js"></script>
    <script src="util/login.js"></script>
    <script src="util/base.js"></script>
    <script src="util/city.js"></script>
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

PS：面试时，经常会问AMD 和 CMD 的区别。

#### 为什么需要模块化？

##### 1、没有模块化时，前端是什么样子 

JavaScript 诞生之初只是作为一个脚本语言来使用，做一些简单的表单校验等等。所以代码量很少，最开始都是直接写到 `<script>` 标签里，如下所示：

```html
// index.html
<script>
var name = 'xialuo'
var age = 14
</script>
```

随着业务进一步复杂，Ajax 诞生以后，前端能做的事情越来越多，代码量飞速增长，开发者们开始把 JavaScript 写到独立的 js 文件中，与 html 文件解耦。像下面这样：

```js
// index.html
<script src="./mine.js"></script>

// mine.js
var name = 'xialuo'
var age = 14
```

再后来，更多的开发者参与进来，更多的 js 文件被引入进来:

```javascript
// index.html
<script src="./mine.js"></script>
<script src="./a.js"></script>
<script src="./b.js"></script>
// mine.js
var name = 'xialuo'
var age = 14

// a.js
var name = 'dongwei'
var age = 15

// b.js
var name = 'qiuya'
var age = 16
```

不难发现，问题已经来了！ JavaScript 在 ES6 之前是没有模块系统，也没有封闭作用域的概念的，所以上面三个 js 文件里申明的变量都会**存在**于**全局作用域中**。不同的开发者维护不同的 js 文件，很难保证不和其它 js 文件冲突。全局变量污染开始成为开发者的噩梦。

#### 模块化的原型

为了解决全局变量污染的问题，开发者开始使用命名空间的方法，既然命名会冲突，那就加上命名空间呗，如下所示：

```html
// index.html
<script src="./mine.js"></script>
<script src="./a.js"></script>
<script src="./b.js"></script>
// mine.js
app.mine = {}
app.mine.name = 'xialuo'
app.mine.age = 14

// a.js
app.moduleA = {}
app.moduleA.name = 'dongwei'
app.moduleA.age = 15

// b.js
app.moduleB = {}
app.moduleB.name = 'qiuya'
app.moduleB.age = 16
```

此时，已经开始有隐隐约约的模块化的概念，只不过是用命名空间实现的。这样在一定程度上是解决了命名冲突的问题， b.js 模块的开发者，可以很方便的通过 `app.moduleA.name` 来取到模块A中的名字，但是也可以通过 `app.moduleA.name = 'rename'` 来**任意改掉模块A中的名字**，而这件事情，模块A却毫不知情！这显然是不被允许的。

聪明的开发者又开始**利用 JavaScript 语言的函数作用域**，使用**闭包**的特性来解决上面的这一问题。

```html
// index.html
<script src="./mine.js"></script>
<script src="./a.js"></script>
<script src="./b.js"></script>
// mine.js
app.mine = (function(){
    var name = 'xialuo'
    var age = 14
    return {
        getName: function(){
            return name
        }
    }
})()

// a.js
app.moduleA = (function(){
    var name = 'dongwei'
    var age = 15
    return {
        getName: function(){
            return name
        }
    }
})()

// b.js
app.moduleB = (function(){
    var name = 'qiuya'
    var age = 16
    return {
        getName: function(){
            return name
        }
    }
})()
```

现在 b.js 模块可以通过 `app.moduleA.getName()` 来取到模块A的名字，但是各个模块的名字都保存在各自的函数内部，没有办法被其它模块更改。这样的设计，已经有了模块化的影子，每个模块内部维护私有的东西，开放接口给其它模块使用，但依然不够优雅，不够完美。譬如上例中，模块B可以取到模块A的东西，但模块A却取不到模块B的，因为上面这三个模块加载有先后顺序，互相依赖。当一个前端应用业务规模足够大后，这种依赖关系又变得异常难以维护。

综上所述，前端需要模块化，并且模块化不光要处理 **全局变量污染**、**数据保护**的问题，还要很好的**解决模块之间依赖关系的维护**。



#### **commonJS规范简介**

既然JavaScript需要模块化来解决上面的问题，那就需要制定模块化的规范，CommonJs就是解决上面问题的模块化规范，规范就是规范，没有为什么，就和编程语言的语法一样。我们一起来看看。

Node.js 应用由模块组成，每个文件就是一个模块，有自己的作用域。在一个文件里面定义的变量、函数、类，都是私有的，对其他文件不可见。

```js
// a.js
var name = 'xialuo'
var age = 14
```

上面代码中，a.js 是 Node.js 应用中的一个模块，里面申明的变量 `name` 和 `age` 是 a.js 私有的，其他文件都访问不到。

Commonjs规范还规定，每个模块内部有两个变量可以使用， require  和 module。

require 用来加载某个模块。

module代表当前模块，是一个对象。

CommonJS 规范规定：每个模块内部，module 变量代表当前模块。这个变量是一个对象，它的 exports 属性（即 module.exports）是对外的接口对象。加载某个模块，其实是加载该模块的 module.exports 对象。

##### 在 CommonJS 中，每个文件都可以当作一个模块：

- 在服务器端：模块的加载是运行时同步加载的。
- 在浏览器端: 模块需要提前编译打包处理。首先，既然同步的，很容易引起阻塞；其次，浏览器不认识`require`语法，因此，需要提前编译打包。

**common.js**的规范是可以 定义模块，暴露模块接口 ，引用模块，调用模块。

#### 模块成员导出

```typescript
//a.js
//1、定义一个模块
const name ={
    surname :"li",   //name 姓
    sayName(){
        console.log(this.surname);
    }
}

//再 定义一个模块
const age = {
    age:100,
    
}

//2.1、暴露模块接口   在app.js中引入
// module.exports = {
//     name,
//     age
// }

// 2.2  暴露模块接口
exports.name = name;
exports.age = age;
```

#### 模块成员的导入

```javascript
在另一个js 文件中引用它：
//b.js
//3、引入一个包
const {name,age} = require("./a.js");   //如果你引入一个包，是自定义的包也一定要写清路径，不然又到全局里去找

//4、 调用一个包
name.sayName();
console.log(age.age);
```

### **[exports 和 module.exports 的区别](https://link.zhihu.com/?target=http%3A//whyknown.com/detail.html%3Fid%3DU2FsdGVkX1%2BOBcMljOBDRu%2Bc7gUmv9Yplijinwo1Irg%3D%23toc314)**

最重要的区别：

- 使用exports时，只能单个设置属性 `exports.a = a;`
- 使用module.exports时，既单个设置属性 `module.exports.a`，也可以整个赋值 `module.exports = obj`。

其他要点：

- Node中每个模块的最后，都会执行 `return: module.exports`。

- Node中每个模块都会把 `module.exports`指向的对象赋值给一个变量 `exports`，也就是说 `exports = module.exports`。

- `module.exports = XXX`，表示当前模块导出一个单一成员，结果就是XXX。

- 如果需要导出多个成员，则必须使用 `exports.add = XXX; exports.foo = XXX`。或者使用 `module.exports.add = XXX; module.export.foo = XXX`。

  

node.js  --》Typescript  --》 React --》CKB钱包

