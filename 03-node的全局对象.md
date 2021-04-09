### 常见的全局对象
+ Node中给我们提供了一些全局对象, 方便我们进行一些操作
  - 这些全局变量很多
  - 某些全局变量不常用

### 特殊的全局对象
+ 什么是特殊的全局对象
  - 某些全局对象可以在模块中任意使用, 但是在命令行交互中是不可以使用的
  - 包括: __dirname, __filename, export, module, require
+ __dirname: 获取当前文件所在的路径
  - 注意: 这个不包括文件名
+ __filename: 获取当前文件所在的路径和文件名
  - 注意: 这个包含文件名

### 常见的全局对象
+ process对象: process提供了Node进程中的相关信息
  - 比如Node的运行环境, 参数信息等等
  - 后面的项目中, 会讲到如何将一些环境变量读取到process的env中
+ console对象: 这个对象提供了简单的调试控制台
+ 定时器函数: 在Node中使用定时器有好几种
  - setTimeout(): callback会在设置的毫秒后执行一次
  - setInterval(): callback会在每设置的后面间隔重复执行一次
  - setImmediate(): callbackI/O时间后的回调的"立即"执行
    + 这里不展开讨论setImmediate和setTimeout设置0毫秒之前的区别
    + 实质上这两个涉及事件循环的阶段问题
  - process.nextTick(): 添加到下一次tick队列中
    + 以后详细讲解
``` js
// 定时器函数
setTimeout(() => {
  console.log("setTimeout");
},1000)

// 注意如果要使用clearInterval等的清除定时器的方法则需要给对应的定时器函数赋名
let interval = setInterval(() => {
  console.log("setInterval");
},1000)

setImmediate(() => {
  console.log("setImmediate");
})

process.nextTick(() => {
  console.log("process.nextTick");
})

setTimeout(() => {
  clearInterval(interval)
},3000)

```


### global对象
+ global是一个全局对象, 事实上前面中提到的process, console, setTimeout等都有被放到global对象中

### global对象和浏览器中的window对象的区别
+ 在浏览器中，全局变量都是在window上的，比如有document、setInterval、setTimeout、alert、console等等
+ 在Node中，我们也有一个global属性，并且看起来它里面有很多其他对象。
  - 但是在浏览器中执行的JavaScript代码，如果我们在顶级范围内通过var定义的一个属性，默认会被添加到window对象中
  - 而在node中, 定义了一个变量, 它只是在当前模块中有一个变量, 不会存放在全局中