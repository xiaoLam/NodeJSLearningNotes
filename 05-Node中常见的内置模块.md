### 内置模块path
+ path模块是用于对路径的文件进行处理的模块, path提供了很多好用的方法
+ 需要知道, 在不同的操作系统中, 使用的文件分隔符是不一样的
  - 在window操作系统中使用的文件分隔符为 "/", "\" 和 "\\"
  - 而在Linux和mac os操作系统中使用的文件分隔符为 "/"
+ 如果我们在window上使用"\"分隔符开发了一个程序, 而要将这个程序部署在Linux上面
  - 此时文件的路径就会出现问题
  - 所以为了解决这个问题, 在开发中对路径的操作需要使用path模块来对文件的路径进行转换
+ 课外知识:
  - 为什么window操作系统和Linux操作系统使用的文件分隔符是不一样的?
    + 这是由于 Linux和mac os操作系统都实现了POSIX接口(可移植操作系统接口), 在这个规范中文件分隔符规定使用"/"
    + 而window操作系统在早期并没有实现POSIX接口, 就没有使用这个规范, 但是在现在新版本的window操作系统中已经支持了这个接口, 所以也可以使用"/"分隔符了

``` js
const path = require("path")

const basePath = "/User/xiaoLam"
const filename = "good.txt"

// 使用path模块中的 resolve接口来进行路径的拼接
const filePath = path.resolve(basePath, filename)

console.log(filePath); // C:\User\xiaoLam\good.txt
```


### path常见的API
+ 从路径中获取信息
  - dirname: 获取传入文件路径参数的父文件夹
  - basename: 获取文件名
  - extname: 获取文件的后缀名
+ 路径的拼接
  - 如果我们希望将多个路径进行拼接, 但是在不同的操作系统中使用的分隔符可能不一样(上面讲过)
  - 这个时候可以使用 path.json方法来对路径进行拼接
+ 将文件和某个文件夹拼接
  - 如果我们希望将某个文件和文件夹进行拼接, 可以使用path.resolve
  - resolve函数会判断传入的路径参数的前面是否以 / 或者 ./ 或者 // 开头
  - 如果有表示是一个绝对路径, 此时函数返回对应的拼接路径
  - 如果没有, 此时函数会将当前执行文件所在的文件夹的路径拼接在前面再进行返回
+ 一般来说resolve 比join 更加常用
+ 在webpack中获取路径或者起别名的地方都是使用resolve方法来实现的

### 内置模块fs
+ fs是file system的缩写, 表示文件系统
+ 对于任何一个为服务器端服务的语言或者框架通常都会有自己的文件系统
  - 因为服务器需要将各种数据, 文件等放置到不同的地方
  - 比如用户数据可能放到数据库中
  - 比如某些配置文件或者用户资源(图片, 音频视频等) 都是以文件的形式放到操作系统中
+ Node也有自己的文件系统操作模块, 就是fs
  - 借助于Node帮我们封装的文件系统, 我们可以在任何的操作系统(window, mac os, Linux)中直接去操作文件
  - 这也是Node可以开发服务器的一大原因, 也是它可以成为前端自动化脚本等热门工具的原因

### fs的API介绍
+ Node文件操作系统fs的API非常多
  - https://nodejs.org/dist/latest-v14.x/docs/api/fs.html
  - 不可能, 也没必要一个个学习
  - 更应该作为一个API查询手册, 等到用到的时候查询即可
  - 学习阶段只需学习最常用的API
+ Node中的fs的API大多数都提供了三种操作方式
  - 方式一: 同步操作文件, 这种方法操作文件会让代码阻塞, 后续的代码会在操作结束后继续执行
  - 方式二: 异步操作文件, 异步回调函数操作文件, 这种方式操作文件代码不会阻塞, 这种方法需要传入回调函数, 当获取到结果时, 回调函数会被执行
  - 方式三: 通过异步Promise操作文件, 这种方法操作文件代码不会阻塞, 通过fs.promises调用方法来操作, 会返回一个Promise对象, 通过then, catch进行处理返回的数据

### 文件描述符
+ 文件描述符(File descriptors)
  - 在POSIX系统中, 对于每一个进程, 内核都维护者一张当前打开着的文件和资源的表格
  - 每一个打开的文件都分配了一个称为文件描述符的简单的数字标识符
  - 在系统层, 所有文件系统操作都使用这些文件描述符来标识和跟踪每一个特定的文件
  - window系统使用了一个虽然不同但是概念上类似的机制来跟着资源
+ 为了简化用户的工作, Node.js抽象出操作系统之间的特定差异, 并为所有打开的文件分配了一个数字型的文件描述符

+ 通过fs模块中的open()方法来分配新的文件描述符
  - 一旦被分配, 则文件描述符可以用于从文件中读取数据, 向文件中写入数据, 或者请求关于文件的信息

``` js
const fs = require("fs")

// 使用fs中的open方法来获取文件描述符
// open方法需要传入两个参数, 第一个参数时文件的路径, 第二个参数时一个回调函数, 回调函数中的err为错误信息, fd为文件描述符
fs.open("./test.txt", (err, fd) => {
  if (err) {
    console.log(err);
    return
  }

  console.log(fd);

  // 通过文件描述符 fd 就可以获取到对应的文件
  // node中有对应的接口来通过文件描述符 fd 获取到对应的文件
  fs.fstat(fd, (err, info) => {
    console.log(info);
  })
})
```

### 文件的读写
+ 如果我们希望对文件的内容进行操作, 这个时候就可以使用fs模块中对文件读写的方法
  - fs.readFile(path[, options], callback): 用于读取文件的内容
  - fs.writeFile(path[, options], callback): 用于在文件中写入内容

+ [, options] 为可填写参数, 其作用是对读写进行一些设置, options参数一般有两项, flag属性和 encoding属性
  - flag属性设置写入数据的方式
  - encoding属性设置字符编码

### flag 选项
+ flag的值有很多: https://nodejs.org/dist/latest-v14.x/docs/api/fs.html#fs_file_system_flags
+ 常用的有以下几个
  - w 打开文件写入，默认值；
  - w+打开文件进行读写，如果不存在则创建文件；
  - r+ 打开文件进行读写，如果不存在那么抛出异常；
  - r打开文件读取，读取时的默认值；
  - a打开要写入的文件，将流放在文件末尾。如果不存在则创建文件；
  - a+打开文件以进行读写，将流放在文件末尾。如果不存在则创建文件

### encoding 选项
+ 关于字符编码的文章：https://www.jianshu.com/p/899e749be47c
+ 目前基本用的都是UTF-8编码
+ 注意: 在文件的读取操作, 即使用 fs.readFile() 方法的时候
  - 如果不填写encoding选项, 此时返回的结果是Buffer(二进制编码)
  - 所以一般使用该方法的时候都会设置encoding选项


### 文件夹操作
+ 新建一个文件夹
  - 使用fs.mkdir() 或者 fs.mkdirSync() 创建一个文件夹
  - 基本语法为 fs.mkdir(dirname, callback)
+ 获取文件夹中的内容
  - 使用 fs.readdir() 方法来获取文件夹中的内容
  - 基本语法为 fs.readdir(dirname[, options], callback)
+ 文件重命名
  - 使用 fs.rename() 方法来给文件重命名
  - 基本语法为 fs.rename(olddirname, newdirname, callback)

### events模块
+ Node中的核心API都是基于异步事件驱动的
  - 在这个体系中, 某些对象(发射器(Emitters)) 发出某一个事件
  - 我们可以监听这个事件(监听器Listeners), 并且传入回调函数, 这个回调函数会在监听到事件的时候调用
+ 发出事件和监听事件都是通过EventEmitter类来完成的, 它们都是属于events对象的
  - emitter.on(eventName, listener): 监听事件, 也可以使用addListener, addListener和on方法是完全一样的
  - emitter.off(eventName, listener): 移除事件监听, 也可以使用removeListener, 这两个也是完全一样的
  - emitter.emit(eventName[, ...args]): 发出事件, 可以携带参数


### events模块中常见的方法
+ 除了以上所用的常见方法外, 还有一些比较常见的方法
+ emitter.eventNames()：返回当前 EventEmitter对象注册的事件字符串数组；
+ emitter.getMaxListeners()：返回当前 EventEmitter对象的最大监听器数量，可以通过setMaxListeners()来修改，默认是10；
+ emitter.listenerCount(事件名称)：返回当前 EventEmitter对象某一个事件名称，监听器的个数；
+ emitter.listeners(事件名称)：返回当前 EventEmitter对象某个事件监听器上所有的监听器数组；

### events方法的补充(不太常用的)
+ emitter.once(eventName, listener)：事件监听一次
+ emitter.prependListener()：将监听事件添加到最前面
+ emitter.prependOnceListener()：将监听事件添加到最前面，但是只监听一次
+ emitter.removeAllListeners([eventName])：移除所有的监听器