### 为什么需要数据库
+ 任何软件系统都需要存放大量的数据, 这些数据通常都是非常复杂和庞大的
  - 比如用户信息包括姓名、年龄、性别、地址
  - 比如商品信息包括商品名称、描述、价格、分类标签、商品图片等
  - 比如歌曲信息包括歌曲名称、歌手、专辑、歌曲时长、歌词、封面等
+ 这些信息如果直接存储到文件系统中可以吗？可以，但是文件系统有很多缺点
  - 很难以合适的方式组织数据(多张表之前的关系合理组织)
  - 并且对数据进行增删改查中的复杂操作(虽然一些简单确实可以), 并且保证操作的原子性
  - 很难实现数据共享, 比如一个数据库需要为多个程序服务
  - 需要考虑如何进行数据的高效备份 迁移 恢复
+ 数据库通俗来说就是一个存储数据的仓库, 本质上就是一个软件 一个程序

### 常见的数据库有哪些
+ 通常我们将数据库划分为两类: 关系型数据库和非关系型数据库
+ 关系型数据库: MySQL、Qracle、DB2、SQL Server、Postgre SQL等
  - 关系型数据库通常我们会创建很多个二维数据表
  - 数据表之间相互关联起来, 形成一对一 一对多 多对多等关系
  - 之后可以利用SQL语句再多张表中查询我们所需的数据
  - 支持事务, 对数据的访问更加的安全
+ 非关系型数据库:MongoDB、Redis、Memcached、HBse等
  - 非关系型数据库的英文其实是Not Only SQL, 简称为NoSQL
  - 相对而言非关系型数据库比较简单一些, 存储数据也会更加自由(甚至我们可以直接将一个复杂的json对象直接塞入数据库中)
  - NoSQL是基于Key-Value的对应关系, 而且查询的过程中不需要进过SQL解析, 所以性能更高
+ 如何在开发中选择数据库? 具体的选择会根据不同的项目进行综合的分析
  - 一般来说, 目前再公司进行后端开发(Node, Java, Go等), 还是以关系型数据库为主
  - 比较常用的用到非关系型数据的情况是再爬取大量的数据需要进行存储的时候会比较常见

### 认识MySQL
+ MySQL的介绍:
  - MySQL原本是一个开源的数据库, 原开发者为瑞典的MySQL AB公司
  - 再2008年被Sun公司收购; 再2009年, Sun被Oracle收购
  - 所以目前MySQL归属于Oracle(甲骨文)
+ MySQL是一个关系型数据库, 其实本质上就是一款软件, 一个程序
  - 这个程序中管理着多个数据库
  - 每个数据库中可以有多张表
  - 每个表中可以有多条数据

### MySQL的连接操作
+ 打开终端, 查看MySQL的安装
  - mysql --version
  - 如果mySQL安装成功后, 会出现mySQL的版本信息
+ 如果我们想要操作数据, 需要先和数据库建立链接, 最直接的方式就是通过终端来连接
+ 有两种方式来连接
  - 两种方式的区别在于输入密码一个是直接输入, 一个是另起一行以密文的形式输入
  - 方式一: 终端命令行输入 mysql -uroot -p密码
  - 方式二: 终端命令行输入 mysql -uroot -p 回车, 另起一行输入密码 然后回车
  - 注意在git bash中如果出现 输入连接命令却没有反应的情况下, 在命令行前添加 winpty即可解决

### 显示数据库
+ 一个数据库软件中, 可以包含很多个数据库
  - 命令行输入 show databases; 即可查看数据库, 注意分号;也是必须填入的 
+ 此时我们可以看到MySQL默认帮我们创建了几个数据库
  - infomation_schema: 信息数据库, 其中包括MySQL在维护的其他数据库, 表列, 访问权限等信息
  - performance_schema: 性能数据库, 记录着MySQL_Server数据库引擎在运行过程中的一些资源消耗相关的信息
  - mysql: 用于存储数据库管理者的用户信息 权限信息 以及一些日志信息等
  - sys: 相当于是一个简易版的performance_schema, 将性能数据库中的数据汇总成更容易理解的形式

### 创建数据库-表
+ 在终端中直接创建一个属于自己的新的数据库(一般情况下一个新的项目会对应一个新的数据库)
  - create database 数据库名;
+ 使用指定的数据库
  - use 数据库名;
+ 在数据库中, 创建一张表
  - create table user(name varchar (20), age int, height double);
+ 在表中插入数据
  - insert into user (name, age, height) values ("xiaoLam", 18, 1.88);

### GUI工具的介绍
+ 我们会发现在终端操作数据库有很多不方便的地方
  - 语句写出来没有高亮, 并且不会有任何提示
  - 复杂的语句分成多行, 格式看起来不美观, 很容易出现错误
  - 终端中查看所有的数据库或者表非常的不直观和不方便
  - 等等
+ 所以在开发中, 我们可以借助于一些GUI工具来帮助我们连接上数据库, 之后直接在GUI工具中操作就很方便了
+ 常见的MySQL的GUI工具有很多
  - Navicat: 最好用的, 但是收费
  - SQLYog: 免费, 但是界面很丑
  - TablePlus: 常用的功能都可以使用, 但是会多一些限制(比如只能开两个标签页)

### 认识SQL语句
+ 我们希望操作数据库(特别是在程序中操作), 就需要又和数据库沟通的语言, 这个语言就是SQL
  - SQL是Structured Query Language, 称之为结构化查询语言, 简称SQL
  - 使用SQL编写出来的语句, 就成为SQL语句
  - SQL语句可以用于对数据库进行操作
+ 事实上, 常见的关系型数据库SQL语句都是比较相似的, 所以学习了MySQL中的SQL语句, 之后去操作其他关系型数据库也是非常简单的
+ SQL语句的常用规范
  - 通常关键字是大写的, 比如CREATE, TABLE, SHOW等等
  - 一条语句结束后, 需要以分号; 结尾, 否则语句不会结束
  - 如果遇到关键字作为表明或者字段名称, 可以使用反引号 `` 包裹

### SQL语句的分类
+ 常见的SQL语句我们可以分成四类
+ DDL(Data Definition Language): 数据定义语言;
  - 可以通过DDL语句对数据库或者表进行
+ DML(Data Manipulation Language): 数据操作语言
  - 可以通过DML语句对表进行: 添加, 删除, 修改等操作
+ DQL(Data Query Language): 数据查询语言
  - 可以通过DQL从数据中查询记录;(重点)
+ DCL(Data Control Language): 数据控制语言
  - 对数据库, 表格的权限进行相关访问控制操作

### 数据库的操作
+ 查看当前数据库
  - 查看所有的数据: SHOW DATABASES;
  - 使用某一个数据: USE 数据库名称;
  - 查看当前正在使用的数据库: SELECT DATABASE();
+ 创建新的数据库
  - 创建数据库: CREATE DATABASE 数据库名称;
  - 一般来说我们不会以上面的方式创建数据库, 因为如果已经存在了同名的数据库, 会报错, 甚至程序崩溃
  - 一般用更加严谨的方式创建: CREATE DATABASE IF NOT EXISTS 数据库名称;
  - 如果想要创建数据库的同时设置数据库的字符集和排序规则: CREATE DATABASE IF NOT EXISTS 数据库名称 DEFAULT CHARACTER SET 字符集 COLLATE 排序规则;
  - 注: 排序规则指, 如果在数据库查询的时候, 排序规则规定了查询结果的显示顺序, ai和as指是否区分英文轻重音, ai指不区分; ci和cs指是否区分英文大小写, ci指不区分; 一般使用默认即可
+ 删除数据库
  - 删除数据库: DROP DATABASE 数据库名称;
  - 更加严谨的写法: DROP DATABASE IF EXISTS 数据库名称;
+ 修改数据库(一般只修改数据库的字符集和排序规则)
  - ALTER DATABASE 数据库名称 CHARACTER SET = 字符集 COLLATE = 排序规则;

### SQL的数据类型-数字类型
+ 我们知道不同的数据会划分不同的数据类型, 在数据库中也是一样的
  - MySQL支持的数据类型又: 数字类型, 日期和时间类型, 字符串(字符和字节)类型, 空间类型(存储一个点的坐标, 经度纬度的时候会使用, 一般不会使用)和JSON数据类型(一般不会将一个JSON对象存储在数据库中, 不常用)
+ 数字类型
  - MySQL的数字类型有很多
  - 整数数字类型: INTEGER, INT(最常用), SMALLINT, TINYINT, MEDLUMINT, BIGINT
  - 浮点数字类型: FLOAT, DOUBLE(常用), FLOAT是四个字节, 而DOUBLE是八个字节
  - 精确数字类型: DECIMAL, NUMERIC(DECIMAL是NUMERIC的实现形式), 这两个的使用方法和作用基本一样, 一般常用DECIMAL, 使用方法为传入两个参数, 第一个参数表示数字的长度, 第二个参数为保存的小数个数

### SQL的数据类型-日期类型
+ MySQL的日期类型也很多
+ YEAR以YYY格式显示值
  - 范围1901到2155, 和0000
+ DATE类型用于具有日期部分但是没有时间部分的值
  - DATE以格式YYYY-MM-DD显示值
  - 支持的范围是'1000-01-01'到'9999-12-31'
+ DATETIME类型用于包含日期和时间部分的值
  - DATETIME以格式'YYYY-MM-DD hh:mm:ss'显示值
  - 支持的范围是1000-01-01 00:00:00到9999-12-31 23:59:59
+ TIMESTAMP数据类型被用于同时包含日期和时间部分的值(一般用这个)
  - TIMESTAMP以格式'YYYY-MM-DD hh:mm:ss'显示值；
  - 但是它的范围是UTC的时间范围：'1970-01-01 00:00:01'到'2038-01-19 03:14:07'
  - 与DATETIME的区别只是范围不一样
+ 另外：DATETIME或TIMESTAMP 值可以包括在高达微秒（6位）精度的后小数秒一部分
  - 比如DATETIME表示的范围可以是'1000-01-01 00:00:00.000000'到'9999-12-31 23:59:59.999999'

### SQL的数据类型-字符串类型
+ MySQL的字符串类型表示方式如下
+ CHAR类型在创建表时为固定长度, 长度看也是0到255之间的任何值
  - 在被查询时, 会删除后面的空格
+ VARCHAR类型的值是可变长度的字符串, 长度可以指定为0到65535之间的值
  - 在被查询时, 不会删除后面的空格
  - 一般使用VARCHAR这个类型
+ BINARY和VARBINARY类型用于存储二进制字符串, 存储的时字节字符串;
  - 一般来说不会将二进制的数据(比如图片数据)存储在数据库中, 一般时将这种数据存储在文件系统中, 然后根据文件系统的存储位置URL存储在数据库中
+ BLOB用于存储大的二进制类型
+ TEXT用于存储大的字符串类型

### 表约束
+ 主键: PRIMARY KEY
  - 一张表中, 为了区分每一条记录的唯一性, 必须有一个字段时永远不会重复, 而且不能为空的, 这个字段我们通常会将其设置为主键
  - 主键时表中唯一的所以
  - 而且必须时NOT NULL的, 如果没有设置NOT NULL, 那么MySQL也会隐式地设置为NOT NULL
  - 主键也可以多列索引, PRIMARY KEY(key...pary, ...) 我们一般称之为联合主键
  - 建议: 在开发中主键字段一个和业务无关, 尽量不要使用业务字段来作为主键
+ 唯一: UNIQUE
  - 某些字段在开发中我们希望是唯一地, 不会重复地, 比如手机号码, 身份证号码等, 这些字段我们可以使用UNIQUE来约束
  - 使用UNIQUE约束地字段在表中必须是不同的
  - 对于所有引擎, UNIQUE索引允许NULL包含的列具有多个NULL值
+ 不能为控: NOT NULL
  - 某些字段我们要求用户必须插入值, 不可以为空, 这个时候我们可以使用NOT NULL来约束
+ 默认值: DEFAULT
  - 某些字段我们希望在没有设置时给予一个默认值, 这个时候我们可以使用DEFAULT来完成
+ 自动递增: AUTO_INCREMENT
  - 某些字段我们希望不设置值的时候可以进行递增, 比如用户的ID, 这个时候可以使用AUTO_INCREMENT来完成
+ 外键约束也是最常用的一种约束手段

### 创建一个完整的表
``` SQL
CREATE TABLE IF NOT EXISTS `users` {
  id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(20) NOT NULL,
  age INT DEFAULT 0,
  telPhone VARCHAR(20) DEFAULT '' UNIQUE NOT NULL,
  createTime TIMESTAMP
}
```

### 删除表
``` SQL
DROP TABLE users;
-- 更加严谨的写法
DROP TABLE IF EXISTS users;
```

### 表操作
``` SQL
# 修改表名
ALTER TABLE users RENAME TO user;

# 添加一个新的列
ALTER TABLE user ADD updateTime TIMESTAMP;

# 修改字段的名称
ALTER TABLE user CHANEG phoneNum telePhone VARCHAR(20) UNIQUE NOT NULL;

# 修改字段的类型
ALTER TABLE user MODIFY name VARCHAR(30);

# 删除某一个字段
ALTER TABLE user DROP age;

# 补充
# 根据一个表结构去创建另外一个表
CREATE TABLE user1 LIKE user;

# 根据一个表中的所有内容去创建另外一个表
CREATE TABLE user2 (SELECT * FROM user);

# 注意以上两种创建表的方式不会包含主键等的设置
```

### DML 数据操作语句
+ DML: Date Manipulation Language (数据操作语句)
``` SQL
# 插入数据
INSERT INTO user VALUES (按照数据项传入的数据);
INSERT INTO user (需要填入的数据项) VALUES (按照顺序插入的顺序);

# 需求: 在插入日期数据的时候, 数据库可以按照当前时间插入默认值
# DEFAULT 指在添加数据的时候会自动设置默认值
ALTER TABLE user MODIFY createTime TIMESTAMP DEFAULT CURRENT_TIMESTAMP;

# ON UPDATE 指在数据更新的时候会自动设置当前时间
ALTER TABLE user MODIFY updateTime TIMESTAMP ON UPDATE CURRENT_TIMESTAMP;

# 如果想要既在添加数据的时候设置默认值, 并且在更新数据的时候也会更新当前时间, 则如下设置
ALTER TABLE user MODIFY updateTime TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP;


# 删除数据
# 删除所有的数据(一般来说不会这样操作)
DELETE FROM user;
# 删除符合条件的数据
DELETE FORM user WHERE id = 110;

# 更新数据
# 更新所有的数据(一般来说不会这样操作)
UPDATE user SET name = 'lily', telePhone = '1011121';
# 更新符合条件的数据
UPDATE user SET name = 'lily', telePhone = '1011121' WHERE id = 110;
```

### DQL查询操作语句
+ DQL: Data Query Language(数据查询语言)
  - SELECT用于从一个或者多个表中检索选中的行(Record)
``` SQL
# 1.基本的查询操作
# 查询表中所有字段以及所有的数据
# * 号表示通配符, 选择所有字段
SELECT * FROM products;
# 查询特定字段的数据
SELECT title, price FROM products;
# 对字段结果起别名
# AS 后面跟想要起的别名
SELECT title AS phoneTitle, price AS currentPrice FROM procucts;

# 2. WHERE条件
# 2.1 条件判断语句
# 案例: 查询价格小于1000的手机
SELECT * FROM products WHERE price < 1000;
# 案例二: 价格等于999的手机
SELECT * FROM products WHERE price = 999;
# 案例三: 价格不等于999的手机
SELECT * FROM products WHERE price != 999;
SELECT * FROM products WHERE price <> 999;
# 案例四: 查询品牌是华为的手机
SELECT * FROM products WHERE brand = '华为';

# 2.2 逻辑运算语句
# 案例一: 查询价格1000到2000之间的手机
SELECT * FROM products WHERE price > 1000 AND price < 2000;
SELECT * FROM products WHERE price > 1000 && price < 2000;
# 注意使用 BETWEEN AND 关键字是包含等于的
SELECT * FROM products WHERE price BETWEEN 1000 AND 2000;

# 案例二: 查询价格在5000以上或者品牌是华为的手机
SELECT * FROM products WHERE price > 5000 OR brand = '华为';
SELECT * FROM products WHERE price > 5000 || brand = '华为';

# 查询url值为NULL的数据
SELECT * FROM products WHERE url IS NULL;
# 查询url值不为NULL的数据
SELECT * FROM products WHERE url IS NOT NULL;

# 2.3. 模糊查询
# % 百分号表示匹配任意个的任意字符；
# _ 下划线表示匹配一个的任意字符
# 查询title中有M的数据
SELECT * FROM products WHERE title LIKE '%M%';
# 查询title中第二个字符为P的数据
SELECT * FROM products WHERE title LIKE '_P%';
# 查询title中第三个字符为P的数据
SELECT * FROM products WHERE title LIKE '__P%';

# 2.4. IN表示取多个值中的其中一个即可
# 查询品牌为华为或者小米或者苹果的手机
SELECT * FROM products WHERE brand = '华为' || brand = '小米' || brand = '苹果';
SELECT * FROM products WHERE brand IN ('华为', '小米', '苹果');

# 3. 对结果进行排序
# 使用 ORDER BY 关键字, ASC 表示升序, DESC表示降序
SELECT * FROM products WHERE brand IN ('华为', '小米', '苹果') ORDER BY price ASC, score DESC;

# 4. 分页查询
# 使用LIMIT关键字, 表示获取多少条, OFFSET关键字, 表示从哪一条开始获取
SELECT * FROM products LIMIT 20 OFFSET 0;
# 另外一种写法, LIMIT offset, limit;
SELECT * FROM products LIMIT 0, 20;
```

### 聚合函数
+ 什么是聚合函数
  - 聚合函数指的是, 将一张表作为一组数据, 当我们想对这一组数据进行某些操作(比如求和, 求平均值, 求最大最小值)的时候所使用的方法就是聚合函数
``` SQL
# 1.聚合函数的使用
# 1.1 求所有手机的价格总和
SELECT SUM(price) FROM products;
# 当然也可以给结果取别名
SELECT SUM(price) AS totalPrice FROM products;
# 求符合特定条件的数据的手机价格总和
SELECT SUM(price) FROM products WHERE brand = '华为';
# 求华为手机的平均价格
SELECT AVG(price) FROM products WHERE brand = '华为';
# 求手机的最高和最低价格
SELECT MAX(price) FROM products;
SELECT MIN(price) FROM products;

# 求手机的个数
# * 号通配符, 表示所有的手机
SELECT COUNT(*) FROM products;
# 求品牌为华为的所有手机的个数
SELECT COUNT(*) FROM products WHERE brand = '华为';
# 求具有url的手机个数
SELECT COUNT(url) FROM products;

# 求具有price的手机的个数
SELECT COUNT(price) FROM products;
# 如果想要去重则使用关键字DISTINCT
SELECT COUNT(DISTINCT price) FROM products;

# 2.GROUP BY的使用
# GROUP BY的作用是分组
# 将所有手机按照品牌分组
SELECT * FROM products GROUP BY brand;
# 将所有手机的平均值和数量和平均分按照品牌分组
SELECT brand, AVG(price), COUNT(*), AVG(score) FROM products GROUP BY brand;
# HAVING的使用
SELECT brand, AVG(price) avgPrice, COUNT(*), AVG(score) FROM products GROUP BY brand HAVING avgPrice > 2000;

# 3.需求: 求评分score > 7.5 的手机的, 平均价格是多少
SELECT brand, AVG(price) FROM products WHERE score > 7.5 GROUP BY brand;
```

### 多表的设计
+ 假如我们的上面的商品表中, 对应的品牌还需要包含其他的信息
  - 比如品牌的官网, 品牌的世界排名, 品牌的市值等等
+ 如果我们直接在商品中添加一个字段取存储品牌相关的信息, 会存在一些问题
  - 一方面, products表中应该表示的都是商品相关的数据, 应该有另外的一张表取表示brand的数据
  - 另一方面, 多个商品使用的品牌是一致的时候, 会存在大量的冗余数据
+ 所以, 我们可以将所有的批评数据, 单独放到一张表中, 然后通过外键将两张表关联在一起

``` SQL

# 给products添加一个brand_id字段
ALTER TABLE products ADD brand_id INT;

# 设置外键限制: 外键限制指的是限制值只能为引用的值
# 修改products的brand_id的字段外键为brand中的id
ALTER TABLE products ADD FOREIGN KEY(brand_id) REFERENCES brand(id);

# 设置brand_id的值
UPDATE products SET brand_id = 1 WHERE brand = '华为';
```

### 外键存在的时候更新和删除数据
+ 我们来思考一个问题
  - 如果products中引用的外键被更新了或者删除了, 此时会出现什么情况呢?
+ 使用SQL语句进行一个更新操作
  - UPDATE brand SET id = 100 WHERE id = 1;
+ 执行代码会报错
  - 这是因为外键有默认的action设置为了RESTRICT, 此时就不可以更新和删除关联的外键

### 如何进行更新呢?
+ 如果我们希望可以更新和删除关联的外键, 我们需要修改on delete 或者 on update的值
+ 我们可以给更新或者删除时设置几个值
  - RESTRIC(默认属性): 当更新或者删除某个关联外键的时候, 会检查该记录是否有关联的外键记录, 有的话就会报错, 不允许更新或者删除
  - NO ACTION: 和RESTRIC是一致的, 在SQL标准中定义的
  - CASCADE: 当更新或者删除某个记录的时候, 会检查该记录是否有关联的外键记录, 有的话:
    + 更新: 会更新对应的记录
    + 删除: 会将关联的记录一起删除(一般来说删除的时候不会设置为CASCADE值,)
  - SET NULL: 当更新或者删除某个记录的时候, 会检查该记录是否有关联的外键记录, 有的话, 将对应的值设置为NULL

``` SQL
# 3.修改和删除外键引用的id
# 不可以直接修改和删除被引用的外键
UPDATE brand SET id = 100 WHERE id = 1;
ALTER TABLE brand DROP id;
# 原因是因为关联外键的action被设置为RESTRICT, 此时是不可以修改和删除被引用的外键的

# 4.修改关联外键的action
# 要经过三步才可以修改, 本质是先删除原本的外键, 然后在重新设置外键的时候设置对应的action
# 4.1 获取到目前的外键的名称
SHOW CREATE TABLE products;

-- CREATE TABLE `products` (
--   `id` int NOT NULL AUTO_INCREMENT,
--   `brand` varchar(20) DEFAULT NULL,
--   `title` varchar(100) NOT NULL,
--   `price` double NOT NULL,
--   `score` decimal(2,1) DEFAULT NULL,
--   `voteCnt` int DEFAULT NULL,
--   `url` varchar(100) DEFAULT NULL,
--   `pid` int DEFAULT NULL,
--   `brand_id` int DEFAULT NULL,
--   PRIMARY KEY (`id`),
--   KEY `brand_id` (`brand_id`),
--   CONSTRAINT `products_ibfk_1` FOREIGN KEY (`brand_id`) REFERENCES `brand` (`id`)
-- ) ENGINE=InnoDB AUTO_INCREMENT=109 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci

# 4.2 根据获取到的外键的名称删除外键
ALTER TABLE products DROP FOREIGN KEY products_ibfk_1;

# 4.3 重新添加外键的同时设置对应的action
ALTER TABLE products ADD FOREIGN KEY (brand_id) REFERENCES brand(id) ON UPDATE CASCADE ON DELETE RESTRICT;

# 此时修改关联的外键就可以正常修改了
UPDATE brand SET id = 100 WHERE id = 1;
```

### 多表查询
+ 什么是多表查询
  - 如果我们希望查询到产品的同时, 显示对应的品牌相关的信息, 因为数据是存放在两张表中的, 所以这个时候就需要进行多表查询
+ 如果我们直接通过查询语句希望在多张表中查询到数据, 使用下列SQL语句会产生什么效果呢?
  - SELECT * FROM products, brand;
+ 会发现结果的数量为两张表数据数量的乘积
+ 这个结果我们称为笛卡尔乘积, 也称为直积, 表示为X*YEAR以YYY格式显示值
+ 但是事实上很多的数据是没有意义的, 所以我们要对笛卡尔乘积进行筛选
  - 使用WHERE进行筛选
  - 使用SQL语句: SELECT * FROM products, brand WHERE products.brand_id = brand.id;

``` SQL
# 多表查询的方法
# 1. 先获取到笛卡尔乘积
SELECT * FROM products, brand;

# 2. 对笛卡尔乘积进行筛选
SELECT * FROM products, brand WHERE products.brand_id = brand.id;

# 以上方法本质上是将所有的结果都查询出来然后进行了筛选, 一般来说我们不会这样取操作的
```

### 多表之间的连接
+ 事实上想要达到上面所说的效果, 并不是像上面的方式操作的, 而是使用SQL JOIN操作
+ 左连接
  - 如果我们希望获取到的是左边所有的数据(以左表为主)
    + 这个时候就表示无论左边的表是否有对应的brand_id的值对应右边表的id, 左边的数据都会被查询出来
    + 这个也是开发中使用最多的情况, 它的完整写法是 LEFT [OUTER] JOIN, 其中的OUTER可以省略
+ 右连接
  - 如果我们希望获取到的是右边所有的数据(以右表为主)
    + 这个时候就表示无论左边的表中的brand_id是否有和右边表中的id对应, 右边的数据都会被查询出来
    + 右连接在开发中没有左连接常用, 它的完整写法是 RIGHT [OUTER] JOIN,其中的OUTER可以省略
+ 内连接
  - 事实上内连接表示左边的表和右边的表都有对应的数据关联
    + 内连接在开发中偶尔也会常见使用
    + 内连接有其他的写法: INNER JOIN , CROSS JOIN 或者 JOIN都可以
+ 外连接
  - SQL规范中全连接是使用FULL JOIN, 但是MySQL中并没有对它支持, 所以我们是使用UNION来实现全连接的

``` SQL
# 3. 左连接
# 3.1 查询所有的手机(包括没有品牌信息的手机)以及对应的品牌
SELECT * FROM products LEFT JOIN brand ON products.brand_id = brand.id;

# 3.2 查询没有对应品牌信息的手机
SELECT * FROM products LEFT JOIN brand ON products.brand_id = brand.id WHERE brand.id IS NULL;

# 4. 右连接
# 4.1 查询所有品牌(包括没有手机信息的品牌)以及对应的手机
SELECT * FROM products RIGHT JOIN brand ON products.brand_id = brand.id;

# 4.2 查询没有对应手机信息的品牌
SELECT * FROM products RIGHT JOIN brand ON products.brand_id = brand.id WHERE products.brand_id IS NULL;

# 5. 内连接
# INNER JOIN 和 CROSS JOIN 都是一样的
SELECT * FROM products INNER JOIN brand ON products.brand_id = brand.id;
SELECT * FROM products CROSS JOIN brand ON products.brand_id = brand.id;

# 6. 全连接
# mySQL中是不支持 FULL OUTER JOIN 的
# 所以我们需要将左连接和右连接通过 UNION 做一个联合
(SELECT * FROM products LEFT JOIN brand ON products.brand_id = brand.id)
UNION
(SELECT * FROM products RIGHT JOIN brand ON products.brand_id = brand.id);

(SELECT * FROM products LEFT JOIN brand ON products.brand_id = brand.id WHERE brand.id IS NULL)
UNION
(SELECT * FROM products RIGHT JOIN brand ON products.brand_id = brand.id WHERE products.brand_id IS NULL);
```

### 多对多关系数据
+ 在开发中我们会经常遇到多对多的关系
  - 比如学生可以选择多门课程, 一个课程也可以被多个学生选择
  - 这种情况我们就会使用多对多关系的数据
+ 多对多数据表关键点是有一个关系表来记录两张表中的数据关系
``` SQL
# 创建两张表并插入数据
CREATE TABLE IF NOT EXISTS students (
	id INT PRIMARY KEY AUTO_INCREMENT,
	name VARCHAR(20) NOT NULL,
	age INT
);

CREATE TABLE IF NOT EXISTS courses (
	id INT PRIMARY KEY AUTO_INCREMENT,
	name VARCHAR(20) NOT NULL,
	price DOUBLE
);

# 插入学生数据以及课程数据

INSERT INTO `students` (name, age) VALUES('why', 18);
INSERT INTO `students` (name, age) VALUES('tom', 22);
INSERT INTO `students` (name, age) VALUES('lilei', 25);
INSERT INTO `students` (name, age) VALUES('lucy', 16);
INSERT INTO `students` (name, age) VALUES('lily', 20);

INSERT INTO `courses` (name, price) VALUES ('英语', 100);
INSERT INTO `courses` (name, price) VALUES ('语文', 666);
INSERT INTO `courses` (name, price) VALUES ('数学', 888);
INSERT INTO `courses` (name, price) VALUES ('历史', 80);
INSERT INTO `courses` (name, price) VALUES ('物理', 888);
INSERT INTO `courses` (name, price) VALUES ('地理', 333);

# 创建关系表
CREATE TABLE IF NOT EXISTS stuendts_select_courses (
	id INT PRIMARY KEY AUTO_INCREMENT,
	students_id INT NOT NULL,
	courses_id INT NOT NULL,
	FOREIGN KEY (students_id) REFERENCES students(id) ON UPDATE CASCADE,
	FOREIGN KEY (courses_id) REFERENCES courses(id) ON UPDATE CASCADE
);

# 学生选课
INSERT INTO students_select_courses (students_id, courses_id) VALUES (1,1);
INSERT INTO students_select_courses (students_id, courses_id) VALUES (1,3);
INSERT INTO students_select_courses (students_id, courses_id) VALUES (1,4);

INSERT INTO students_select_courses (students_id, courses_id) VALUES (3,2);
INSERT INTO students_select_courses (students_id, courses_id) VALUES (3,4);

INSERT INTO students_select_courses (students_id, courses_id) VALUES (5,2);
INSERT INTO students_select_courses (students_id, courses_id) VALUES (5,3);
INSERT INTO students_select_courses (students_id, courses_id) VALUES (5,4);

# 查询所有有选课的学生, 以及这些学生选择的课程
SELECT stu.id id, stu.name studentName, stu.age studentAge, cs.id coursesId, cs.name coursesName, cs.price coursesPrice  
FROM students stu 
INNER JOIN students_select_courses ssc ON ssc.students_id = stu.id 
INNER JOIN courses cs ON ssc.courses_id = cs.id; 

# 查询所有学生的选课情况
SELECT stu.id id, stu.name studentName, stu.age studengAge, cs.id coursesId, cs.name coursesName, cs.price coursesPrice
FROM students stu
LEFT JOIN students_select_courses ssc ON ssc.students_id = stu.id
LEFT JOIN courses cs ON ssc.courses_id = cs.id;

# 查询哪些学生是没有选课的
SELECT stu.id id, stu.name studentName, stu.age studengAge, cs.id coursesId, cs.name coursesName, cs.price coursesPrice
FROM students stu
LEFT JOIN students_select_courses ssc ON ssc.students_id = stu.id
LEFT JOIN courses cs ON ssc.courses_id = cs.id
WHERE ssc.courses_id IS NULL;

# 查询哪些课程是没有被选择的
SELECT stu.id id, stu.name studentName, stu.age studengAge, cs.id coursesId, cs.name coursesName, cs.price coursesPrice
FROM courses cs
LEFT JOIN students_select_courses ssc ON ssc.courses_id = cs.id
LEFT JOIN students stu ON ssc.students_id = stu.id
WHERE ssc.students_id IS NULL;

# 查询某一个学生选择了哪些课程
# 比如查询why id为1 学生的选课情况
SELECT stu.id id, stu.name studentName, stu.age studengAge, cs.id coursesId, cs.name coursesName, cs.price coursesPrice
FROM students stu
LEFT JOIN students_select_courses ssc ON ssc.students_id = stu.id
LEFT JOIN courses cs ON ssc.courses_id = cs.id
WHERE stu.id = 1;
```


### 将数据转换成对象(一对多)
+ 在真实开发中, 我们经常会将数据打包到一个对象中然后传输给客户端
+ 这个时候我们就要用到JSON_OBJECT();

### 将多条数据组织成对象然后放到一个数组中(多对多)
+ 在多对多关系中, 我们希望擦好像到的是一个数组
+ 这个时候我们要用JSON_ARRAYAGG和JSON_OBJECT结合来使用

``` SQL
# 将联合查询到的数据转换成对象(一对多)
SELECT
	products.id id, products.title title, products.price price, products.score score, products.url url,
	JSON_OBJECT('id', brand.id, 'name', brand.name, 'website', brand.website, 'phoneRank', brand.phoneRank) brand
FROM products
LEFT JOIN brand ON products.brand_id = brand.id;

# 将查询到的多条数据, 组织成对象, 放到一个数组中(多对多)
# 首先要将查询到的学生数据按照学生id分组
# 通过JSON_OBJECT()方法将数据转换为对象
# 然后通过JSON_ARRAYAGG()将对象存放到数组中
SELECT 
	stu.id id, stu.name name, stu.age age,
	JSON_ARRAYAGG(JSON_OBJECT('id', cs.id, 'name', cs.name, 'price', cs.price)) courses
FROM students stu
JOIN students_select_courses ssc ON stu.id = ssc.students_id
JOIN courses cs ON ssc.courses_id = cs.id
GROUP BY stu.id;
```