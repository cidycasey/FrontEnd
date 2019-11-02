## let const var区别

1. const定义的变量不可以修改，而且必须初始化
2. var定义的变量可以修改，如果不初始化会输出undefined，不会报错。变量声明提升。
3. let是块级作用域，函数内部使用let定义后，对函数外部无影响。

**var** 声明一个变量，并可选地将其初始化为一个值。

**let** 声明一个块级作用域的本地变量，并且可选的将其初始化为一个值。**不存在变量提升**；

**暂时性死区；**

只要块级作用域内存在let命令，它所声明的变量就会“绑定”这个区域，不在受外部的影响。

**如果区块中存在let和const命令**，这个区块对这些命令声明的变量，从一开始就形成封闭作用域。

**凡是在声明之前就使用这些变量，就会报错**。这在语法上称为“暂时性死区”(简称TDZ)。

```js
var a = 45;
{
    console.log(a);
    let a = 23;
    console.log(a);
}
console.log(a);
```

**不允许重复声明**；let 不允许在相同作用域内重复声明同一个变量。

- 比较隐蔽的“死区”。

```jsx
function bar(x = y, y = 2){
  return [x, y];
}
bar();  //报错

//因为参数x的默认值等于另一个参数y，而此时y还没有声明，属于死区，
//如果y的默认值为x，就不会报错，例如：

function bar(x = 2, y = x){
  return [x, y];
}
bar();  //[2, 2]
```

ES6的**块级作用域，**块级声明用于声明在指定的作用域外无法访问的变量。块级作用域存在于：

- 函数内部
- 块中(字符{和}之间的区域)

**const** 声明一个只读的常量。一旦声明，常量的值就不能改变。const 命令实际上保证的并不是变量的值不得改动，而是变量指向的那个内存地址不得改动。

从es6开始，全局变量(例如：import、class、let、const声明的)将逐步与顶层对象的属性隔离（不再默认绑定window）。

```jsx
//顶层对象中的var
var foo = {name: 'lee', age: 18};
cosole.log(foo === window.foo);  //true
console.log(window.foo);   //{name: 'lee', age: 18}
//顶层对象中的let和const
let obj = {height: 182, weight: 140};
const mt = {msg:＇hello’};
console.log(obj === window.obj);  //false
console.log(window.obj);   //undefined
console.log(mt === window.mt);   //false
console.log(window.mt);      //undefined
```

## JSON

JSON对象：

- encodeURIComponent

- json 的标准写法：1、只能用双引号。2、key 和 value 必须是字符串（加双引号）
- 两个方法：JSON.stringify(); JSON.parse();

简写：

- key 和 value 一样的时候，可以简写

  ```js
  let a = 1;
  let b = 2;
  let json = {a, b, c:15 };
  console.log(json); // { a: 1, b: 2, c: 15 }
  ```

- 函数的简写

  ```js
  let json2 = {
      // show : function(){
      //     console.log("show");
      // },
      show(){
          console.log("show");
      }
  }
  ```

## Promise

异步请求：操作之间没有关系，同时可以进行多个操作。代码更复杂。

同步：同时只能做一件事

回调地域。

Promise——消除异步操作

- 用同步的方式来书写异步代码。

Promise 怎么使用：

```js
let p = new Promise(function(resolve,reject){
	// 异步代码
    // resove 成功了
    // reject 失败了
	p.then(function(arr){
        console.log("success");
    }, function(err){
        console.log("fail");
    });// resolve, reject
});
function createPromise(url){
    return new Promise(function(resolve, reject){
        $.ajax({
            url: 'data/arr.txt',
            dataType: 'json',
            success(arr){
                resolve(arr);
            },
            error(err){
                reject(err);
            }
        })
    })
}
Promise.all([p1, p2]).then(function(arr){
    let [res1, res2] = arr;// 解构赋值
    console.log("全都成功了");
},function(){
    console.log("至少一个失败了");
})
```





