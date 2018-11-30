# 基础

# 变量类型和计算

## JS 中使用 typeof 能得到哪些类型（考查变量类型）

### typeof 的类型(六种)

只能区分值类型的类型，区分不了引用，只能区分函数

```js
typeof undefined; //undefined
typeof "abc"; //string
typeof 123; //number
typeof true; //boolean
typeof {}; //object
typeof []; //object
typeof null; //object
typeof console.log; //function
```

## 何时使用===何时使用== （变量转换）

### 变量计算 （强类型转换）

出现情况：

- 字符串拼接

```js
100 + 10; //110
100 + "10"; //10010
```

- == 运算符

```js
100 == "100"; //true
0 == ""; //true 都转为false
null == undefined; //true 都转为false
```

- if 语句

```js
var a = "";
if (a) {
}

//if的false：0、NaN、''、null、false
```

- 逻辑运算

```js
10 && 0; //0 返回第一份false
"" || "abc"; // 'abc' 返回第一个true
!widow.abc; //true
!!abc; // 判断true or false
```

- ===全等运算符

```js
if (obj.a == null) {
  //这里相当于obj.a===null || obj.a===undefined ，的简写
  //这是jq源码推荐的写法
}
//除此之外都用===
```

## JS 有哪些内置函数 (数据封装类对象)

Object
Array
Boolean
Number
String
Function
Date
RegExp
Error

## JS 变量按照存储方式区分为哪些类型，并描述其特点

- 值类型（栈）值拷贝
- 引用类型（堆：对象、数组、函数）指针指向

## 如何理解 JSON

JSON 只不过是一个 JS 的对象而已，内置对象 /数据格式

```js
JSON.stringify({ a: 1 }); //对象转json字符串
JOSN.parse('{"a":1}'); //json字符串转js对象
```

# 原型与原型链

- 构造函数

```js
// 构造函数：大写定义的函数，
function Foo(name, age) {
  this.name = name;
  this.age = age;
  this.class = "class-1";
  // return this;        //默认有这一行
}
var f = new Foo("zhangsan", 20);
var f1 = new Foo("lisi", 22); //创建多个对象实例
```

- 构造函数-扩展

  - var a={} 其实是 var a=new Object()的语法糖，Object()是对象的构造函数。
  - var a=[] 其实是 var a=new Array()的语法糖
  - function Foo(){...} 其实是 var Foo=new Function(...)的语法糖
  - 使用 instanceof 判断一个函数是否是一个变量的构造函数

- 原型规则和示例
  - 所有的引用类型（数组、对象、函数），都具有对象的特性，即可自由扩展属性（除了"null"以外）
  - 所有引用类型（数组、对象、函数），都有一个`__proto__`（隐式原型）属性，属性值是一个普通的对象
  - 所有的函数,都有一个 prototype 属性（显式原型），属性值也是一个普通对象
  - 所有的引用类型（数组、对象、函数），`__proto__`属性值都指向它的构造函数的“prototype”属性值
  - 当试图得到一个对象的某个属性时，如果这个对象本身没有这个属性，那么它会去它的`__proto__`（即它的构造函数的 prototype）中寻找。

```js
var obj = {};
obj.a = 100;
var arr = [];
arr.a = 100;
function fn() {}
fn.a = 100;

console.log(obj.__proto__);
console.log(arr.__proto__);
console.log(fn.__proto__);

console.log(fn.prototype);

console.log(obj.__proto__ === Object.prototype); // Object 是构造函数
```

补充：

- 示例 （this）
- 原型链

```js
//构造函数
function Foo(name, age) {
  this.name = name;
}
Foo.prototype.alertName = function() {
  alert(this.name);
};
//创建实例
var f = new Foo("zhangsan");
f.printName = function() {
  console.log(this.name);
};
//测试
f.printName(); //zhangsan
f.alertName(); //zhangsan
f.toString(); // 要去 f.__proto__.__proto__中查找 即在Object.prototype中查找
```

```js
//f对象的隐式原型等于Foo构造函数的显式原型
f.__proto__ === Foo.prototype,
  //Foo.prototype是一个对象，此对象的隐式原型等于Object构造函数的显式原型
  Foo.prototype.__proto__ === Object.prototype,
  //Object.prototype是一个对象，此对象的隐式原型等于null
  Object.prototype.__proto__ === null;
```

- 循环对象自身的属性

```js
var item;
for (item in f) {
  //高级浏览器已经在for in 中屏蔽了来自原型自身的属性
  //但是这里建议大家还是加上这个判断，保证程序的健壮性
  // 是否为f的自身属性
  if (f.hasOwnProperty(item)) {
    console.log(item);
  }
}
```

## 如何判断一个变量时数组类型 （`typeof instanceof`）

- instanceof
  用于判断引用类型属于哪个构造函数的方法
  - `f instanceof Foo`的判断逻辑是：
  - f 的`__proto__`一层一层往上，能否对应到`Foo.prototype`
  - 再试着判断`f instanceof Object`

## 写一个原型链继承的例子

```js
//动物
function Animal() {
  this.eat = function() {
    console.log("animal eat");
  };
}
// 狗
function Dog() {
  this.bark = function() {
    console.log("dog bark");
  };
}
//
Dog.prototype = new Animal();

var hashiqi = new Dog();

//接下来的代码演示时，会推荐更加贴近实战的原型继承示例！

// 高端
```

- 一个比较完美的 js 继承方式

```js
//一个完善的js继承写法
//定义一个
function Parent() {
  this.name = "parent";
  this.colors = ["red", "bule", "yellow"];
}
Parent.prototype.sex = "man";
Parent.prototype.say = function() {
  console.log("say hello");
};
//
function Child() {
  //构造函数
  Parent.call(this);
  this.type = "child";
}
//Child构造函数的原型对象为父级构造函数的原型对象
Child.prototype = Object.create(Parent.prototype);
//Child对象的构造函数为Child，避免前面原型链赋值后构造函数为父级的构造函数
Child.prototype.constructor = Child;
```

- ES6 的方式实现对象继承

```js
//es6方式定义一个class
class Parent {
  //定义构造函数
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }
}
//定义子类 并继承父类
class Child extends Parent {
  constructor(x, y, colors) {
    super(x, y); //调用父类的constructor(x,y)
    this.colors = colors;
  }
  //函数
  toString() {
    return this.colors + "" + super.toString(); //调用父类的toString 方法
  }
}
```

## 描述 new 一个对象的过程

- 创建一个新对象,这步是把一个空的对象的 proto 属性设置为 F.prototype 。
- this 指向这个新对象,函数 F 被传入参数并调用，关键字 this 被设定为该实例。
- 执行代码，即对 this 赋值
- 返回 this,返回实例

```js
var f = new F();
var o = new Object();
o.__proto__ = F.prototype;
F.call(o);
var f = o;
```

## zepto（或其他框架中）源码中如何使用原型链

- 阅读源码时高效提高技能的方式
- 但不能“埋头苦钻”有技巧在其中

## 写一个 dom 查询

```js
function Elem(id) {
  this.elem = document.getElementById(id);
}
// 创建dom
Elme.prototype.html = function(val) {
  var elem = this.elem;
  if (val) {
    elem.innerHTML(val);
    return this; //链式调用
  } else {
    return elem.innerHTML;
  }
};

// 绑定事件
Elem.prototype.on = function(type, fn) {
  var elem = this.elem;
  elem.addEventListener(type, fn);
  return this; //链式
};

var div1 = new Elem("div1");
//console.log(div1.html());
div1
  .html("<p>hello</p>")
  .on("click", function() {
    console.log("click me!");
  })
  .html("<p>clicked</p>");
```

# 作用域和闭包

```js
// 声明提前
fn();
// 函数声明
function fn() {}

fn1(); //报错，undefined
//函数表达式
var fn1 = function() {};
```

## 变量提升的理解

- 变量声明
- 函数声明（注意和函数表达式的区别）

- 执行上下文
  - 范围：一段`<script>`或者一个函数
  - 全局：变量定义、函数声明 (一段`<script>`)
  - 函数：变量定义、函数声明、this、arguments (函数)

```js
console.log(a)       //undefined
var a=100;

fn('zhangsan');  // 'zhangsan' 20
function(name){
	age=100;
	console.log(name,age);
	var age;
}
```

## 说明 this 几种不同的使用场景

答：作为构造函数执行

- this
  - this 要在执行时才能确认值，定义时无法确认
    `js var a={ name:'A', fn:function(){ console.log(this.name); } } a.fn(); //this===a a.fn.call({name:'B'}); //this==={name:'B'} var fn1=a.fn; fn1(); //this===window`
  - 作为构造函数执行
    ```js
    function Foo(name) {
      //构造函数
      //this={};
      this.name = name;
      //return this;
    }
    ```
  - 作为对象属性执行
    ```js
    var obj={
        name:'A';
        printName:function(){
          //对象属性
         console.log(this.name);
         //
        }
    }
    obj.printName();
    ```
  - 作为普通属性执行
    ```js
    function fn() {
      console.log(this); //this===window
    }
    //
    fn();
    ```
  - call apply bind
    ```js
    function fn1(name) {
      alert(name); //zhangsan
      console.log(this); //this==={x:100}
    }
    //
    fn1.call({ x: 100 }, "zhangsan"); //
    //bind 要使用函数声明的格式定义
    var fn2 = function(name) {
      alert(name); //zhangsan
      console.log(this); //this==={x:100}
    }.bind({ x: 100 });
    //
    fn2("zhangsan"); //
    ```

## 创建 10 个<a>标签，点击的时候弹出来对应的序号

```js
var i;
for (i = 0; i < 10; i++) {
  (function(i) {
    //函数作用域
    document.createElement("a");
    a.innerHTML = i + "<br/>";
    a.addEventListener("click", function(e) {
      e.preventDefault();
      alert(i); //自由变量，向上查找作用域，运行时去查找，
    });
    document.body.appendChild(a);
  })(i); //
}
```

## 如何理解作用域

答：自由变量、作用域链，即自由变量的查找、闭包的两个场景

- 作用域
  - js 没有块作用域
  - 函数和全局作用域

```js
//无块作用域
if (true) {
  var name = "zhangsan";
}
console.log(name); //zhangsan

// 函数和全局作用域
var a = 10;
function fn() {
  var a = 200;
  console.log("fn", a);
}
console.log("global", a);
fn();
```

- 作用域链

```js
//定义的时候确定，哪里运行不影响
var a = 100;
function fn() {
  var b = 200;

  //当前作用域没有定义的变量，即“自由变量”
  console.log(a); //当前作用域没有该变量，向其父级作用域链式向上查找
  console.log(b);
}
fn();
```

- 闭包
  使用场景：
  - 函数作为返回值
  - 函数作为参数传递

```js
//1.函数作为返回值
function F1() {
  var a = 100;
  //返回一个函数（函数作为返回值）
  return function() {
    console.log(a); //父作用域中寻找
  };
}
var f1 = F1(); //得到一个返回的函数
var a = 200;
f1(); //执行该函数

//2.作为参数传递
function F1() {
  var a = 100;
  //返回一个函数（函数作为返回值）
  return function() {
    console.log(a); //父作用域中寻找
  };
}
var f1 = F1(); //得到一个返回的函数
function F2(fn) {
  var a = 200;
  fn(); //运行不改变
}
F2(f1); // 100  去声明时的作用域找
```

## 实际开发中闭包的使用

答：主要用于封装变量，收敛权限

```js
//在函数外，无法修改_list的值
function isFirstLoad() {
  var _list = []; //私有
  return function(id) {
    if (_list.indexOf(id) >= 0) {
      return false;
    } else {
      _list.push(id);
      return true;
    }
  };
}
var firstLoad = isFirstLoad();
firstLoad(10); //true
firstLoad(10); //false
firstLoad(20); //true
```

# 异步和单线程

## 同步和异步的区别是什么，分别举一个同步和异步的例子

- 同步会阻塞代码执行，而异步不会
- alert 是同步，setTimeout 是异步

### 知识点

- 什么是异步（对比同步）
  是否阻塞程序运行，

```js
console.log(100);
alert(101); //阻塞
//不阻塞,单线程只能等程序执行完后执行
setTimeout(function() {
  console.log(200);
}, 1000);
console.log(300);
// 100 300 200
```

- 异步和单线程
  - 执行第一行，打印 100
  - 执行 setTimeout 后，传入 setTimeout 的函数会被暂存起来，不会立即执行（单线程的特定，不能同时干两件事）
  - 执行最后一行，打印 300
  - 待所有程序执行完，处于空闲时，会立马看又没暂存起来的要执行。
  - 发现暂存起来的 setTimeout 中的函数无需等待时间，就立即执行。

## 一个关于 setTimeout 的笔试题

## 前端使用异步的场景有哪些

- 前端异步使用的场景
  在可能发生等待的情况、等待过程中不能像 alert 一样阻塞程序、因此，所有的“等待的情况”都需要异步。ajax 请求也是等待请求还来后，在调用回调函数。回调函数，触发解封时执行。
  - 定时任务：setTimeout,setInterval
  - 网络请求：ajax 请求，动态`<img>` 加载(onload)
  - 事件绑定

# 其他知识

## 知识点

日期 API：

```js
Date.now(); //获取当前时间毫秒数
var date = new Date();
date.getTime(); //获取当前时间毫秒数
date.getFullOYear(); //获取当前时间的年份
date.getMonth(); //月（0-11）+1为当前月
date.getDate(); //日（0-30）
date.getHours(); //小时（0-23）
date.getMinutes(); //分钟（0-59）
date.getSeconds(); //秒 （0-59）
```

Math 的 API

```js
Math.random(); //获取随机数 默认0-1间随机
```

数组 API:
forEach:遍历所有元素

```js
var arr=[1,2,3];
arr.forEach(function(item,index)){
	//遍历数组的所有元素
	console.log(index,item); //0 1
}
```

every:判断所有元素是否符合条件

```js
var arr=[1,2,3];
var result=arr.every(function(item,index)){
	//用来判断是否所有的数组元素，都满足一个条件
	if(item<4){
	    return true;
	}
}
console.log(result); //true
```

some：判断是否有至少一个元素符合条件

```js
var arr=[1,2,3];
var result=arr.some(function(item,index)){
	//用来判断所有的数组元素，只需一个满足条件即可
	if(item<4){
	    return true;
	}
}
console.log(result); //true
```

sort:排序

```js
var arr=[1,4,2,3,5];
var arr1=arr.sort(function(a,b)){
	//从小到大
	return a-b;
	//从大到小
	//return b-a;
}
console.log(arr1);
```

map:对元素重新组装，生成新的数组

```js
var arr=[1,2,3,4];
var arr1=arr.map(function(item,index)){
	//将元素重新组装，并返回
	return '<b>'+item+'</b>';
}
console.log(arr1);
```

filter：过滤符合条件的元素

```js
var arr=[1,2,3];
var arr1=arr.every(function(item,index)){
	//通过某一个条件过滤数组
	if(item>=2){
	    return true;
	}
}
console.log(arr1);
```

对象 API:

```js
var obj = {
  x: 100,
  y: 200,
  x: 300
};
var key;
for (key in obj) {
  //注意这里的hasOwnProperty,
  //判断属性是否是对象的自己的属性
  if (obj.hasOwnProperty(key)) {
    console.log(key, obj[key]);
  }
}
```

## 题目

- 获取 20xx-xx-xx 格式的日期

```js
function formatDate(dt) {
  if (!dt) {
    dt = new Date();
  }
  var year = dt.getYear();
  var month = dt.getMonth() + 1;
  var date = dt.getDate();
  if (month < 10) {
    //强转
    month = "0" + month;
  }
  if (date < 10) {
    date = "0" + date;
  }
  //
  return year + "-" + month + "-" + date;
}
```

- 获取随机数，要求是长度一致的字符串格式

```js
var random = Math.random();
// 加10个0字符串，保证至少10位
var random = random + "0000000000";
//取前10位
var random = random.slice(0, 10);
console.log(random);
```

- 写一个能遍历对象和数组的通用的 forEach 函数

```js
function forEach(obj, fn) {
  var key;
  //是否为数组
  if (obj instanceof Array) {
    obj.forEach(function(item, index) {
      fn(index, item);
    });
  } else {
    //对象
    for (key in obj) {
      fn(key, obj[key]);
    }
  }
}

var arr = [1, 3, 4];
// 数组
forEach(arr, function(index, item) {
  console.log(index, item);
});
var obj = { x: 100, y: 300 };
forEach(obj, function(key, value) {
  console.log(key, value);
});
```

# JS Web API

- 常说的 JS(浏览器执行的 JS)包含两部分：
  - JS 基础知识（ECMA262 标准）
  - JS-Web-API(W3C 标准)

## DOM(Document Object Model)操作

### 知识点

- DOM 的本质
  - 树，浏览器把拿到的 html 代码，结构化为一个浏览器可识别的模型。
- DOM 结构
- 获取 DOM 节点

```js
document.getElementById("div1"); //元素
document.getElementsByTagName("div"); //集合
document.getElementsByClassName(".contact"); //集合
document.querySelectorAll("p"); //集合
```

- property(js 对象的属性)

```js
var plist = document.querySelectorAll("p");
var p = plist[0];
p.style.width = "100px"; //修改样式
p.className = "p1"; //修改class
// 获取class,nodeName,nodeType
console.log(p.className, p.nodeName, p.nodeType);
```

- Attribute(HTML 代码文档中的属性)

```js
var plist = document.querySelectorAll("p");
var p = plist[0];
p.getAttribute("data-name"); //获取
p.setAttribute("data-name", "hello"); //修改
p.getAttribute("style"); //获取
p.setAttribute("style", "font-size:30px;"); //修改
```

- DOM 结构操作

```js
//新增节点
var p1 = document.createElement("p"); //创建 p元素
p.innerHTML = "this is p1"; //设置p元素的内容
div1.appendChild(p1); //添加新创建的元素

var p2 = document.getElementById("p2");
div1.appendChild("p2"); //把p2节点从原来位置移动到当前位置

//获取父元素
var parent = div1.parentElment;
//获取子元素
var child = div1.childNodes;
console.log(child[0].nodeType, child[0].nodeName); //元素间有字符 3 #text
console.log(child[0].nodeType, child[1].nodeName); //元素 1 P
//删除节点
div1.removeChild(child[0]); //#text 删除会报错，删除child[1] 成功。
```

### 题目

- DOM 是哪种基本的数据结构?
  树，
- DOM 操作的常用的 API 有哪些

- DOM 节点的 attr 和 property 有何区别
  - property 只是一个 JS 对象的属性修改
  - Attribute 是对 html 标签的属性更改

## BOM(Browser Object Model)

### 知识点

- navigator

```js
var ua = navigator.userAgent;
var isChrome = ua.indexOf("Chrome");
```

- screen

```js
console.log(screen.width);
cosnole.log(screen.height);
```

- loaction

```js
console.log(location.href); //url
cosnole.log(location.protocol); // 协议：http，https
cosnole.log(location.pathname); //路径
cosnole.log(location.search);
cosnole.log(location.hash);
```

- history

```js
history.go(-1);
history.back();
history.forward();
```

### 题目

- 如何检测浏览器类型

- 拆解 url 的各部分

# 事件

## 知识点

- 通用事件绑定

```js
var btn = document.getElementById("btn1");
//
btn.addEventListener("click", function(event) {
  console.log("clicked");
});
//通用事件绑定，IE低版本使用attachEvent
function bindEvent(elem, type, fn) {
  elem.addEventListener(type, fn);
}
// 使用
bindEvent(btn, "click", function(e) {
  // 阻止默认行为
  e.preventDefault();
  alert("clicked");
});
```

- IE 低版本使用 attachEvent 绑定事件，和 W3C 标准不一样

- 事件冒泡

  - 父元素绑定事件，点击子元素会触发父元素绑定的事件。
  - 阻止事件冒泡（e.stopPropagation()）,执行了子元素的绑定事件后，不在向上冒泡。

- 代理
  - 很多子元素需要绑定同一事件，则可以在其父元素中绑定该事件，然后用 e.tagert 获取当前操作元素，进行下一步操作

## 题目

- 编写一个通用的事件监听函数

```js
// 通用事件绑定（代理）
function bindEvent(elem, type, selector, fn) {
  if (fn == null) {
    fn = selector;
    selector = null;
  }
  elem.addEventListener(type, function(e) {
    var target;
    if (selector) {
      target = e.target;
      if (target.matches(selector)) {
        fn.call(target, e);
      }
    } else {
      fn(e);
    }
  });
}

// 使用
bindEvent(div1, "click", "a", function(e) {
  console.log(this.innerHTML);
});
```

- 描述事件冒泡流程
  - DOM 树形结构
- 对于一个无线下拉加载图片的页面，如何给每个图片绑定事件
  使用代理，代理简洁代码，提高性能，优化内存

# Ajax

## 知识点

### XMLHttpRequest

```js
var xhr = new XMLHttpRequest();
xhr.open("GET", "/api", false);
xhr.onreadystatechange = function() {
  // 这里的函数异步执行，可参考之前JS基础中的异步模块
  if (xhr.readyState == 4) {
    if (xhr.staus == 200) {
      alert(xhr.responseText);
    }
  }
};

xhr.send(null);
```

- IE 低版本使用 ActiveXObject，和 W3C 标准不同
- 状态码说明
  (xhr.readyState)
  - 0-（未初始化）还没有调用 send()方法
  - 1-（载入）已调用 send()方法，正在发送请求
  - 2-（载入完成）send()方法执行完成，已经接收到全部响应内容
  - 3-（交互）正在解析响应内容
  - 4-(完成)响应内容解析完成，可以在客户端调用了
    xhr.staus
  - 2xx-表示成功处理请求
  - 3xx-需要重定向，浏览器直接跳转
  - 4xx-客户端请求错误
  - 5xx-服务器端错误

### 跨域

- 什么是跨域：浏览器同源策略，不允许 ajax 访问其他域接口
- 条件：协议、域名、端口，有一个不同就算跨域
- 三个可以跨域的标签：img、script、link
- 三个标签场景：
  - img 用于打点统计，统计网站可能是其他域
  - link,script 可以使用 CDN,CDN 的也是其他域
  - script 可以用于 JSONP

## 题目

- 手写一个 ajax，不依赖第三方库
- 跨域的几种实现方式
  - JOSNP 原理是：动态插入 script 标签，通过 script 标签引入一个 js 文件，这个 js 文件载入成功后会执行我们在 url 参数中指定的函数，并且会把我们需要的 json 数据作为参数传入。
  - 服务器端设置 http header
    ```nodejs
      //第二个参数填写允许跨域的域的名称，不建议写“*”
      respone.setHeader('Access-Control-Allow-Origin','http://a.com,http://b.com')
      respone.setHeader('Access-Control-Allow-Headers','X-Requested-With')
      respone.setHeader('Access-Control-Allow-Methods','PUT,GET,POST,OPTIONS,DELETE')
      //接受跨域cookie
      respone.setHeader('Access-Control-Allow-Credentials','true')
    ```
  - 反向代理

# 存储

- 描述 cookie,sessionStorage 和 localStorage 的区别
  共同点：都是保存在浏览器端，且同源的。
  区别：（容量，是否携带 ajax,api 易用）

1. cookie 数据始终在同源的 http 请求中携带（即使不需要），即 cookie 在浏览器和服务器间来回传递。而 sessionStorage 和 localStorage 不会自动把数据发给服务器，仅在本地保存。cookie 数据还有路径（path）的概念，可以限制 cookie 只属于某个路径下。
2. 存储大小限制也不同，cookie 数据不能超过 4k，同时因为每次 http 请求都会携带 cookie，所以 cookie 只适合保存很小的数据，如会话标识。sessionStorage 和 localStorage 虽然也有存储大小的限制，但比 cookie 大得多，可以达到 5M 或更大。
3. 数据有效期不同，sessionStorage：仅在当前浏览器窗口关闭前有效，自然也就不可能持久保持；localStorage：始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据；cookie 只在设置的 cookie 过期时间之前一直有效，即使窗口或浏览器关闭。
4. 作用域不同，sessionStorage 不在不同的浏览器窗口中共享，即使是同一个页面；localStorage 在所有同源窗口中都是共享的；cookie 也是在所有同源窗口中都是共享的。
5. Web Storage 支持事件通知机制，可以将数据更新的通知发送给监听者。
6. Web Storage 的 api 接口使用更方便。

- ios safari 隐藏模式下，localStorage.getItem 会报错，建议使 try-catch 封装

# 开发环境

## IDE

## git(代码管理，多人协作)

## JS 模块化

- AMD
  异步加载 js
- CoommonJS
  不算是异步加载 js,而是同步一次性加载

## 打包工具

webpack gulp rollup

## 上线回滚的流程

- 是非常重要的环节
- 各个公司流程不同
- 上线流程要点

  - 将测试完成的代码提交到 git 版本库的 master 中
  - 每个稳定版本一个 tag 和 branch
  - 将当前服务器的代码全部打包并记录版本号备份
  - 将 master 分支的代码提交覆盖到线上服务器，生成新的版本号

- 回滚流程要点
  - 将当前服务器的代码打包并记录版本号备份
  - 将备份的上一个版本号解压，覆盖到线上服务器，并生成新的版本号

## linux

# 运行环境

## 页面加载过程

### 知识点

- 加载资源的形式
  - 输入 url(或者跳转页面)加载 html
  - 加载 html 中的依赖静态资源（css,js）
- 加载一个资源的过程
  - 浏览器根据 DNS 服务器得到域名 IP 地址
  - 向这个 IP 的机器发送 http 请求
  - 服务器收到、处理并返回 http 请求
  - 浏览器得到返回内容
- 浏览器渲染页面的过程
  - 根据 HTML 结构生成 DOM Tree
  - 根据 CSS 生成 CSSOM
  - 将 DOM 和 CSSOM 整合形成 RenderTree
  - 根据<script>时，会执行并阻塞渲染（因为 js 可以改变 DOM 结构）

### 题目

- 从输入 url 到得到 html 的详细过程
- window.onload 和 DOMContentLoaded 的区别

```js
document.addEventListener("load", function() {
  //页面的全部资源加载完才会执行，包括图片、视频等
});

document.addEventListener("DOMContentLoaded", function() {
  //DOM渲染完成即可执行，此时图片、视频还没有加载完
});
```

## 性能优化

原则:

- 多使用内存、缓存或者其他方法
- 减少 CPU 计算、较少的网络
  从哪入手：
- 加载页面和静态资源
  - 静态资源的压缩合并
  - 静态资源缓存
  - 使用 CDN 让资源加载更快
  - 使用 SSR 后端渲染，数据直接输出到 HTML 中
  -
- 页面渲染
  - CSS 放在前面，JS 放后面
  - 懒加载（图片懒加载、下拉加载更多）
  - 减少 DOM 查询，对 DOM 查询做缓存
  - 减少 DOM 操作，多个操作尽量合并在一个执行
  - 事件节流
  - 尽早执行操作（如 DOMContentLoaded）

## 安全性

### XSS 和 CSRF

概念：

1. XSS(Cross Site Scripting 跨站脚本)，XSS 定义的主语是“脚本”，是一种跨站执行的脚本，也就是 JavaScript 脚本，指的是在网站上注入我们的 javascript 脚本，执行非法操作。
2. CSRF（Cross-site request forgery 跨站请求伪造），CSRF 定义的主语是”请求“，是一种跨站的伪造的请求，指的是跨站伪造用户的请求，模拟用户的操作。

方式：
XSS 漏洞利用的两种方式: 1.获取用户的 cookie 信息，并发送到自己的服务器。(如：添加文章时加 script,把 cookie 发送自己服务器，利用 cookie 登陆) 2.进行站内 CSRF 攻击(通过 javascript 代码，生成一个隐藏的 img 标签，src 属性为http://www.sitename.com/delete/id/23，就会完成删除文章的操作。)
跨站的“CSRF”攻击： 1.邮件内容包含了一个隐藏的 img 标签`<img src="http://www.sitename.com/delete/id/23" style="display:none">`，用户站点保持登录状态的情况下打开这封邮件，也能成功执行删除操作。

防御：
防御 XSS 攻击可以通过以下两方面操作： 1.对用户表单输入的数据进行过滤，对 javascript 代码进行转义，然后再存入数据库；cookie 标记为 http only 2.在信息的展示页面，也要进行转义，防止 javascript 在页面上执行。对输入内容 encode ,输出到网页中。
CSRF 攻击的防御可以通过以下两方面操作： 1.所有需要用户登录之后才能执行的操作属于重要操作，这些操作传递参数应该使用 post 方式，更加安全； 2.为防止跨站请求伪造，我们在某次请求的时候都要带上一个`csrf_token`参数，用于标识请求来源是否合法，`csrf_token`参数由系统生成，存储在 SESSION 中。跨域提交 form 的 action
3、增加验证流程：验证码、密码等

