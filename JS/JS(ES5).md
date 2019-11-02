## json 与 jsonp（jsonp底层原理）

JSON是一种轻量级的数据传输格式

【[js---JSONP原理及使用](https://www.cnblogs.com/lanyueff/p/7771075.html)】

1、利用<script>标签没有跨域限制的“漏洞”（历史遗迹啊）来达到与第三方通讯的目的。当需要通讯时，本站脚本创建一个<script>元素，地址指向第三方的API网址，形如： 

<script src="http://www.example.net/api?param1=1&param2=2"></script> 
**Web页面上调用js文件时则不受是否跨域的影响**（不仅如此，我们还发现凡是拥有”src”这个属性的标签都拥有跨域的能力，比如<\script>、<\img>、<\iframe>）

2、并提供一个**回调函数来接收数据**（函数名可约定，或通过地址参数传递）。 所以在**发送请求之前必须声明一个函数**，并且函数的名字与参数中传递的名字要一致。
3、第三方产生的响应**为json数据的包装**（故称之为jsonp，即json padding），形如： 
callback({"name":"hax","gender":"Male"}) 
这样浏览器会调用callback函数，并传递解析后json对象作为参数。**本站脚本**可在callback函数里处理所传入的数据。

JSONP将**访问跨域请求**变成了执行**远程JS**代码，服务端不再返回JSON格式的数据，**而是返回了一段将JSON数据作为传入参数的函数执行代码。**

不支持 post 请求。

## 同源策略：

协议+主机+端口号一致。

## JSON 与 XML

与 XML 相同之处

- JSON 是纯文本
- JSON 具有"自我描述性"（人类可读）
- JSON 具有层级结构（值中存在值）
- JSON 可通过 JavaScript 进行解析
- JSON 数据可使用 AJAX 进行传输

------

与 XML 不同之处

- 没有结束标签
- 更短
- 读写的速度更快
- 能够使用内建的 JavaScript eval() 方法进行解析
- 使用数组
- 不使用保留字

## 为什么javascript是单线程？**任务队列**

## call apply bind 区别

https://www.cnblogs.com/cosiray/p/4512969.html

## 创建对象

https://www.cnblogs.com/juggdxy/p/8245491.html

**new + Object，字面式创建对象：**以上两种方法在使用同一接口创建多个对象时，会产生大量重复代码，为了解决此问题，工厂模式被开发。

**工厂模式：**instanceof无法判断它是谁的实例，只能判断他是对象，构造函数都可以判断出。工厂模式解决了重复实例化多个对象的问题，但没有解决对象识别的问题（但是工厂模式却无从识别对象的类型，因为全部都是Object，不像Date、Array等，本例中，得到的都是o对象，对象的类型都是Object，因此出现了构造函数模式）。

**构造函数模式：**

以此方法调用构造函数步骤 {

​	1、创建一个新对象

​	2、将构造函数的作用域赋给新对象（将this指向这个新对象）

​	3、执行构造函数代码（为这个新对象添加属性）

​	4、返回新对象

}

对比工厂模式有以下不同之处：

- 没有显式地创建对象
- 直接将属性和方法赋给了 this 对象
- 没有 return 语句

构造函数也有其缺陷，每个实例都包含不同的Function实例（ 构造函数内的方法在做同一件事，但是实例化后却产生了不同的对象，方法是函数 ，**函数也是对象**）详情见构造函数详解，因此产生了原型模式。

**原型模式：**原型模式的好处是所有对象实例共享它的属性和方法（即所谓的共有属性），此外还可以如代码第16,17行那样设置实例自己的属性（方法）（即所谓的私有属性），可以覆盖原型对象上的同名属性（方法）。

**混合模式（构造函数模式+原型模式）**：

构造函数模式用于定义实例属性，原型模式用于定义方法和共享的属性。混合模式共享着对相同方法的引用，又保证了每个实例有自己的私有属性。

```js
 1 function Person(name,age,family){
 2     this.name = name;
 3     this.age = age;
 4     this.family = family;
 5 }
 6 
 7 Person.prototype = {
 8     constructor: Person,  //每个函数都有prototype属性，指向该函数原型对象，原型对象都有constructor属性，这是一个指向prototype属性所在函数的指针
 9     say: function(){
10         alert(this.name);
11     }
12 }
13 
14 var person1 = new Person("lisi",21,["lida","lier","wangwu"]);
15 console.log(person1);
16 var person2 = new Person("wangwu",21,["lida","lier","lisi"]);
17 console.log(person2);
```

## JS数组，字符串方法

## 闭包+作用域链

闭包是指有权访问另一个函数作用域中变量的函数。

创建闭包的最常见的方式就是在一个函数内创建另一个函数，通过另一个函数访问这个函数的局部变量,利用闭包可以突破作用链域，将函数内部的变量和方法传递到外部。

它的最大用处有两个，一个是前面提到的可以读取函数内部的变量（通过闭包调用局部变量），另一个就是让这些变量的值始终保持在内存中（让局部变量始终保存在内存中）。

作用域：指有效范围，定义了变量或者函数有权访问的其他数据。有局部和全局之分。

当代码在一个环境中执行时，会创建变量对象的一个**作用域链**，作用域链的用途是保证对有权访问的所有变量的有序访问。

## this的值

## call/apply

## 正则

## DOM相关

事件

```JS
var input = document.getElementById('d');
        input.onmousedown = function(){
            console.log("onmousedown");
        }
        input.onmouseup = function(){
            console.log("onmouseup");
        }
        input.onclick = function(){
            console.log("onclick");
        }
        input.onfocus = function(){
            console.log("onfocus");
        }
```

顺序：onfocus=>onmousedown=>onmouseup=>onclick

## 运算符

```js
console.log(1 + '2' + '2'); // 123
console.log(1 + +'2' + '2'); // 32
console.log(1 + -'1' + '2'); // 02
console.log(+'1' + '1' + '2'); // 112
console.log('A' - 'B' + '2'); // NaN2
console.log('A' - 'B' + 2); // NaN
console.log('1'-1); // 0
```

## 类数组对象

JavaScript中，数组是一个特殊的对象，其property名为正整数，且其length属性会随着数组成员的增减而发生变化，同时又从Array构造函数中继承了一些用于进行数组操作的方法。而对于一个普通的对象来说，如果它的所有property名均为正整数，同时也有相应的length属性，那么虽然该对象并不是由Array构造函数所创建的，它依然呈现出数组的行为，在这种情况下，这些对象被称为“类数组对象”。

- DOM方法 `document.getElementsByClassName()` 的返回结果（实际上许多DOM方法的返回值都是类数组）；
- 函数内部特殊变量 `arguments` 对象；
- input的文件对象FileList。

## 遍历

**for…of**（ES6，循环数组的元素值）是循环数组（对象数组）的；
        **for…in**循环数组索引、对象的属性（key），但使用 for…in 原型链上的所有属性都将被访问，用 **hasOwnProperty()** 方法解决。