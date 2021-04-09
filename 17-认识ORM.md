### 认识ROM
+ 对象关系映射(英语: Object Relational Mapping), 简称ORM, 或者O/RM mapping, 是一种程序设计的方法
  - 从效果来讲, 它提供了一个可在编程语言中, 使用虚拟对象数据库的效果
  - 比如在Java开发中经常使用的ORM包括: Hibernate, MyBatis
+ Node当中的ORM我们通常使用的是sequelize
  - sequelize是用于Postgres, MySQL, MariaDB, SQLite和Microsoft SQL Server的基于Node.js的ORM
+ 如果我们希望讲Sequelize和MySQL一起使用, 那么我们需要先安装两个东西
  - mysql2: sequelize在操作mysql是使用的是mysql2
  - sequelize: 使用它来让对象映射到表中

### Sequelize的使用 
+ Sequelize连接数据库
  - 第一步: 创建一个Sequelize的对象, 并且指定数据库 用户名 密码 数据库类型 主机地址等
  - 第二步: 使用authenticate方法来连接数据库
``` js
const {
  Sequelize
} = require("sequelize");

// new 一个Sequelize类, 并传入四个参数
// 第一个参数为需要连接的数据库名称
// 第二个参数为用户名
// 第三个参数为密码
// 第四个参数为一个对象, 里面填写相关的option
const sequelize = new Sequelize('coderhub', 'root', 'xiaoLam', {
  host: 'localhost',
  dialect: 'mysql'
});

// 通过authenticate方法连接启动连接, 该方法返回一个Promise对象, 所以可以通过then和catch获取返回的信息
sequelize.authenticate().then(() => {
  console.log("数据库连接成功");
}).catch((err) => {
  console.log('数据库连接失败', err);
})
```