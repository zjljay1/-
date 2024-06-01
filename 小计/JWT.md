JSON Web Token（JWT）是目前最流行的跨域身份验证解决方案。今天给大家介绍JWT的原理和用法。<br />一般Web应用的需要进行**认证**和**授权**。

- **认证：验证当前访问系统的是不是本系统的用户，并且要确认具体是哪个用户**
- **授权：经过认证后判断当前用户是否有权限进行某个操作**

在了解JWT前，我们先了解以前是如何进行认证的

- session认证
   - 众所周知，**http 协议本身是无状态的协议**，那就意味着当有用户向系统使用账户名称和密码进行用户认证之后，下一次请求还要再一次用户认证才行。因为我们不能通过 http 协议知道是哪个用户发出的请求，所以如果要知道是哪个用户发出的请求，那就需要在服务器保存一份用户信息(保存至 session )，然后在认证成功后返回 cookie 值传递给浏览器，那么用户在下一次请求时就可以带上 cookie 值，服务器就可以识别是哪个用户发送的请求，是否已认证，是否登录过期等等。这就是传统的 session 认证方式。
   - session 认证的缺点其实很明显，由于 session 是保存在服务器里，所以如果分布式部署应用的话，会出现session不能共享的问题，很难扩展。于是乎为了解决 session 共享的问题，又引入了 redis，接着往下看。
   - Session认证还会引发CSRF（跨站请求伪造攻击），因为是基于cookie来进行用户识别的, cookie如果被截获，用户就会很容易受到跨站请求伪造的攻击。
- Token认证
   - 这种方式跟Session的方式流程差不多，不同的地方在于保存的是一个token值，token一般是一串随机的字符(比如UUID)，value 一般是用户ID，并且设置一个过期时间。
   - 每次请求服务的时候带上 token 在请求头，后端接收到token 则根据 token 查一下 redis 是否存在，如果存在则表示用户已认证，如果 token 不存在则跳到登录界面让用户重新登录，登录成功后返回一个 token 值给客户端。
- JWT认证
   - JWT（全称Json Web Token），是为了在网络应用环境间传递声明而执行的一种基于JSON的开放标准.该token被设计为紧凑且安全的，特别适用于分布式站点的单点登录（SSO）场景。JWT的声明一般被用来在身份提供者和服务提供者间传递被认证的用户身份信息，以便于从资源服务器获取资源，也可以增加一些额外的其它业务逻辑所必须的声明信息，该token也可直接被用于认证，也可被加密。
<a name="QWY3a"></a>
## JWT的数据结构
JWT 一般是这样一个字符串，分为三个部分，以 “.” 隔开：
```
xxxxx.yyyyy.zzzzz
```
JWT官网：[https://jwt.io/](https://jwt.io/)<br />进入官网我们可以看到首页有这样一个页面<br />![](https://cdn.nlark.com/yuque/0/2024/png/28163149/1713450710182-1cb7bd8c-295b-499d-b07c-a05317a3bdf6.png#averageHue=%23fcfcfc&clientId=u594f3c68-6050-4&from=paste&id=ub84ff682&originHeight=1692&originWidth=2360&originalType=url&ratio=1.25&rotation=0&showTitle=false&status=done&style=none&taskId=u2031831a-b400-4c6c-adef-fd2aeef243a&title=)<br />其中左侧是生成的jwt编码，我们可以看到它生成的格式就如上述所描述那样，分成了三段
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.

eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.

SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

```
右侧是对jwt字符串进行解码<br />**HEADER**<br />jwt的头部承载两部分信息：<br />声明类型，这里是jwt<br />声明加密的算法 通常直接使用 HMAC SHA256<br />完整的头部就像下面这样的JSON：<br />![](https://cdn.nlark.com/yuque/0/2024/png/28163149/1713450741437-2d019776-ea39-4f37-947d-b7315cd2ba1a.png#averageHue=%23fcf8f8&clientId=u594f3c68-6050-4&from=paste&id=u4f93ce07&originHeight=332&originWidth=1144&originalType=url&ratio=1.25&rotation=0&showTitle=false&status=done&style=none&taskId=u2fc7d5a6-4913-4de6-bf00-8c7e3146cfb&title=)<br />然后将头部进行base64加密（该加密是可以对称解密的),构成了第一部分：<br />eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9<br />**PLAYLOAD**：<br />载荷就是存放有效信息的地方。这个名字像是特指飞机上承载的货品，这些有效信息包含三个部分<br />标准中注册的声明

- 公共的声明
- 私有的声明
- 标准中注册的声明 (建议但不强制使用) ：
   - iss: jwt签发者
   - sub: jwt所面向的用户
   - aud: 接收jwt的一方
   - exp: jwt的过期时间，这个过期时间必须要大于签发时间
   - nbf: 定义在什么时间之前，该jwt都是不可用的.
   - iat: jwt的签发时间
   - jti: jwt的唯一身份标识，主要用来作为一次性token,从而回避重放攻击。

公共的声明 ： 公共的声明可以添加任何的信息，一般添加用户的相关信息或其他业务需要的必要信息.但不建议添加敏感信息，因为该部分在客户端可解密.<br />私有的声明 ： 私有声明是提供者和消费者所共同定义的声明，一般不建议存放敏感信息，因为base64是对称解密的，意味着该部分信息可以归类为明文信息。<br />![](https://cdn.nlark.com/yuque/0/2024/png/28163149/1713450869198-34a5d0cc-c6eb-4df2-b654-1b522a984069.png#averageHue=%23fdfdfd&clientId=u594f3c68-6050-4&from=paste&id=u1b05128c&originHeight=376&originWidth=1146&originalType=url&ratio=1.25&rotation=0&showTitle=false&status=done&style=none&taskId=ud2016551-6662-4f3e-aaa7-606b222ae21&title=)<br />然后将其进行base64加密，得到JWT的第二部分：
```
eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ
```
**VERIFY SIGNATURE：**<br /> JWT的第三部分是一个签证信息，这个签证信息由三部分组成：<br />header (base64后的)<br />payload (base64后的)<br />secret<br />这个部分需要base64加密后的header和base64加密后的payload使用.连接组成的字符串，然后通过header中声明的加密方式进行加盐secret组合加密，然后就构成了jwt的第三部分：
```
SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```
:::info
**注意**：secret是保存在服务器端的，jwt的签发生成也是在服务器端的，secret就是用来进行jwt的签发和jwt的验证，所以，它就是你服务端的私钥，在任何场景都不应该流露出去。一旦客户端得知这个secret, 那就意味着客户端是可以自我签发jwt了。
:::
如何应用：<br />一般是在请求头里加入Authorization，并加上Bearer标注
```
 'Authorization': 'Bearer ' + token
```
<a name="Shk6y"></a>
## 签名密钥
在Json网络令牌的安全上下文中，签名密钥是用于对JWT进行数字签名的加密信息，签名密钥用于创建JWT的签名部分，用于验证JWT的发送者是否是已经经过确认的用户，并确保消息在整个过程中没有被更改（保证一致性），因此我们要确保发送此JWT密钥的用户是同一个人。<br />签名密钥通常与JWT标头中指定的登录算法结合使用，以创建签名具体的登录算法，密钥大小将取决于应用程序的安全要求和信任级别（签名方）<br />可以在[allkeysgenertor](https://allkeysgenerator.com/)中生成任意大小的签名密钥<br />注意⚠️：在JWT中最低安全级别是256bit，因此在本教程中，我们将采用256bit的签名密钥，如下所示：<br />![](https://cdn.nlark.com/yuque/0/2024/png/28163149/1713451277371-e77cc645-e5d5-46c8-8908-511ab02b0ba1.png#averageHue=%23f8f7f6&clientId=u594f3c68-6050-4&from=paste&id=u6a519f58&originHeight=1362&originWidth=1638&originalType=url&ratio=1.25&rotation=0&showTitle=false&status=done&style=none&taskId=ubda887ed-b270-4cad-b742-51610da9632&title=)<br />接下来为SpringBoot3.0 + SpringSecurity6.0+JWT的使用
> 

[springBoot整合SpingSecurity](https://www.yuque.com/dadaguaijiangjun-kdkrb/fy3ez5/kun7xu29xddu4ue8?view=doc_embed)
