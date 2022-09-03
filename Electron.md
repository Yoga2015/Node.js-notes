> ### Electron

![image-20211112165217538](F:\Typora\typora-user-images\image-20211112165217538.png)



![image-20211112165755876](F:\Typora\typora-user-images\image-20211112165755876.png)

1、克隆示例项目的仓库：

git clone https://github.com/electron/electron-quick-start

###### 2、electron  设置淘宝镜像源 ，然后再安装就下载快

```typescript
npm config set ELECTRON_MIRROR https://npm.taobao.org/mirrors/electron/
```

![image-20211112170820054](F:\Typora\typora-user-images\image-20211112170820054.png)

![image-20211112171116894](F:\Typora\typora-user-images\image-20211112171116894.png)

> Electron  分为 渲染进程 和 主进程

**渲染进程 ：**用户所看到的web界面 就是由渲染进程描绘出来的，包括HTML  、CSS、  JavaScript 。

**主进程：** electron 运行 package.json 的main 脚本的进程 被称为 主进程，在主进程 中 运行的脚本通过创建web页面来展示用户界面。一个 electron应用 **总是有** 且 **只有一个主进程**。

> 渲染进程的调试就是我们熟悉的网页的调试

 先把项目运行起来再调试：

![image-20211112195910020](F:\Typora\typora-user-images\image-20211112195910020.png)

> 主进程调试

![image-20211112200324248](F:\Typora\typora-user-images\image-20211112200324248.png)

表示可以通过5858端口进行 debug 主进程 ，同时也启动main.js 脚本

![image-20211112200824470](F:\Typora\typora-user-images\image-20211112200824470.png)

然后我们重新启动项目   npm start 

![image-20211112201201371](F:\Typora\typora-user-images\image-20211112201201371.png)



同时也可以通过浏览器运行调试   ， 比如 chrome浏览器 用： chrome://inspect

![image-20211112201649510](F:\Typora\typora-user-images\image-20211112201649510.png)

