### 什么是Stream(流)
+ 什么是流?
  - 我们的第一反应应该是流水, 源源不断地流动
  - 程序中的流也是类似的概念, 我们可以想象在我们从一个文件中读取数据的时候, 文件的二进制(字节)数据会源源不断地被读取至程序中
  - 而这些一连串的字节, 就是程序中的流
+ 所以流
  - 是连续字节的一种表现形式和抽象概念
  - 流应该是可读的, 也是可写的
+ 在之前学习文件的读写的时候, 我们可以直接通过fs模块中的readFile函数和writeFile方式读写文件, 那为什么还需要流呢?
  - 如果直接通过fs模块中的函数来读写文件, 虽然是比较简单和方便, 但是这种方式来操作文件无法控制一些细节的操作
  - 比如从什么位置开始读取, 读取到什么位置, 一次性读取多少个字节
  - 读取到某个位置后, 暂停读取, 在某个时刻又恢复读取等等的操作
  - 又或者需要读取的文件非常大, 比如一个视频文件, 如果一次性全部读取的话是不合适的

### 文件读写的Stream
+ 事实上Node中很多对象是基于流实现的
  - http模块的Request和Response对象
  - process.stdout对象
+ Node官方: 另外所有的流都是EventEmitter的实例
+ Node.js中有四种基本流类型
  - Writable: 可以向其写入数据的流(例如: fs.createWriteStream())
  - Readable: 可以从中读取数据的流(例如: fs.createReadStream())
  - Duplex: 同时为Readable和Writable的流(例如: net.Socket)
  - Transform: Duplex可以在写入和读取数据时修改或转换数据的流(例如: zlib.createDeflate())

### Readable流
+ 之前我们读取一个文件的信息是通过fs模块中的readFile来实现的
+ 这种方式是一次性将一个文件中所有的内容全部都读取到程序(内存)中, 但是这种读取方式就会出现上面所说的问题
  - 文件过大, 读取的位置不能控制, 结束的位置不能控制, 一次读取的字节大小不能控制等
+ 这个时候, 我们可以使用fs模块中的createReadStream来实现可控地读取文件, createReadStream有以下几个参数
  - strat: 文件读取开始的位置
  - end: 文件读取结束的位置
  - highWaterMark: 一次性读取字节的长度, 默认是64kb

``` js
// 首先引入fs模块
const fs = require("fs");

// 使用readFile方法来读取文件
fs.readFile("./foo.txt" ,(err, data) => {
  if(err) {
    console.log(err);
  }

  console.log(data);
})

// 创建文件的Readable
const readable = fs.createReadStream("./foo.txt", {
  start: 3,
  end: 10,
  highWaterMark: 2
})

// 通过监听data事件来获取读取到的数据
readable.on("data", (data) => {
  console.log(data);

  // 通过pause()函数来暂停读取
  readable.pause()

  setTimeout(() => {
    // 通过resume()方法来恢复读取
    readable.resume()
  }, 1000)
})

// 通过监听open事件来接收文件打开事件
readable.on("open", () => {
  console.log("文件被打开了");
})

// 通过监听close事件来接收文件关闭事件
readable.on("close", () => {
  console.log("文件被关闭了");
})

// 通过监听end事件来接收读取结束事件
readable.on("end", () => {
  console.log("读取结束");
})
```

### Writable流
+ 之前我们写入一个文件的方式是通过fs模块中的writeFile实现的
+ 这种方式相当于一次性将所有内容写入到文件中, 但是这种方式也有很多问题
  - 比如不能控制写入内容的数量, 不能控制每次写入的位置等
+ 这个时候, 我们可以使用createWriteStream来实现可控地写入文件, createWriteStream有主要以下两个参数
  - flags: 设置写入的方式, 默认为w, 如果我们希望是追加写入, 可以使用a或者a+
  - start: 写入的位置
+ 注意
  - 写入流在打开后是不会自动关闭文件的
  - 我们必须手动调用close方法来关闭文件, 来告诉Node已经写入结束了
  - close会发出一个finish事件
  - 但是我们一般不会使用close方法, 更常用的是end方法: end方法相当于做了两步操作: write传入的数据和调用close方法
``` js
// 首先引入fs模块
const fs = require("fs");

// 以前写入文件的方式, 通过writeFile实现
// fs.writeFile("./bar.txt", "hello wrold", {}, (err) => {
//   if(err) {
//     console.log(err);
//   }
// })

// 使用createWriteStream的方式写入
// 首先创建文件的Writable
const writable = fs.createWriteStream("./bar.txt", {
  flags: "a+", // 这里的flags用于设置写入的方式
  start: 4 // start设置开始写入的位置
})

// 通过调用write方法来写入
writable.write("你好哇", (err) => {
  console.log(err);
  console.log("写入成功");
})

// 通过监听open事件来接收文件打开的事件
writable.on("open", () => {
  console.log("文件被打开了");
})

// 通过监听close事件来接收文件被关闭的事件
writable.on("close", () => {
  console.log("文件被关闭了");
})

// 写入操作的时候, 文件不会自动地关闭
// 而是要主动地调用 close()方法来关闭文件
// writable.close()

// 但是我们一般不会使用close来关闭文件
// 而是使用end()方法来关闭
// end()方法相当于做了两步操作, write写入传入的数据和调用close方法
writable.end("最终写入")
```

### pipe方法
+ pipe方法可以将读取到的数据存放在管道中