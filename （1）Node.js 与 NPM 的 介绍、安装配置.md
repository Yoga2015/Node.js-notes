### Node.js 是什么 ?  

​		Node.js 发布于2009年5月，由Ryan Dahl开发的。 Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境。V8是google开源的JavaScript引擎，用于执行JavaScript，类似JVM执行java字节码。所有浏览器都有运行网页上JavaScript的JavaScript引擎。例如：Safari 有 JavaScriptCore引擎，Firefox有 Spidermonkey的引擎，Chrome有V8引擎。

| java | JavaScript |
| ---- | ---------- |
| JVM  | V8         |
| JRE  | node.js    |

​	Node.js 就是 一个Javascript运行环境 ( runtime environment )  。Node.js 仅仅就是**用于运行普通JavaScript代码的东西**。

​		其实Ryan Dahl创造 Node.js 的目的是为了**编写高性能的web服务器** ，首先看重的是 **事件机制** 和 **异步IO模型** 的优越性，而不是JavaScript。但是他**需要选择一种编程语言实现他的想法**，这种编程语言不能自带 IO功能，并且需要能良好支持事件机制。JavaScript 没有自带IO功能，天生就用于处理浏览器中的 DOM事件，并且拥有一大群程序员，因此就成为了天然的选择 。

　　Node.js 大部分基本模块 都用 JavaScript 编写。在 node.js 出现之前，JavaScript 是作为客户端程序设计语言使用 的，以 JavaScript 写出的程序都是在用户的浏览器上运行。Node.js 可以让 JavaScript 在服务器中运行，也就是可以和系统进行直接交互，例如可以删除系统中文件，操作系统，Node.js最重要的一点是把 js 的战场从前端迁到后端服务器，不再仅仅是浏览器窗口里。

　　JavaScript 是脚本语言，脚本语言需要一个解析器才能运行，在不同的位置又有不一样的解析器，如写入 Html 的 JavaScript 脚本语言，**浏览器**是它的解析器角色。而对于**需要独立运行的 JavaScript , Node.js 就是一个解析器。**每一种解析器都是一个运行环境**，不但允许JavaScript定义各种数据结构，进行各种计算，还允许JavaScript使用允许环境提供的 **内置对象 和 方法 做一些事情，如：运行在浏览器中的 JavaScript 的用途是 **操作DOM**， 浏览器就提供了document之类的内置对象。而运行在 Node.js 中的 JavaScript 的用途是 **操作磁盘文件** 或者 **搭建 http 服务器**， Node.js 就相应提供了 fs、http 等内置对象。

​	发展到现在的Node.js 已经是一个很大的生态系统，我们用它一些软件、插件、框架来帮助我们开发。Node.js 起初是用来写开发高性能Web服务器的，后来结果形成了一个很大大的生态环境，所以前端开发离不开 Node.js 。

####  Node.js的组成：

- JavaScript 是由 **ECMAScript, DOM, BOM **三部分组成。
- Node.js是由 **ECMAScript** 及 **Node 环境** 提供的一些 **附加API** 组成的，包括文件、网络、路径等一些更加强大的 API。

#### Node.js 能做什么：

Node.js 可以解析 JavaScript 代码，（没有浏览器安全级别的限制）提供很多系统级别的API ，如：

（1）文件的读写（file System） 可以创建文件、

（2）可以修改文件等等 进程的管理（Process）

（3）网络通信（Http / Https）

浏览器安全级别的限制： ajax 测试   browser-safe-sandbox



Node.js 使 前端程序员 可以写 后台管理程序的代码 用来完成一些对后端的操作，类似于php，Java等语言，但其实本质上Node.js 就是运行在服务端的 JavaScript，它是一种运行环境，使前端程序员可以搭建自己的服务器完成一些后端操作。



### NPM 是什么?

#### NPM ：Node Package Manager

NPM是Node.js标准的软件 包管理器 。2010年底，Node.js 的包管理器 npm 诞生，是全球最大的开源库生态系统。

日常在Node.js上开发时，会用到很多别人已经写好的 javascript 代码，如果每当我们需要别人的代码时，都根据名字搜索一下，下载源码，解压，再使用，会非常麻烦。于是就出现了包管理器npm：大家把自己写好的源码上传到 npm官网（https://www.npmjs.com/）上，如果要用某个或某些个，直接通过npm安装就可以了，不用管那个源码在哪里。并且如果我们要使用模块A，而模块A又依赖模块B，模块B又依赖模块C和D，此时npm会根据依赖关系，把所有依赖的包都下载下来并且管理起来。试想如果这些工作全靠我们自己去完成会多么麻烦。

**NPM 是 随同 Node.JS **一起安装的包管理工具。能解决Node.js代码部署上的很多问题，常见的使用场景有以下几种：****

```
- 允许用户从 **NPM服务器** 下载 别人编写的第三方包 到 本地使用。
- 允许用户从 **NPM服务器** 下载并安装 别人编写的命令行程序 到 本地使用。
- 允许用户将 自己编写的包 或 命令行程序 上传到 **NPM服务器** 供别人使用。
```

**想让 Node.js 发挥到极致，得开发自己的包，也需要第三方的包、也需要内置的包。**



### node.js 安装 、npm 安装

Node.js 安装包及源码下载地址为：https://nodejs.org/en/download/。

##### 配置环境变量

安装完成后，右击"我的电脑"，点击"属性"，选择"高级系统设置"；

在 "系统变量" 中设置 1 项属性，Node_path (大小写无所谓) , 若已存在则点击"编辑"，不存在则点击"新建"。

![img](F:\Typora\typora-user-images\906897-20220322150314120-241286044.png)

配置完成后，来检测Node是否安装成功：

　　**点击开始-运行-cmd（win+R），打开dos，输入“node --version”检查Node.js版本**

新版的 Node.js 已经集成了 npm ，NPM 由于是随同 Node.js 一块儿安装的，因此NPM也一并安装好了，同样在cmd命令行输入“npm -v” 来测试 npm 是否 安装成功。

如果你安装的是旧版本的 npm，可以很容易得通过 npm 命令来升级，命令如下：

```
npm install npm -g
```

**使用淘宝 NPM 镜像**：

​		国内直接使用 npm 的官方镜像是非常慢的，这里推荐使用淘宝 NPM 镜像。淘宝 NPM 镜像是一个完整 npmjs.org 镜像，你可以用此代替官方版本，同步频率目前为 10分钟 一次以保证尽量与官方服务同步。

你可以使用淘宝定制的 cnpm (gzip 压缩支持) 命令行工具代替默认的 npm:

```
npm install -g cnpm --registry=https://registry.npmmirror.com

这样就可以使用 cnpm 命令来安装模块了：    cnpm install [name]

安装nrm 工具：    注意有问题！
   	  npm install nrm -g      // 全局安装nrm工具 换源
    
    使用nrm :
        查看下载源：nrm ls 
        换源：nrm use taobao   // 从国外的 换成 国内的淘宝源  
```

##### 使用 npm 命令安装模块

npm 安装 Node.js 模块语法格式如下：

```
npm install <Module Name>
```

##### **查看安装信息**

你可以使用以下命令来 **查看所有全局安装的模块** ：

```
npm list -g
```

使用 npm 命令安装第三方模块（依赖）“ jquery 模块 ”:

```javascript
npm install jquery      //什么也不加默认是生产依赖
		
npm install  //这个命令会在你命令行的当前目录下自动创建一个叫node_modules的文件夹，jquery安装完毕，默认会把依赖放到node_modules这个文件夹中。

jquery安装完毕会记录到package.json中，如下：
	 "dependencies": {
	      "jquery": "^3.6.0"
	  }

初始化一个项目，或者 记录项目中都有哪些第三方模块：
通过 npm init -y 命令为你的项目创建一个package.json文件，（自动生成一个package.json文件）
这个文件可以记录你的项目中都有哪些第三方的依赖   

install 可以简写成 i    例如：npm i jquery

npm i  和  npm install  的区别, windows下实际区别点主要如下：

① npm i 安装的模块及依赖，使用npm uninstall是没有办法删除的，必须使用npm uninstall i才可以删除
② npm i 会帮助检测与当前node最匹配的npm的版本号，并匹配出相互依赖的npm包应该升级的版本号
③ npm i 安装的一些包，在当前的node版本下是没有办法使用的，必须使用建议版本
④ npm i 安装出现问题是不会出现npm-debug.log文件的，但npm install 安装出现问题是有这个文件的。
```

##### 第三方模块分成两类：

```javascript
    开发模块：开发项目时需要的用到的模块  仅仅是开发时需要
    生产模块：项目上线时还需要的用的模块   表示把###安装成开发依赖
    
    npm i ### --save-dev   表示把###安装成开发依赖
    npm i ### -D           表示把###安装成开发依赖
    在package.json中的记录如下：
        "dependencies": {},
        "devDependencies": {     //开发环境依赖模块
            "jquery": "^3.6.0"
        }
        
    npm i ### --save   表示把###安装成生产依赖
    npm i ### -S        表示把###安装成生产依赖
    在package.json中的记录如下：
         "dependencies": {
             "jquery": "^3.6.0"
          },
          "devDependencies": {
             "jquery": "^3.6.0"
          }

```

如果要查看某个模块的版本号，可以使用命令如下：

```javascript
$ npm list jquery
```

刚安装好的 jquery 包就放在了工程目录下的 node_modules 目录中，因此在代码中只需要通过 require('jquery') 的方式就好，无需指定第三方包路径。

```javascript
var jquery = require('jquery');
```

安装 Express 并将其保存到依赖列表中，如果只是临时安装 Express，不想将它添加到开发依赖列表中，只需略去 --save 参数即可：（作用域：只能当前项目下使用）

```
 npm install express --save
```

安装 Node 的模块时，如果指定了 --save 参数，那么此模块将被添加到 package.json 文件中 dependencies 依赖列表中。 然后通过 npm install 命令即可自动安装依赖列表中所列出的所有模块。

npm 的全称是 Node Package Manager 是 JavaScript 世界的包管理工具,并且是 Node.js 平台的默认包管理工具。通过 npm 可以安装、共享、分发代码,管理项目依赖关系。

已安装的模块/包，是会先在全局里找，找不到了，才继续到工程目录下的 node_modules 目录中找

##### 卸载模块 

```
npm uninstall jquery
```

卸载后，你可以到 /node_modules/ 目录下查看包是否还存在，或者使用以下命令查看：

```
npm ls  
```

##### 更新模块

```
npm update jquery
```

##### 搜索模块

```
npm search jquery
```

