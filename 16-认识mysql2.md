### 认识mysql2
+ 前面我们所有的操作都是在GUI工具中, 通过SQL语句来获取结果的,真实开发中肯定是通过代码来完成所有的操作的
+ 如何在Node的代码中执行SQL语句呢? 这里我们可以借助于两个库
  - mysql: 最早的Node连接MySQL的数据库驱动
  - mysql2: 在mysql的基础之上, 进行了很多的优化 改进
+ 目前来说, 建议使用mysql2, 因为mysql2兼容了mysql的API, 并且提供了一些附加功能
  - 更快更好的性能
  - Prepared Statement(预编译语句)
    + 提高性能: 将创建的语句模块发送个MySQL, 然后MySQL编译(解析, 优化, 转换)语句模块, 并且存储它但是不执行, 之后我们在真正执行的时候会给MySQL提供实际的参数才会执行; 就算多次执行, 也只会编译一次, 所以性能是更好的
    + 支持Promise. 所以可以使用async和await语法
    + 等等

### 使用mysql2
+ 安装mysql2
  - npm install mysql2
+ mysql2的基本使用过程如下:
  - 第一步: 创建链接(通过createConection), 并且获取连接对象
  - 第二部: 执行SQL语句(通过query)即可
``` js
const mysql = require("mysql2");

// 1. 创建数据库连接
const connection = mysql.createConnection({
  host: 'localhost',
  port: 3306,
  database: 'coderhub',
  user: 'root',
  password: 'xiaoLam'
});

// 2. 执行SQL语句
const statement = `SELECT * FROM products WHERE price > 8000;`;
// 通过query的方式执行即可
connection.query(statement, (err, result, field) => {
  if (err) {
    console.log(err);
  }
  console.log(result);
})
```

### Prepared Statement(预编译语句)
+ Prepared Statement(预编译语句)
  - 提高性能: 将创建的语句模块发送给MySQL, 然后MySQL编译(解析 优化 转换)语句模块, 并且存储语句但是不执行, 之后再真正执行的时候给?提供实际的参数才会执行; 就算多次执行, 也只会编译一次, 所以性能是更高的
  - 放置SQL注入, 之后传入的值不会像模块引擎那样按照传入的MySQL语句编译, 那么一些SQL注入的内容就不会被执行; OR 1 = 1 不会被执行
+ 强调: 如果再次执行预编译语句, 它将会从LRU(Least Recently Used)(一个用于处理缓存的算法) Cache中获取, 省略了编译statement的时间来提高性能

### Connection Pools
+ 前面我们创建了一个连接(connection), 但是如果我们有多个请求的话, 该连接很有可能正在被占用, 那么我们是否需要每次一个请求都去创建一个新的连接呢?
  - 事实上, mysql2给我们提供了一个连接池(connection pools)
  - 连接池可以在需要的时候自动创建连接, 并且创建的连接不会被销毁, 会被放到连接池值, 后续是可以继续使用的
  - 我们可以在创建连接池的时候设置LIMIT, 也就是最大创建个数


``` js
const mysql = require('mysql2')

// 1. 创建连接池
const connection = mysql.createPool({
  host: 'localhost',
  port: 3306,
  database: 'coderhub',
  user: 'root',
  password: 'xiaoLam',
  // connectionLimit 表示最多可以建立多少个连接
  connectionLimit: 10
  // queueLimit 表示队列中的连接 一般不会设置
});

// 2. 使用连接池
const statement = `SELECT * FROM products WHERE price > ? AND score > ?;`

connection.execute(statement, [6000, 7], (err, result, field) => {
  console.log(result);
})
```

### Promise方式
+ 目前在JavaScript开发中中我们更习惯Promise和awiat, async的方式进行异步处理, mysql2同样也是支持的

``` js
const mysql = require('mysql2')

// 1. 创建连接池
const connection = mysql.createPool({
  host: 'localhost',
  port: 3306,
  database: 'coderhub',
  user: 'root',
  password: 'xiaoLam',
  // connectionLimit 表示最多可以建立多少个连接
  connectionLimit: 10
  // queueLimit 表示队列中的连接 一般不会设置
});

// 2. 使用连接池
const statement = `SELECT * FROM products WHERE price > ? AND score > ?;`

connection.promise().execute(statement, [6000, 7])
  .then(([result, fileds]) => {
    // 实质上返回的结果是一个数组, 里面包含了fileds的信息的
    // 这里对这个结果进行了一个解构
    console.log(result);
  })
  .catch(err => {
    console.log(err);
  })
```

