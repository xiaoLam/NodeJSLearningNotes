### 什么是模块化
+ 什么是模块化开发?
  - 事实上模块化开发最终的目的是将程序划分为一个个小的结构
  - 这个结构中编写属于本身的逻辑代码, 有自己的作用域, 不会影响到其他的结构
  - 这个结构可以将自己希望暴露的变量, 函数, 对象等导出, 供给其他结构使用
  - 也可以通过某种方式, 导入其他结构中的变量, 函数, 对象等

+ 以上说到的结构, 就是模块; 按照这种结构划分开发程序的过程, 就是模块化开发的过程

### 早期的JavaScript
+ 早期的JavaScript有很多的缺陷
  - var定义的变量作用域问题;
    + 这个问题在ES6后, 通过使用let 和 const定义变量就解决了这个问题
  - JavaScript的面向对象并不能像常规面向对象语言一样使用class
    + 这个问题也在ES6后, 通过class关键字解决了
  - JavaScript没有模块化
    + 这个问题在ES6后, 也得到了完善
+ 现在的JavaScript的最紧急的问题是, 没有静态类型检测, 这也是为什么TypeScript发展起来的原因之一
+ 在网页开发的早期, Brendan Eich开发的JavaScript只是作为一种脚本语言, 做一些简单的表单验证和动画实现等, 那个时候JS的代码量是很少的
+ 但是随着前端和JavaScript的快速发展, JavaScript的代码量越来越多, 越来越复杂
  - ajax的出现, 前后端开发分离, 意味着后端返回数据后, 需要通过JavaScript进行前端页面的渲染
  - SPA(单页面富应用)的出现, 前端页面变得更加复杂: 包括前端路由, 页面状态管理等等一系列复杂的需求需要通过JavaScript实现
  - Node的出现, JavaScript可以编写复杂的后端程序了
+ 所以模块化是JavaScript一个非常迫切的需求
  - 但是JavaScript本身, 知道ES6(2015)才推出了自己的模块化方案
  - 那么在此之前, 为了让JavaScript支持模块化, 涌现了很多不同的模块化规范: AMD规范, CMD规范, CommenJS规范等

### 没有模块化带来很多的问题
+ 早期没有模块化带来了很多的问题: 比如命名冲突的问题
+ 我们也有方法去解决上面的问题: 立即函数调用表达式(IIFE), 本质上就是使用函数本身自带的作用域形成闭包来保护变量不泄露
  - IIFE(Immediately Invaoked Function Expression)
+ 但是这样做又带来了其他的问题
  - 首先: 我们必须记得每一个模块中返回对象的命名, 才能在其他模块中正确地使用
  - 第二: 代码变得混乱, 每一个文件中的代码都需要包裹在一个匿名函数中编写
  - 第三: 在没有合适的规范情况下, 每个人, 每个公司都有可能任意命名, 甚至出现模块名称相同的情况
+ 所以, 我们发现, 虽然实现了模块化, 但是我们使用IIFE的实现过于简单, 并且是没有规范的
  - 我们需要制定一定的规范来约束每个人都按照这个规范去编写模块化的代码
  - 这个规范中应该包含核心功能: 模块本身可以导出暴露的属性, 模块又可以导入自己需要的属性
  - JavaScript社区为了解决上面的问题, 涌现出一系列好用的规范, 比如CommonJS, AMD, CMD等

### CommonJS和Node
+ 我们需要知道CommonJS是一个规范, 最初设计出来是在浏览器以外的地方使用的, 并且当时被命名为ServerJS(意为主要使用在服务器端), 后来为了体现CommonJS的广泛性, 才将其改名为CommonJS, 平时也会简称CJS
  - Node是CommonJS在服务器端一个具有代表性的实现
  - Browserify是CommonJS在浏览器中的一种实现
  - Webpack打包工具具备对CommonJS的支持和转换
+ 所以, Node对CommonJS进行了支持和是西安, 让我们在开发node的过程中可以方便地进行模块化开发
  - 在Node中每一个js文件都是一个单独的模块
  - 这个模块中包含CommonJS规范的核心变量: exports, module.exports, require
  - 我们可以使用这些变量来方便的进行模块化开发
+ 前面我们提到过模块化的核心是导出和导入, Node对其进行了实现
  - exports和module.exports可以负责对模块中的内容进行实现
  - require函数可以帮助我们导入其他模块(包括自定义模块, 系统模块, 第三方库模块)中的内容

### Node中的CommonJS规范
+ 通过exports导出
  - 实际上, exports是一个对象, 我们可以在这个对象中添加很多个属性, 添加的属性就会被导出
+ 在另外一个文件中通过 require函数导入
  - require函数需要传入一个参数, 这个参数就是需要导入的变量所在模块的路径
  - 这个函数会返回一个对象, 这个对象本质上就是exports对象

``` js
// bar.js中的代码
const myName = "xiaoLam";
const age = 18;
function sayHello(name) {
  console.log("hello " + name);
}

// 通过Node实现的CommonJS规范来导出需要暴露的变量
// 通过 exports.属性名=属性值 的语法规范来导出

// 实质上exports是一个对象, 可以在这个对象中添加很多个属性, 添加的属性就会被导出
exports.myName = myName
exports.age = age
exports.sayHello = sayHello
```


``` js
// main.js中的代码
// 通过Node实现的CommonJS规范来导入需要的变量
// 通过require函数来导入, require函数会返回一个对象, 这个对象本质上就是exports对象
// require函数需要传入参数, 这个参数就是要导入的模块的路径
// 需要用一个变量来接收require函数返回的对象
// 这一句代码的意义实际上就是, const bar = exports
// 也就是说bar对象是exports对象的浅拷贝(引用赋值)
// 浅拷贝的本事就是一种引用赋值(也就是说修改bar.name的值, exports.name的值也会跟着改变)
const bar = require("./bar")

// 此时就可以通过bar对象访问这些暴露的变量
console.log(bar.myName);
console.log(bar.age);
bar.sayHello("kobe")
```

### module.exports又是什么?
+ 在Node中我们经常导出东西的时候, 是通过 module.exports导出的
  - module.exports和exports有什么关系或者区别吗?
+ 实际上CommonJS规范中是没有module.exports的概念的
  - 但是为了实现模块的导出, Node中使用的是Module的类, 每一个模块都是Module的一个实例, 也就是module
  - 所以在Node中真正用于导出的其实根本不是exports, 而是module.exports, 只是Node源码中做了一步, module.exports = exports 而已
  - 也就是说module才是导出的真正实现者
+ 但是, 为什么exports也可以导出呢? 
  - 因为module对象的exports属性是exports对象的一个引用
  - 也就是说 module.exports = exports = require()
+ 在三者相互引用的情况下, 修改exports中的属性值
  - 那么三者中对应的属性值都会发生改变, 因为这三者都是指向同一个内存地址的
+ 在三者相互引用的情况下, 修改require()函数返回的对象中的属性值
  - 那么三者中对应的属性值都会发生变化, 因为这三者都是指向同一个内存地址
+ 如果module.exports不再引用exports对象了, 这个时候修改exports中的属性值
  - 只有exports对象中的属性值会发生改变, 而module.exports和require()函数返回的对象中的属性值是不会发生改变的
+ 总结: require()函数和module.exports是必定关联在一起的, Node中实现的CommonJS规范, 本质是就是 module.exports导出, require()函数导入, 只不过Node内部做了一步操作, 就是 module.exports = exports, 让module.exports引用exports对象, 所以通过exports也可以导出, 但是当module.exports不再引用exports的时候, exports就不可以导出了

``` js
// Node中实现CommonJS规范, 本质上是通过 module.exports来导出的
// 只不过Node中做了一步操作 module.exports = exports , 所以通过exports也可以导出
console.log(module.exports == exports); // true

// 如果module.exports不再引用exports, 那么exports就不可以导出了
// 如下
module.exports = {
  myName : "jiaZhen",
  age : 123,
  sayHello(name) {
    console.log("你好 " + name);
  }
}
// 通过上面的代码操作就取消了 module.exports对exports的引用, 此时 require()函数导入的是 module.exports对象而不是 exports对象
// 而且, module.export 与 exports 不再指向同一个内存地址
console.log(module.exports == exports); // false
```

### exports存在的意义是什么?
+ 是因为CommonJS规范要求有一个exports作为导出
+ Node作出的妥协

### require函数的细节
+ require是一个函数, 可以帮助我们引入一个文件(模块)中导出的对象
+ require的查找规则是怎么样的?
+ require的查找规则有很多, 这里列举常见的查找规则: 导入的格式: require(X)
  - 情况一: X是一个核心模块, 比如path, http
    + 直接返回核心模块, 并停止查找
  - 情况二: X是以./或者../或者/(根目录)开头的
    + 第一步: 将X当作一个文件在对应的目录下查找
      - 如果X有后缀名, 按照后缀名的格式查找对应的文件
      - 如果X没有后缀名, 会按照以下顺序查找
        + 1>直接查找文件X
        + 2>查找X.js文件
        + 3>查找X.json文件
        + 4>查找X.node文件
    + 第二步: 没有找到对应的文件, 将X作为一个目录查找
      - 然后查找目录下的index文件
        + 1>查找X/index.js文件
        + 2>查找X/index.json文件
        + 3>查找X/index.node文件
    + 如果没有找到则报错: not found
  - 情况三: 直接是一个X(没有路径), 并且X不是一个核心模块
    - 此时会根据module对象中的paths数组中的路径一个一个从上往下查找对应路径中的 node_modules文件夹中的X/index.js文件
    - 如果没找到则报错: not found

### Node中模块的加载过程
+ 一: Node中的模块加载时同步的
+ 二: Node中的模块在被第一次引入的时候, 模块中的js代码就会被运行一次
+ 三: 模块被多次引入的时候会缓存, 最终只加载(运行)一次
  - 为什么会只加载运行一次呢?
  - 因为每一个模块对象module中都有一个属性: loaded
  - 当loaded为false的时候表示该模块还没有被加载过, 当模块被加载完毕后loaded的值会变为true, 表示该模块被加载过了
+ 四: 如果有循环引入, 加载的顺序时深度优先搜索加载

### CommonJS规范的缺点
+ CommonJS加载模块是同步的
  - 同步就意味着有阻塞, 只有等到对应的模块加载完毕后, 当前模块中的内容才能被运行
  - 这个问题如果在服务器端的话是不会有什么问题的, 因为服务器端加载的js文件都是本地文件, 加载速度非常快
+ 但是如果将CommonJS规范应用在浏览器端的话
  - 浏览器加载JS文件有时候是需要从服务器将文件先下载下来之后再加载运行的
  - 那么CommonJS的同步加载模块就意味着后续的js代码都无法正常运行, 即使是一些简单的DOM操作
+ 所以再浏览器中, 通常是不使用CommonJS规范的
  - 当然再webpack中使用CommonJS是另外一回事
  - 因为webpack会将代码转换为浏览器可以直接执行的代码(也就是说webpack会处理这个问题)
+ 在早期为了可以在浏览器中使用模块化, 通常会采用AMD或者CMD规范
  - 但是现在现代浏览器已经支持ES Modules, 另一方面借助于webpack等工具也是可以实现对CommonJS或者ES Modules代码的转换的
  - 所以现在AMD和CMD规范已经很少使用了

### AMD规范
+ AMD主要是应用于浏览器的一种模块化规范
  - AMD是Asynchronous Module Definition (异步模块定义)的缩写
  - AMD规范使用的是异步加载模块
  - 事实上AMD规范还要早于CommonJS规范
+ AMD规范实现常用的库是require.js和curl.js
+ 使用require.js来实现AMD规范的基本语法代码在03-AMD规范基本语法文件夹中

### CMD规范
+ CMD规范是应用于浏览器的一种模块化规范
  - CMD是Common Module Definition（通用模块定义）的缩写；
  - 它也采用了异步加载模块，但是它将CommonJS的优点吸收了过来；
  - 但是目前CMD使用也非常少了
+ CMD也有自己比较优秀的实现方案：
  - SeaJS
+ 使用sea.js来实现CMD规范的基本语法代码在04-CMD规范基本语法文件夹中

### 认识ES Module
+ JavaScript没有模块化一直是它的痛点, 所以才会产生我们前面学习的社区规范, CMD, AMD, commonJS等
+ ES Module和CommonJS的模块化有一些不同之处
  - 一方面它使用了 import 和export 关键字
  - 另一方面它采用编译期的静态分析, 并且加入了动态引用的方式
+ ES Module模块采用export和import关键字来实现模块化
  - export负责将模块内的内容导入
  - import负责从其他模块导入内容
+ 注意: 采用ES Module进行模块化将自动采用严格模式: use strict

### 使用ES Module实现模块化的注意事项
+ 如果之间将使用了ES Module实现模块化的html代码直接在浏览器中运行的话
  - 将会遇到CORS错误(跨域错误), 这是由于JavaScript模块的安全性需要
  - 此时需要使用一个服务器来测试我们的模块化代码
  - 这里我们使用的是VScode中的一个插件 Live Server, 这个插件可以开启一个服务器来运行我们的html代码, 并且可以根据代码改变来实现页面的热更新

### export关键字
+ export关键字将一个模块中的变量, 函数, 类等导出
+ 通过export关键字导出内容有以下三种方式
  - 方式一: 在语句声明的前面直接加上export关键字
  - 方式二: 将所有需要导出的标识符, 放到export后面的{}大括号(不是对象)中
    + 注意: 这里的{}里面不是ES6对象字面量的增强写法, {}也不是一个对象
    + 所以: export {name: name} 是错误的写法
    + 导出的是变量的引用, 而不是变量的值
  - 方式三: 导出的时候给标识符起一个别名

### import关键字
+ import关键字负责从另外一个模块中导入内容
+ 导入内容的方式常用的有三种方式
  - 方式一: import {标识符列表} form "模块路径"
    + 注意: 这里的{}也不是一个对象, 这个大括号里面只是存放导入的标识符列表内容
  - 方式二: 导入时给标识符起别名
  - 方式三: 通过 * 将模块功能放到一个模块功能对象 (a module object) 中

### export和import 结合使用
+ export 和import 是可以结合使用的
  - export {} from "模块的路径"

+ 这样做的意义
  - 在开发和封装一个功能库的时候, 通常我们希望将暴露的所有接口都放在一个文件中
  - 这样方便指定统一的接口规范, 也方便阅读
  - 这个时候, 就可以使用export和import结合来使用了

### default的用法
+ 前面学习的导出方式都是有名字的导出 (named exports)
  - 在导出export的时候指定了名字
  - 在导入import的时候也需要知道具体的名字
+ 还有一种导出的方式称为 默认导出 (default exports)
  - 默认导出 export 的时候可以不需要指定名字
  - 在导入的时候不需要使用 {}, 并且可以自己来指定名字
  - 它也方便我们和现有的CommonJS等规范相互操作
+ 注意: 在一个模块中, 只能有一个默认导出 (default export)

### import函数
+ 通过import关键字来加载一个模块的时候, 是不可以将其放到逻辑代码中的
  - 这是因为ES Module在被JS引擎解析的时候, 就必须知道它的依赖关系
  - 由于这个时候JS代码还没有任何的运行, 所以是无法根据代码逻辑来判断是否加载某一个模块的
+ 但是在某种情况下, 确实希望动态地加载某一个模块
  - 此时就需要使用import()函数来动态加载了
  - import函数在JS代码中是异步加载的
  - import函数会返回一个promise对象, 所以可以通过then来获取到需要导入的内容

### CommonJS的加载过程
+ CommonJS模块加载JS文件的过程是运行时加载的, 并且是同步的
  - 运行时加载意味着是js引擎在执行js代码过程中加载模块的, 所以可以将引入模块的代码写到逻辑代码里面
  - 同步加载意味着一个文件没有加载结束之前, 后面的代码是不会继续执行的 (阻塞)

+ CommonJS通过module.exports导出的是一个对象
  - 导出的是一个对象意味着可以将这个对象的引用在其他模块中赋值给其他变量
  - 但是最终他们共同指向的是同一个对象, 如果修改了对象中的属性值, 那么其他地方的属性值也会跟着改变

### ES Module的加载过程
+ ES Module加载js文件的过程是编译(解析)时加载的, 并且是异步的
  - 编译时加载, 意味着import不能和运行时相关的内容写在一起, 也就是说不可以将导入模块的代码写到逻辑代码中, 以下情况都不可以
  - 比如from后面的路径需要动态获取的时候
  - 比如将import写到if等逻辑语句中
  - 所以我们有时候也称ES Module是静态解析的, 而不是动态或者运行时解析的
+ 异步意味着: JS引擎在执行代码的过程中遇到import的时候会去获取这个js文件, 但是这个获取过程是异步的, 并且不会阻塞主线程的继续执行
  - 也就是说设置了 type = module 的script标签, 相当于给该标签加上了async属性
  - 如果该标签后面有普通的script标签以及对应的代码, 那么ES Module对应的js文件和代码不会阻塞它们的执行
+ ES Module通过export导出的是变量本身的引用
  - export在导出一个变量的时候, js引擎会解析这个语法, 并且创建一个称为 模块环境记录(module environment record) 的一块内存
  - 在 模块环境记录 中会将变量进行绑定(binding), 而且这个绑定是实时绑定的(时刻绑定最新的变量)
  - 而在导入的地方, 可以实时地获取到绑定地最新值
+ 所以, 如果在导出的模块中修改了变量, 那么导入地地方可以实时获取到最新的变量
  - 注意: 在导入的地方不可以修改获取的变量, 因为它只是绑定到这个变量上, 但是这个变量的绑定实质上是用const 来绑定的
+ 思考: 如果导出的是一个对象, 那么在引入的地方去修改这个对象中的属性值, 会成功吗?
  - 会成功的, 因为导出的是一个对象的话, 说明导出的是这个对象的内存地址, 而在引入的地方去修改这个对象中的属性值这个操作, 实际上是根据这个内存地址去找到对应的属性值去修改, 也就是说 导出的对象 和 引入的对象实际上指向的是同一块内存地址, 所以会成功

``` js
// 在纯使用ES Module的情况下实现模块化
// 在通过import 来加载一个模块的时候, 是不可以将其放入逻辑代码中的
// 比如:
// const flag = true;
// if (flag) {
//   import {name, age} from "./modules/foo.js" // 这里浏览器会报语法错误
// }
// 出现这种情况的原因是因为, ES Module在被JS引擎解析的时候, 就必须知道模块与模块之间的依赖关系
// 而这个时候JS代码还没有实际运行起来, 所以是无法根据代码逻辑来判断是否加载某一个模块的


// 但是如果在某些情况下, 确实需要动态来加载某个模块的话
const flag = true
if (true) {
  // 第一种方式
  // 如果, 当前环境有webpack打包工具的话, 可以使用commonJS中的require来实现
  // const data = require("路径")

  // 第二种方式:
  // 如果, 是纯JS中的ES Module来实现模块化的话
  // 可以使用 import()函数来实现
  import("./modules/foo.js").then(res => {
    console.log("import()函数中then中的打印");
    console.log(name);
    console.log(age);
  }).catch(err => {})

  // impor函数在JS中是异步调用的
  // 其返回值是一个promise对象
}
```

### Node对ES Module的支持
+ 在最新的Current的Node版本中(14.13.1), 支持es module需要进行如下操作
  - 方式一: 在package.json中配置type属性值为module(后续会学习, 现在还没有讲到package.json文件的作用, 其实这个文件的作用就是对项目文件的配置)
  - 方式二: 在需要当作模块的js文件以.mjs为后缀, 表示使用ES Module模块化

### CommonJS和ES Module的相互引用
+ 结论一: 通常情况下, CommonJS是不能加载ES Module的导出内容的
  - 因为CommonJS是同步加载的, 但是ES Module是必须经过静态分析等操作的, 在ES Module的静态操作的过程中是不会执行JavaScript代码的
  - 但是这个结论并不是绝对的, 在某些平台中是可以对代码进行针对性的解析的
  - 在Node平台中式不支持的

+ 结论二: 多数情况下, ES Module式可以加载CommonJS导出的内容的
  - ES Module在加载CommonJS导出的内容的时候, 会将module.exports导出的内容作为default导出的方式来使用
  - 这个依然需要看具体的实现, 比如在webpack中是支持的, Node的最新版本也是支持的
  - 在较早的Node版本中是不支持的

