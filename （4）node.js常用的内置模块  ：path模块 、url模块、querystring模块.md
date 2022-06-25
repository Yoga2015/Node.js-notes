## path模块 、url模块、querystring模块 

实际开发项目时，需要引入一些依赖的模块 来才能完整地制作项目。

### path模块

**引入path模块**：

```javascript
var path  =  require('path');   
```

**基本语法**

```javascript
path.basename()；	获取文件
path.dirname();	获取路径
path.extname();	获取后缀
path.join();	合并目录
path.resolve();	合并目录（自带解析）
```



### URL模块

引入url 模块

```javascript
var url = require('url');    //
```

 **基本语法**

| url.parse(地址 ，true) ； | 获取参数 |
| ------------------------- | -------- |

# ![image-20220626000147500](F:\Typora\typora-user-images\image-20220626000147500.png)

### querystring模块   (已弃用)

`querystring` 模块提供了一些实用函数，用于解析与格式化 URL 查询字符串。querystring从字面上的意思就是查询字符串，一般是对http请求所带的数据进行解析。querystring模块只提供4个方法,分别是 querystring.parse、querystring.stringify、querystring.escape 和 querystring.unescape。

首先，使用querystring模块之前，需要require进来：

```typescript
const querystring = require("querystring");
```

其次，就可以使用模块下的方法了：

```javascript
//parse这个方法是将一个字符串反序列化为一个对象。
querystring.parse(str,separator,eq,options)**

//stringify这个方法是将一个对象序列化成一个字符串，与querystring.parse相对。
querystring.stringify(obj,separator,eq,options)

//escape可使传入的字符串进行编码
querystring.escape(str)

//unescape方法可将含有%的字符串进行解码
querystring.unescape(str)
```

**querystring 将替换为 querystringify**。

