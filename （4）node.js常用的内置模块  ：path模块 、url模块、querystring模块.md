## path模块 、url模块、querystring模块 

实际开发项目时，需要引入一些依赖的模块 来才能完整地制作项目。

### 内置模块 - path

用于处理文件和目录的路径

```javascript
var path = require('path');   //引入path模块
```

**基本语法**

```
path.basename()；	获取文件
path.dirname();	获取路径
path.extname();	获取后缀
path.join();	合并目录
path.resolve();	合并目录（自带解析）
```

**案例：** 解析路径

```javascript
const path = require("path");

const pathAdress = path.parse('/home/user/dir/file.txt');

console.log(pathAdress)
```

![解析路径](https://user-images.githubusercontent.com/94358089/177195242-19ee4f3b-260d-4298-beae-7487fd16b944.png)



### 内置模块 - url

引入url 模块

```javascript
var url = require('url');    //
```

 **基本语法**

| url.parse(地址 ，true) ； | 获取参数 |
| ------------------------- | -------- |

![watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5aSn6IGq5piO56CB5Yac5b6Q,size_20,color_FFFFFF,t_70,g_se,x_16](https://user-images.githubusercontent.com/94358089/177195337-ebcc8d35-d1c5-4ec2-a443-51c6b012147f.png)


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

