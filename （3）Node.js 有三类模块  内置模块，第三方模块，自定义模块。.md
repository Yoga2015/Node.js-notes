### Node.js 有三类模块 : 内置模块，第三方模块，自定义模块。

#### 常用的内置模块：

```java
    http 创建一个服务器 
    fs  filesystim  文件系统   操作文件 
    path  路径操作  路径=路径名+查询字符串
    url  专门分析网页地址的信息
    querystirng  对查询字符串的操作
    events 事件相关
    process  进程相关
    util 工具方法 
    .... 
```

```typescript
例子：
    const url = require("url");

    const urlString = "http://www.hao828.com/YingYong/HB1/";

    console.log(url.parse(urlString));
```

```typescript
控制台打印如下：
    PS D:\NodeJs\node-basics\04-buildin-module\01-url> node app.js
    Url {
      protocol: 'http:',
      slashes: true,
      auth: null,
      host: 'www.baidu.com:443',
      port: '443',
      hostname: 'www.baidu.com',
      hash: '#tag=3',
      search: '?id=2',
      query: 'id=2',
      pathname: '/path/index.html',
      path: '/path/index.html?id=2',
      href: 'http://www.baidu.com:443/path/index.html?id=2#tag=3'
    }
    PS D:\NodeJs\node-basics\04-buildin-module\01-url>
```



#### 第三方模块

别人写好的、具有特定功能的，第三方的node.js 模块指的是为了实现某些功能，发布的npmjs.org上的模块，按照一定的开源协议供社群使用。如：

```apl
npm install chalk 
```

```apl
const chalk = require('chalk');
console.log(chalk.blue("hello boy"))；
```



#### 自定义的模块

![image-20220323150951037](F:\Typora\typora-user-images\image-20220323150951037.png)

> ##### Node.js中模块化开发规范

- Node.js规定一个**JavaScript文件**就是一个模块, 模块**内部定义的变量和函数**默认情况下在**外部无法得到**
- 模块内部可以使用**exports对象进行成员导出**，使用**require方法**导入其他模块。

![img](F:\Typora\typora-user-images\1564834-20200315222731207-1766468766.png)

导出：你写的模块需要导出去让别人使用
导入：使用require导入别人写好的模块（导入系统模块）

##### 模块成员导出

```typescript
//a.js
//1、定义一个模块
const name ={
    surname :"hong",   //surname 姓
    sayName(){
        console.log(this.surname);
    }
}

//再 定义一个模块
const age = {
    age:100,
    
}

//2、暴露模块接口   在app.js中引入
//2.1
// module.exports = {
//     name,
//     age
// }

// 2.2
exports.name = name;
exports.age = age;
```

##### 模块成员的导入

```javascript
在另一个js 文件中引用它：
//b.js
//3、引入一个包
const {name,age} = require("./a.js");   //如果你引入一个包，是自定义的包也一定要写清路径，不然又到全局里去找

//4、 调用一个包
name.sayName();
console.log(age.age);
```



#### 案例：自定义创建一个包/模块

自定义创建一个包（模块），并暴露出去，供自己、别人使用       （my chunk  我的一部分）

![image-20220123210605251](F:\Typora\typora-user-images\image-20220123210605251.png)

test测试一下

![image-20220123210731572](F:\Typora\typora-user-images\image-20220123210731572.png)

供别人使用前，记得上传放在   https://www.npmjs.com/     

![image-20220124092223504](F:\Typora\typora-user-images\image-20220124092223504.png)

上传后  ， 别人想使用 就下载包  ， 例如想上面自定义的模块，就用： npm i 3-custom -S  ，