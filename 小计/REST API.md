[一文彻底弄懂REST API](https://zhuanlan.zhihu.com/p/536437382)
<a name="Liw4Q"></a>
## 什么是API
首先介绍API的概念，Application Programming Interface（**应用程序接口**）是它的全称。简单的理解就是，API是一个接口。那么它是一个怎样的接口呢，现在我们常将它看成一个HTTP接口即HTTP API。也就是说这个接口得通过HTTP的方式来调用，做过前后端开发的小伙伴可能知道，后端开发又叫做面向接口开发，我们往往会提供一个接口供前端调用，或者供其他服务调用。举个例子，我们程序中往往会涉及到调用第三方接口，比如说，调用支付宝或者微信的支付接口来实现我们程序中的支付功能、调用带三方的短信接口来向用户发送验证码短信等等...<br />这样说吧，比如说我们有一个可以允许我们查看（view），创建（create），编辑（edit）以及删除（delete）部件的应用程序。我们可以创建一个可以让我们执行这些功能的HTTP API:
```java
http://demo.com/view_books
http://demo.com/create_new_book?name=shuxue
http://demo.com/update_book?id=1&name=shuxue
http://demo.com/delete_book?id=1
```
这是4个HTTP API，分别实现了图书的查看、新增、编辑、删除的操作，当我们把接口发布出去的时候，别人就可以通过这四个接口来调用相关的服务了。但是这样做有什么不方便的地方呢？你可能发现了，这种API的写法有一个缺点 ，那就是没有一个统一的风格，比如说第一个接口表示查询全部图书的信息，我们也可以写成这样：
```java
http://demo.com/books/list
```
那这样就会造成使用我们接口的其他人必须得参考API才能知道它是怎么运作的。<br />不用担心，REST会帮我们解决这个问题。
<a name="Cz21K"></a>
## 什么是REST？
有了上面的介绍，你可能也大概有了直观的了解，说白了，**REST是一种风格！**<br />REST的作用是将我们上面提到的查看（view），创建（create），编辑（edit）和删除（delete）直接映射到HTTP 中已实现的**GET,POST,PUT和DELETE**方法。<br />这四种方法是比较常用的，HTTP总共包含**八种**方法：<br /> 
```java
GET
POST
PUT
DELETE
OPTIONS
HEAD
TRACE
CONNECT
```
当我们在浏览器点点点的时候我们通常只用到了GET方法，当我们提交表单，例如注册用户的时候我们就用到了POST方法...<br />介绍到这里，我们重新将上面的四个接口改写成REST风格：<br />**查看所有图书：**
```java
GET http://demo.com/books
```
**新增一本书：**
```java
POST http://demo.com/books
Data: name=shuxue
```
**修改一本书：**
```java
PUT http://demo.com/books
Data:id=1,name=shuxue
```
**删除一本书：**
```java
DELETE http://demo.com/books
Data:id=1
```
大家有没有发现，这样改动之后API变得统一了，我们只需要改变请求方式就可以完成相关的操作，这样大大简化了我们接口的理解难度，变得易于调用。<br />**HTTP状态码**<br />REST的另一重要部分就是为既定好请求的类型来响应正确的状态码。如果你对HTTP状态码陌生，以下是一个简易总结。当你请求HTTP时，服务器会响应一个状态码来判断你的请求是否成功，然后客户端应如何继续。以下是四种不同层次的状态码：

- 2xx = Success（成功）
- 3xx = Redirect（重定向）
- 4xx = User error（客户端错误）
- 5xx = Server error（服务器端错误）

我们常见的是200（请求成功）、404（未找到）、401（未授权）、500（服务器错误）...<br />**API格式响应**<br />上面介绍了REST API的写法，响应状态码，剩下就是请求的数据格式以及响应的数据格式。说的通俗点就是，我们用什么格式的参数去请求接口并且我们能得到什么格式的响应结果。<br />我这里只介绍一种用的最多的格式——JSON格式<br />目前json已经发展成了一种最常用的数据格式，由于其轻量、易读的优点。<br />所以我们经常会看到一个请求的header信息中有这样的参数：<br />Accept:application/json<br />这个参数的意思就是接收来自后端的json格式的信息。<br />我举个json响应的例子：
```json
{
  "code": 200,
  "books": [{
    "id": 1,
    "name": "yuwen"
  }, {
    "id": 2,
    "name": "shuxue"
  }]
}
```
这样返回是不是一目了然，而且冗余信息很少！

