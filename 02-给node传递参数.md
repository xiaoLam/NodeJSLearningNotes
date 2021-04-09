### Node的REPL
+ REPL就是Read Eval-Print Loop 的简称, 翻译为"读取-求值-输出-循环"
+ REPL是一个简单的, 交互式的编程环境
+ 事实上, 浏览器的console就可以看成一个REPL
+ Node也提供一个REPL环境, 我们可以在其中演练简单的代码

### Node程序传递参数
+ 正常情况下执行一个node程序, 通过 node + 对应的js文件 命令行即可
  - 比如: node index.js
+ 但是, 在某些情况下执行node程序的过程中, 我们希望给node传递一些参数
  - 比如: node index.js env=development xiaoLam
+ 如果我们通过上面的命令行来执行程序, 就意味着我们需要在程序中获取到传递过来的参数
  - 传递过来的参数其实是存放在process的内置全局对象中的
  - 如果我们直接打印这个内置全局对象, 就会发现它里面包含很多信息
  - 比如node的版本, 操作的系统等等
  - 我们传递过来的参数就存放在一个名为 argv的数组中
    + 这个数组里面就包含了我们传来的参数

### 为什么叫argv呢?
+ 在C/C++程序中的main函数中, 实际上可以获取到两个参数
  - argc: argument counter的缩写, 意为传递参数的个数
  - argv: argument vector的缩写, 意为传入的具体参数
    + vector翻译过来式矢量的意思, 在程序中表示的是一种数据结构
    + 在C++, Java中都有这种数据结构, 是一种数组结构
    + 在JavaScript中也是一个数组, 里面存储一些参数信息
``` js
console.log(process.argv);

process.argv.forEach(item => {
  console.log(item);
});
```

### Node的输出
+ console.log
  - 最常用的输出内容的方式: console.log
+ console.clear
  - 清空控制台的打印: console.clear
+ console.trace
  - 打印函数的调用栈: console.trace
+ 还有很多其他的console方法
  - https://nodejs.org/dist/latest-v14.x/docs/api/console.html


  