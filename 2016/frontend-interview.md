## HTML5 新增了那些内容或 API，使用过那些

新特性：

1. 拖拽释放(Drag and drop) API
2. 语义化更好的内容标签（header,nav,footer,aside,article,section）
3. 音频、视频 API(audio,video)
4. 画布(Canvas) API
5. 地理(Geolocation) API
6. 本地离线存储 localStorage 长期存储数据，浏览器关闭后数据不丢失；
7. sessionStorage 的数据在浏览器关闭后自动删除
8. 表单控件，calendar、date、time、email、url、search
9. 新的技术 webworker, websocket, Geolocation
   web worker:是运行在后台的 JavaScript，独立于其他脚本，不会影响页面的性能。您可以继续做任何愿意做的事情：点击、选取内容等等，而此时 web worker 在后台运行。
   websocket:是一个持久化的协议，相对于 HTTP 这种非持久的协议来说
   html5shim：解决 HTML5 新标签的浏览器兼容问题。

## CSS3 有哪些新特性

1. CSS3 实现圆角（border-radius），阴影（box-shadow），
2. 对文字加特效（text-shadow），线性渐变（gradient），旋转（transform）
3. transform:rotate(9deg) scale(0.85,0.90) translate(0px,-30px) skew(-9deg,0deg);// 旋转,缩放,定位,倾斜
4. 增加了更多的 CSS 选择器 多背景 rgba
5. 在 CSS3 中唯一引入的伪元素是 ::selection.
6. 媒体查询，多栏布局
7. border-image

## 本地存储（Local Storage ）和 Cookies

共同点：都是保存在浏览器端，且同源的。
区别：

1. cookie 数据始终在同源的 http 请求中携带（即使不需要），即 cookie 在浏览器和服务器间来回传递。而 sessionStorage 和 localStorage 不会自动把数据发给服务器，仅在本地保存。cookie 数据还有路径（path）的概念，可以限制 cookie 只属于某个路径下。
2. 存储大小限制也不同，cookie 数据不能超过 4k，同时因为每次 http 请求都会携带 cookie，所以 cookie 只适合保存很小的数据，如会话标识。sessionStorage 和 localStorage 虽然也有存储大小的限制，但比 cookie 大得多，可以达到 5M 或更大。
3. 数据有效期不同，sessionStorage：仅在当前浏览器窗口关闭前有效，自然也就不可能持久保持；localStorage：始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据；cookie 只在设置的 cookie 过期时间之前一直有效，即使窗口或浏览器关闭。
4. 作用域不同，sessionStorage 不在不同的浏览器窗口中共享，即使是同一个页面；localStorage 在所有同源窗口中都是共享的；cookie 也是在所有同源窗口中都是共享的。
5. Web Storage 支持事件通知机制，可以将数据更新的通知发送给监听者。
6. Web Storage 的 api 接口使用更方便。

## input 和 textarea 的区别

都可以输入数据，只是表现形式不同而已，input 单行文本框，textarea 多行文本

## 用一个 div 模拟 textarea 的实现

```css
.test-box {
  width: 400px;
  min-height: 120px;
  max-height: 300px;
  _height: 120px;
  margin-left: auto;
  margin-right: auto;
  padding: 3px;
  outline: 0;
  border: 1px solid #a0b3d6;
  font-size: 12px;
  word-wrap: break-word;
  overflow-x: hidden;
  overflow-y: auto;
  -webkit-user-modify: read-write-plaintext-only;
}
```

```html
<div class="test-box" contenteditable="true"><br /></div>
```

## 左右布局：左边定宽、右边自适应，不少于 3 种方法

方法一：左边设置左浮动，右边宽度设置 100%

```css
.left {
  float: left;
}
.right {
  width: 100%;
}
```

方法二： 父容器设置 `display：flex`；Right 部分设置 `flex：1`

```css
body {
  display: flex;
}
.right {
  flex: 1;
}
```

方法三：左边绝对定位，右边加个 margin-left;

```css
.left {
  position: absolute;
  top: 0;
  left: 0;
  width: 200px;
  background: #ff0;
}
.right {
  margin-left: 200px;
  background: #f0f;
}
```

方法四：左边左浮动，右边加个`overflow:hidden`;

```css
.left {
  float: left;
  width: 200px;
  background: #ff0;
}
.right {
  overflow: hidden;
  background: #f0f;
}
```

方法五：左边左浮动，右边加个`margin-left`;

```css
.left {
  float: left;
  width: 200px;
  background: #ff0;
}
.right {
  margin-left: 200px;
  background: #f0f;
}
```

方法六：左右两边绝对定位，右边加个`width,top,left,right`

```css
.left {
  position: absolute;
  top: 0;
  left: 0;
  width: 200px;
  background: #ff0;
}
.right {
  position: absolute;
  top: 0;
  left: 200px;
  width: 100%;
  rigth: 0;
  background: #f0f;
}
```

## WebView 优化

- WebView 初始化慢，可以在初始化同时先请求数据，让后端和网络不要闲着。
- 后端处理慢，可以让服务器分 trunk 输出，在后端计算的同时前端也加载网络静态资源。
- 脚本执行慢，就让脚本在最后运行，不阻塞页面解析。
- 同时，合理的预加载、预缓存可以让加载速度的瓶颈更小。
- WebView 初始化慢，就随时初始化好一个 WebView 待用。
- DNS 和链接慢，想办法复用客户端使用的域名和链接。
- 脚本执行慢，可以把框架代码拆分出来，在请求页面之前就执行好。

## this

- this 指的是，调用函数的那个对象
- 在函数被直接调用时 this 绑定到全局对象。在浏览器中，window 就是该全局对象
- setTimeout、setInterval 这两个方法执行的函数 this 也是全局对象
- `Function.prototype.bind` bind，返回一个新函数，并且使函数内部的`this`为传入的第一个参数

```javascript
var fn3 = obj1.fn.bind(obj1);
fn3();
```

- 使用 call 和 apply 设置 this,call apply，调用一个函数，传入函数执行上下文及参数

```javascript
fn.call(context, param1, param2...)
fn.apply(context, paramArray)
```

## caller

在函数 A 调用函数 B 时，被调用函数 B 会自动生成一个 caller 属性，指向调用它的函数对象，如果函数当前未被调用，或并非被其他函数调用，则 caller 为 null

## arguments

1.在函数调用时，会自动在该函数内部生成一个名为 arguments 的隐藏对象 2.该对象类似于数组，可以使用[]运算符获取函数调用时传递的实参 3.只有函数被调用时，arguments 对象才会创建，未调用时其值为 null

## callee

当函数被调用时，它的 arguments.callee 对象就会指向自身，也就是一个对自己的引用

由于 arguments 在函数被调用时才有效，因此 arguments.callee 在函数未调用时是不存在的（即 null.callee），且解引用它会产生异常

## new 过程

new 运算符接受一个函数 F 及其参数：new F(arguments...)。这一过程分为三步：

1. 创建类的实例。这步是把一个空的对象的 proto 属性设置为 F.prototype 。
2. 初始化实例。函数 F 被传入参数并调用，关键字 this 被设定为该实例。
3. 返回实例。
   ```javascript
   var o = new Object();
   o.__proto__ = F.prototype;
   F.call(o);
   var f = o;
   ```

- prototype 是面向构造函数，proto 是面向实例化后的对象。

## 函数的执行环境

- JavaScript 中的函数既可以被当作普通函数执行，也可以作为对象的方法执行，这是导致 `this`含义如此丰富的主要原因

1.  一个函数被执行时，会创建一个执行环境（ExecutionContext），函数的所有的行为均发生在此执行环境中，构建该执行环境时，JavaScript 首先会创建 `arguments`变量，其中包含调用函数时传入的参数

2.  接下来创建作用域链，然后初始化变量。首先初始化函数的形参表，值为 arguments 变量中对应的值，如果 arguments 变量中没有对应值，则该形参初始化为 `undefined`。

3.  如果该函数中含有内部函数，则初始化这些内部函数。如果没有，继续初始化该函数内定义的局部变量，需要注意的是此时这些变量初始化为`undefined`，其赋值操作在执行环境（ExecutionContext）创建成功后，函数执行时才会执行，这点对于我们理解 JavaScript 中的变量作用域非常重要，最后为 this 变量赋值，会根据函数调用方式的不同，赋给 this 全局对象，当前对象等

4.  至此函数的执行环境（ExecutionContext）创建成功，函数开始逐行执行，所需变量均从之前构建好的执行环境（ExecutionContext）中读取

## 三种变量

实例变量：（this）类的实例才能访问到的变量
静态变量：（属性）直接类型对象能访问到的变量
私有变量：（局部变量）当前作用域内有效的变量

```javascript
function ClassA() {
  var a = 1; //私有变量，只有函数内部可以访问
  this.b = 2; //实例变量，只有实例可以访问
}

ClassA.c = 3; // 静态变量，也就是属性，类型访问

console.log(a); // error
console.log(ClassA.b); // undefined
console.log(ClassA.c); //3

var classa = new ClassA();
console.log(classa.a); //undefined
console.log(classa.b); // 2
console.log(classa.c); //undefined
```

## 原型链和继承

- 我们通过函数定义了类`Person`，类（函数）自动获得属性`prototype`
- 每个类的实例都会有一个内部属性`__proto__`，指向类的`prototype`属性
- 任何类的`prototype`属性本质上都是个类`Object`的实例，所以`prototype`也和其它实例一样也有个`__proto__`内部属性，指向其类型`Object`的`prototype`

## 继承

继承是指一个对象直接使用另一对象的属性和方法

```js
function inherit(superType, subType) {
  var _prototype = Object.create(superType.prototype);
  _prototype.constructor = subType;
  subType.prototype = _prototype;
}
```

## hasOwnProperty

hasOwnPerperty 是 Object.prototype 的一个方法，可以判断一个对象是否包含自定义属性而不是原型链上的属性，hasOwnProperty 是 JavaScript 中唯一一个处理属性但是不查找原型链的函数

## 多态

## object.create object.extend

## 图片懒加载

主要原理是将非首屏的图片 src 设为一个默认值，然后监听窗口滚动，把真实地址放在 data-\*属性，当图片出现在视窗中时再给他赋予真实的图片地址，这样可以保证首屏的加载速度然后按需加载图片。

## Less、Sass、Stylus

-

- Less:不支持 if, 需要使用 when 封装成 mixin 实现，
- Sass:提供内置数字函数，支持 if else
- Stylus:

## CSS 的优先级

- 继承样式 < 通用选择器\* < 元素(类型)选择器 < 类选择器 < 属性选择器 < 伪类 < ID 选择器 < 内联样式 <js 设置样式 <!important

- 都有!important：最后一个!important，给选择器更高的优先级

## import & export

1.当用`export default people`导出时，就用 `import people` 导入（不带大括号） 2.一个文件里，有且只能有一个`export default`。但可以有多个`export`。 3.当用`export name`时，就用`import { name }`导入（记得带上大括号） 4.当一个文件里，既有一个`export default people`, 又有多个`export name` 或者 `export age`时，导入就用 `import people, { name, age }` 5.当一个文件里出现 n 多个`export` 导出很多模块，导入时除了一个一个导入，也可以用`import * as example`

## 节流、防抖

## $(function(){}) 与$(document).ready(function(){}) 、window.onload

http://www.cnblogs.com/a546558309/p/3478344.html

## 快速排序、时间和空间复杂度

# 网络

## HTTP 状态码及其含义

参考 RFC [2616](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html),[MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status)
1XX：信息状态码
100 Continue：客户端应当继续发送请求。这个临时相应是用来通知客户端它的部分请求已经被服务器接收，且仍未被拒绝。客户端应当继续发送请求的剩余部分，或者如果请求已经完成，忽略这个响应。服务器必须在请求万仇向客户端发送一个最终响应
101 Switching Protocols：服务器已经理解力客户端的请求，并将通过 Upgrade 消息头通知客户端采用不同的协议来完成这个请求。在发送完这个响应最后的空行后，服务器将会切换到 Upgrade 消息头中定义的那些协议。
2XX：成功状态码
200 OK：请求成功，请求所希望的响应头或数据体将随此响应返回
201 Created：请求成功并且服务器创建了新的资源
202 Accepted：服务器已接受请求，但尚未处理
203 Non-Authoritative Information：
204 No Content：
205 Reset Content：
206 Partial Content：
3XX：重定向
300 Multiple Choices：
301 Moved Permanently：请求的网页已永久移动到新位置
302 Found：临时性重定向，短网址就是基于这个状态码实现的。我们请求一个短网址，服务器会返回重定向，定向到新的地址
303 See Other： 临时性重定向，且总是使用 GET 请求新的 URI
304 Not Modified：代表所请求的资源并没有更新，缓存继续有效
305 Use Proxy：使用代理
306 （unused）：
307 Temporary Redirect：
4XX：客户端错误
400 Bad Request: 服务器无法理解请求的格式，客户端不应当尝试再次使用相同的内容发起请求。
401 Unauthorized: 请求未授权
402 Payment Required:
403 Forbidden: 禁止访问
404 Not Found: 找不到如何与 URI 相匹配的资源
405 Method Not Allowed:
406 Not Acceptable:
407 Proxy Authentication Required:
408 Request Timeout:
409 Conflict:
410 Gone:
411 Length Required:
412 Precondition Failed:
413 Request Entity Too Large:
414 Request-URI Too Long:
415 Unsupported Media Type:
416 Requested Range Not Satisfiable:
417 Expectation Failed:
5XX: 服务器错误
500 Internal Server Error: 服务器内部错误
501 Not Implemented:
502 Bad Gateway: 错误网关
503 Service Unavailable: 服务器端暂时无法处理请求（可能是过载或维护）
504 Gateway Timeout:
505 HTTP Version Not Supported:

# 数据结构

# 算法

## 冒泡排序

说明：
1、比较相邻两个元素，若前一个比后一个大，则交换位置
2、

```javascript
...
```

# 安全

## XSS 和 CSRF

概念：

1. XSS(Cross Site Scripting 跨站脚本)，XSS 定义的主语是“脚本”，是一种跨站执行的脚本，也就是 JavaScript 脚本，指的是在网站上注入我们的 javascript 脚本，执行非法操作。
2. CSRF（Cross-site request forgery 跨站请求伪造），CSRF 定义的主语是”请求“，是一种跨站的伪造的请求，指的是跨站伪造用户的请求，模拟用户的操作。

方式：
XSS 漏洞利用的两种方式: 1.获取用户的 cookie 信息，并发送到自己的服务器。(如：添加文章时加 script,把 cookie 发送自己服务器，利用 cookie 登陆) 2.进行站内 CSRF 攻击(通过 javascript 代码，生成一个隐藏的 img 标签，src 属性为http://www.sitename.com/delete/id/23，就会完成删除文章的操作。)
跨站的“CSRF”攻击： 1.邮件内容包含了一个隐藏的 img 标签`<img src="http://www.sitename.com/delete/id/23" style="display:none">`，用户站点保持登录状态的情况下打开这封邮件，也能成功执行删除操作。

防御：
防御 XSS 攻击可以通过以下两方面操作： 1.对用户表单输入的数据进行过滤，对 javascript 代码进行转义，然后再存入数据库；cookie 标记为 http only 2.在信息的展示页面，也要进行转义，防止 javascript 在页面上执行。对输入内容 encode ,输出到网页中。
CSRF 攻击的防御可以通过以下两方面操作： 1.所有需要用户登录之后才能执行的操作属于重要操作，这些操作传递参数应该使用 post 方式，更加安全； 2.为防止跨站请求伪造，我们在某次请求的时候都要带上一个`csrf_token`参数，用于标识请求来源是否合法，`csrf_token`参数由系统生成，存储在 SESSION 中。跨域提交 form 的 action

# Node

## API

require: node 和 es6 都支持的引入
export / import : 只有 es6 支持的导出引入
module.exports / exports: 只有 node 支持的导出

# 优化

# 开放

## 你觉得你最大的优势(可以多个)是什么？你为什么选择前端？
