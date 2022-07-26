## os模块、fs模块、path模块 、url模块

### os模块

`os` 模块提供了一些基本的系统操作函数。 可以使用以下方式访问它：

```js
const os = require('os');
```

**案例：**获取系统信息

```javascript
var os = require('os');

console.log("操作系统的主机名"+os.hostname());
console.log("操作系统名 "+os.type());
console.log("操作系统 CPU 架构 "+os.arch());
console.log("操作系统的发行版本  "+os.release());
console.log('系统内存总量 : ' + os.totalmem() + " bytes.");
```

![image-20220708111037103](F:\Typora\typora-user-images\image-20220708111037103.png)



### fs 模块

f :  file文件, s :  system系统 。

fs文件系统模块 是node.js 官方提供的，用来 **操作文件** 的模块，负责读写文件。

fs 模块 还提供了异步和同步方法。

回顾一下什么是异步方法？由于JavaScript 是单线程模型，执行IO操作时，JavaScript代码无需等待，而是传入回调函数 后，继续执行后续JavaScript代码。

内置模块 fs 文件操作，文件操作有四种 增删改查 CRUD (create 、read 、update、delete)

回调函数是错误优先的回调函数，错误优先的回调函数中第一个参数是错误error。

回调函数，可以理解为，就是说这个函数是我传进去的，什么时候执行我不管，那么什么时候执行呢？当读取文件、写文件时，这些都是一些io操作，是比较耗时的，所以呢，一定是异步，所以才有了是回调函数。

node.js 中每个方法里面都一个同步、异步方法，回调函数是异步方法。

#### **引入fs模块**

```
var fs = require('fs');    // fs模块引入
```

**基本语法**

| fs.writeFile(参数1，参数2，参数3);<br />参数1 文件路径   参数2 写入文件的内容  参数3 err回调 | 向指定文件中容写入内容           |
| ------------------------------------------------------------ | -------------------------------- |
| fs.appendFile(参数1，参数2，参数3);<br />参数1 文件路径  参数2 内容 参数3 err回调 | 追加内容                         |
| **fs.readFile(参数1，参数2，参数3) ;<br />参数1 文件路径 参数2 字符串 参数3 读取内容回调** | **读取指定文件中的内容**         |
| fs.existsSync();                                             | 判断文件是否存在                 |
| **fs.stat('目标文件或者文件夹' ，（err,stat）=>{<br />console.log(stat.isFile()); //是否文件<br />console.log(stat.isDirectory()); //是否是目录<br />console.log(stat.size);  //128字节<br />})** | **判断文件是否是文件或着文件夹** |
| fs.unlink(参数1，参数2)；<br />参数1 文件路径   参数2 错误回调 | 删除指定的文件                   |

#### 使用fs模块

##### 1、通过 fs 模块 对 文件夹 进行 **增删改查** 操作，同步和异步 。

```js
const fs = require("fs");

//创建一个文件夹 log , 回调函数是错误优先的回调函数，错误优先的回调函数中第一个参数是错误
fs.mkdir("log",(err)=>{
    if (err) throw err
    console.log("文件夹创建成功")
})

//修改文件夹名字
fs.rename("./log","./log2",()=>{
    console.log("文件夹名称修改成功")
})

// //删除文件夹 
fs.rmdir("./log2",()=>{
    console.log("删除文件夹")
})

// //查询文件夹中的内容
fs.readdir("./logs",(err,result)=>{
    console.log(result)
})
```



##### 2、通过 fs 模块 对 文件进行 **增删改查 **操作，同步和异步 。

```js
const fs = require("fs");

//读取指定文件中的内容 , 但是 不加参数 utf-8 会返回的是buffer,不是二进制，这是数据流，得转成字符串,用 toString()。
fs.readFile('./logs/abc.log',,(err,content)=>{
    console.log(content.toString())
})

fs.readFile('./logs/abc.log','utf-8',(err,content)=>{
    console.log(content)
})

fs.existsSync();

//向指定文件中容写入内容    ，这样重复写入会存在覆盖
fs.writeFile('./logs/abc.log','hello\nworld12',(err)=>{
    console.log('给abc.log文件写入 helloworld 圆满成功')
})

//给原来的文件中追加内容
fs.appendFile('./logs/abc.log','追加内容，不覆盖前面的内容',(err)=>{
    console.log('给abc.log文件中写入追加内容，不覆盖前面的内容')
})

// 删除指定文件
fs.unlink('./logs/abc.txt',(err)=>{
    console.log('删除文件')
})


//异步读取文件
//从这个上下文，执行代码是从上到下的，但是 'continue...'打印在先，是因为用了异步（回调函数）
fs.readFile('./logs/abc.log',(err,content)=>{
    console.log(content.toString())
})
console.log('continue...')

// 同步方法
const content = fs.readFileSync('./logs/abc.log','utf-8');
console.log(content)
console.log('continue...')

// 批量增加文件
for(var i = 0; i<10;i++){
    fs.writeFile("./logs/log-0.log",`log-${i}`,(err)=>{
        console.log("done.")
    })
}

// 遍历读取文件夹
fs.readdir("./",(err,content)=>{
    content.forEach((value,index)=>{
        
    })
})

// 监视文件   监听一个文件的变化
fs.watch('./logs/log-0.log',(err)=>{
    console.log("file has change")
})

```

<img src="F:\Typora\typora-user-images\image-20220310113122583.png" alt="image-20220310113122583" style="zoom: 67%;" />

<img src="F:\Typora\typora-user-images\image-20220310170827931.png" alt="image-20220310170827931" style="zoom:80%;" />

<img src="F:\Typora\typora-user-images\image-20220310170543953.png" alt="image-20220310170543953"  />



#### 案例：考试成绩整理

![image-20220724200017305](F:\Typora\typora-user-images\image-20220724200017305.png)

**核心实现步骤：**

![image-20220724200556791](F:\Typora\typora-user-images\image-20220724200556791.png)

```js
//1、导入需要的fs 文件系统模块
const fs = require('fs');

// 2、使用fs.readFile()方法，读取素材目录下的abc.txt文件
fs.readFile('./abc.txt','utf-8',(err,content)=>{
    //3.1、判断文件是否读取成功
    if(err){
       return console.log("读取文件失败" + err.message)
    }
    //3.2、若读取文件成功显示内容 
    console.log("读取文件成功  "+ content)   //看实现效果

    //4文件读取成功后 ，处理成绩数据
    //4.1 先把成绩的数据，按照空格进行分隔
    const arrOld = content.split(' ');
    // console.log(arrOld)     //看实现效果

    //4.2 循环分割后的数组，对每一项数据，进行字符串的替换操作
    const arrNew =[];
    arrOld.forEach(item =>{
        arrNew.push(item.replace('=',' : '))
    })
    // console.log(arrNew)    //看实现效果

    //4.3 把新数组中的每一项，进行合并，得到一个新的字符串
    const newStr = arrNew.join('\r\n');
    // console.log(newStr)     //看实现效果

   // 5、调用 fs.writeFile()方法 , 将处理完毕的成绩数据，写入到新文件 EFG.txt 中
    fs.writeFile('./EFG.txt',newStr,(err)=>{
        if(err){
            return console.log("写入文件失败！" + err.message)
        }
        console.log("成绩写入成功！")
    })  
    
})
```

![image-20220724230220225](F:\Typora\typora-user-images\image-20220724230220225.png)



### path模块

##### 什么是 path模块？

path模块 是node.js 官方提供的，用来 **处理路径** 的模块。

##### **引入path模块**

```javascript
var path = require('path');   //引入path模块
```

##### **path模块语法**

```javascript
path.join();	用来将多个路径片段并接成一个完整额路径字符串   
path.basename()；	用来获取路径中的文件名
path.dirname();	获取路径
path.extname();	获取路径中的文件扩展名 ，即后缀名
path.resolve();	合并目录（自带解析）
```

##### **使用path模块** 

```javascript
const path = require("path");

// 解析路径
const pathAddress = path.parse('/home/user/dir/file.txt');
console.log(pathAddress)

// 路径拼接
const pathAddress2 = path.join(__dirname,'/files/abc.txt');
console.log(pathAddress2);

//获取路径中的文件名
const fpath = '/a/b/c/index.html';
var fullname = path.basename(fpath);
console.log(fullname);
var fullnam2 = path.basename(fpath,'.html');
console.log(fullnam2);
```

![image-20220724231002778](F:\Typora\typora-user-images\image-20220724231002778.png)

##### 案例：路径拼接

```js
const path = require("path");
const fs = require('fs');

fs.readFile(path.join(__dirname,'/files/abc.txt'),'utf-8',(err,dataStr)=>{
    if(err){
        return console.log(err.message)
    }
    console.log(dataStr)
})
```

![image-20220724230022124](F:\Typora\typora-user-images\image-20220724230022124.png)



###  url模块

```javascript
var url = require('url');    //引入URL内置模块
```

 **基本语法**

| url.parse(地址 ，true) ； | 获取参数 |
| ------------------------- | -------- |

**案例：**获取地址的信息

```javascript
const url = require("url");

const urlString = url.parse("https://www.npmjs.com/package/url");

console.log(urlString);
```

![image-20220708112259569](F:\Typora\typora-user-images\image-20220708112259569.png)





