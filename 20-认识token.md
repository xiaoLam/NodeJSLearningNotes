### 认识token
+ cookie和session的方式有很多的缺点
  - cookie会被附加在每一个HTTP请求中, 即是这个HTTP请求不需要cookie, 所以无形中增加了流量
  - cookie是使用明文传递的,所以存在安全性问题, session可以解决这个问题,但是session也是可以自行使用base64解密的, 除非使用加严操作
  - cookie的大小限制是4kb, 所以对于复杂的需求来说是不够用的
  - 对于浏览器外的其他客户端, 比如安卓和IOS, 必须手动地设置cookie和session
  - 对于分布式系统和服务器集群中想要正确地解析session是比较困难的
  - token解决这个问题的方式为, 只有一台服务器拥有私钥和颁发token的权限, 而其他服务器拥有公钥和解析token的权限, 以此方便地解析token
+ 所以在目前的前后端分离开发过程中, 使用token来进行身份验证是最多的情况
  - token可以翻译为令牌
  - 也就是在验证了用户账号和密码正确的情况下, 给用户颁发一个令牌
  - 这个令牌作为后续用户访问一些接口或者资源的凭证
  - 我们可以根据这个凭证来判断用户是否有权限来访问
+ 所以token的使用一个分成两个重要的步骤
  - 生成token: 登陆的时候, 颁发token
  - 验证token: 访问某些资源或者接口的时候, 验证token

### JWT(json web token)实现Token机制
+ JWT的作用就是帮助我们生成token
+ JWT生成的Token由三个部分组成:
  - header
    - alg: 采用的加密算法, 默认的是HMAC SHA256(HS256), 采用同一个密钥进行加密和解密
    - typ: JWT, 固定值, 通常都写成JWT即可
    - 会通过base64Url算法进行编码
  - payload
    - 携带的数据, 比如我们可以将用户的id和name放到payload中
    - 默认也会携带iat(issued at), 也就是令牌的签发时间
    - 我们也可以设置令牌的过期时间: exp(expiration time)
    - 会通过base64Url算法进行编码
  - signature
    - 设置一个secretKey, 通过将前面两个的结果合并后进行HMACSHA256的算法
    - HMACSHA256(base64Url(header)+.+base64Url(payload), secretKey);
    - 但是如果secretKey暴露是一件十分危险的时器, 英文之后就可以模拟颁发token, 也可以解密token了

### Token的使用
+ 在真实开发中, 直接使用jsonwebtoken来实现
``` js
// 使用jsonwebtoken库
const jwt = require("jsonwebtoken");

// 设置secretKey
const SECRET_KEY = "xiaoLam123";

// 登陆验证颁发token
testRouter.post("/login", (ctx, next) => {
  // 首先获取到用户的信息
  const user = {
    id: 100,
    name: "xiaoLam"
  };

  // 然后通过jwt.sign方法生成token
  const token = jwt.sign(user, SECRET_KEY, {
    // 设置过期时间, 对应秒
    expiresIn: 10
  })

  // 然后将生成的token返回给客户端
  ctx.body = token;
})

// 验证token接口
testRouter.get("/auth", (ctx, next) => {
  // 一般来说, 客户端会将token放在headers中, 所以从headers中获取token
  const authorization = ctx.headers.authorization;
  // 对authorization进行字符串切割获取真正的token
  const token = authorization.replace("Bearer ", "");

  // 返回验证信息
  // 注意jsonwebtoken库中, 如果验证失败的话会抛出错误, 所以使用try catch来捕获错误
  try {
    // 使用jwt.verify来解析验证从客户端传递过来的token, 将token和密钥作为参数传入即可
    const result = jwt.verify(token, SECRET_KEY);
    ctx.body = result;
  } catch (error) {
    ctx.body = "token错误 " + error.message;
  }
})
```

### 非对称加密
+ HS256加密算法使用单一密钥, 如果一旦暴露就是非常危险的事情
  - 比如在分布式系统中, 每一个子系统都需要获得密钥来对token进行解析
  - 那么拿到这个密钥后子系统既可以颁发token, 也可以验证token
  - 但是对于一些资源服务器来说, 只需要由验证token的能力就足够了
+ 这个时候我们可以使用非对称加密, RS256
  - 私钥(private key): 用于颁发token
  - 公钥(public key): 用于验证token
+ 我们可以使用openssl工具来生成一对私钥和公钥
  - Mac电脑可以直接使用terminal终端来使用openssl
  - 但是window电脑中默认的cmd终端是不能直接使用的, 建议使用git bash终端来使用openssl
+ openssl的基本使用
  - 生成一个私钥
    + genrsa -out 输出的文件名 私钥长度
    + 比如: genrsa -out private.key 1024
  - 根据私钥生成一个公钥
    + rsa -in 私钥的文件名 -pubout -out 输出的文件名
    + 比如: rsa -in private.key -pubout -out public.key

### 使用公钥和私钥签发和验证签名
``` js
const Koa = require("koa");
const Router = require("koa-router");

const jwt = require("jsonwebtoken");
const fs = require("fs");

const app = new Koa();

const testRouter = new Router();

// 获取私钥和公钥
const PRIVATE_KEY = fs.readFileSync("./keys/private.key");
const PUBLIC_KEY = fs.readFileSync("./keys/public.key");

testRouter.post("/login", (ctx, next) => {
  // 首先获取到用户信息
  const user = {
    id: 100,
    name: "xiaoLam"
  };

  // 根据用户信息和私钥生成token
  // jwt中传入secretKey的参数可以传入Buffer
  const token = jwt.sign(user, PRIVATE_KEY, {
    expiresIn: 10,
    // 注意因为是使用了非默认算法进行加密, 所以要设置加密方式
    algorithm: "RS256"
  })

  // 返回密钥
  ctx.body = token;
})

testRouter.get("/auth", (ctx, next) => {
  // 获取密钥
  const authorization = ctx.headers.authorization;
  const token = authorization.replace("Bearer ", "");

  // 验证token
  const result = jwt.verify(token, PUBLIC_KEY, {
    algorithms: ["RS256"]
  })

  // 返回结果
  ctx.body = result;
})

app.use(testRouter.routes());
app.use(testRouter.allowedMethods());

app.listen(8000, () => {
  console.log("使用非对称密钥服务器启动成功");
})
```

### 补充知识点
+ 在项目中的任何一个地方的相对路径, 都是相对于process.cwd()的, 也就是相对于启动目录