# React.js-Part3

## 主要内容

1. React 组件的生命周期
2. React 中绑定this
3. 评论列表：增加评论

## 1. 组件的生命周期(生命周期钩子，钩子函数)

生命周期的概念：每个组件的实例，从 **创建**、到**运行**、直到**销毁**，在这个过程中，会出发一些列 事件，这些**事件就叫做组件的生命周期函数**；

### React组件生命周期分为三部分：

- **组件创建阶段**：特点：一辈子只执行一次

> 【componentWillMount:  组件将要挂载时执行】（挂载，把虚拟DOM树放到页面上显示），**此时虚拟DOM树没有被创建，也没有被挂载到页面上**；虚拟 DOM 是在 render 函数中创建的，所以此时还没创建虚拟DOM。
> 【render：正在渲染虚拟DOM】，内存中创建虚拟DOM，还没挂载。
> 【componentDidMount:  】组件完成挂载到页面上，此时，我们的 state 上的数据和内存中的虚拟 DOM 以及浏览器中的页面，已经完全保持一致了。
>
> 到这里，组件创建阶段的生命周期函数，已经执行完毕，页面第一次被渲染好。

- **组件运行阶段**：按需，**根据 props 属性 或 state 状态的改变**，有选择性的 执行 0 到多次

> 属性props改变【↓】
>
> 【componentWillReceiveProps: 组件将要接收新的属性。】此时只要这个方法被触发，**就证明父组件为当前组件子组件传递了新的属性。**
>
> 状态state改变【↓】
>
> 【shouldComponentUpdate: 组件是否需要被更新。】组件尚未开始更新，但 state ，props 肯定是最新的。（如果是false，回到运行中？），**页面还是旧的，数据是新的**。
> 【componentWillUpdate: 组件将要更新】组件尚未被更新，内存中的虚拟DOM还是旧的。组件的最新状态将要更新，**页面还是旧的，数据是新的。**
> 【render: 根据最新的状态，重新渲染内存的虚拟DOM元素】，当render调用完毕，内存中的旧虚拟DOM已经被替换成新的虚拟DOM树。**页面还是旧的，数据是新的**。
> 【componentDidUpdate: 组件已经完成了更新】，这时候，状态是最新的，**页面也是新的，数据是新的**

- **组件销毁阶段**：一辈子只执行一次

> 【componentWillUnmount: 组件将要被卸载】组件中的数据和方法还可以正常可用。

[vue中的生命周期图](https://cn.vuejs.org/v2/guide/instance.html#生命周期图示)
[React Native 中组件的生命周期](http://www.race604.com/react-native-component-lifecycle/)

### React生命周期图

![LifeCycle](E:\MarkDown\FrontEnd\React\images\LifeCycle.png)

### React生命周期的回调函数总结成表格：

![LifeCycleTable](E:\MarkDown\FrontEnd\React\images\LifeCycleTable-1572675969639.png)

### **组件生命周期的执行顺序：**

##### 1.**Mounting：**（无参数）

- constructor()

- componentWillMount()

- render()

- componentDidMount()

  ![1570441288561](E:\MarkDown\FrontEnd\React\images\1570441288561.png)

##### **2.Updating：**(有参数,除了render())

- componentWillReceiveProps**(nextProps)** 【this.props 是旧数据】

- shouldComponentUpdate**(nextProps, nextState)**【this.props，this.state 是旧数据】

- componentWillUpdate**(nextProps, nextState)【**this.props，this.state 是旧数据】

- render() 【this.props，this.state 是新数据】

- componentDidUpdate(prevProps, prevState)【this.props，this.state  是新数据】

  ![1570441418268](E:\MarkDown\FrontEnd\React\images\1570441418268-1572677616628.png)

##### **3.Unmounting：**

- componentWillUnmount()

## 2. 通过Counter计数器的小案例 - 了解生命周期函数 `Counter.jsx`

### defaultProps

> 在组件创建之前，会先初始化默认的props属性，这是全局调用一次，严格地来说，这不是组件的生命周期的一部分。在组件被创建并加载候，首先调用 constructor 构造器中的 this.state = {}，来初始化组件的状态。

- 在封装组件时，组件内部肯定有一些必须的参数，哪怕用户没有传递，此时需要在组件内部给自己提供一个默认值。

- 在 React 中，使用类的静态的 defaultProps 属性（值是对象），来设置组件的默认属性。

  ```jsx
      static defaultProps = {
          initCount : 0
      }
  ```

### props 类型校验

​	封装组件的目的，是为了团队协作开发，有的人负责开发组件，有的人只负责调用别人开发好的组件。最好在封装组件的时候，为组件中一些必要数据进行类型校验。

​	如果要为传递过来的属性做类型校验，必须安装 React 提供的第三方包：prop-types。

​	 prop-types 大概在 v15.* 之前并没有单独抽离出来，那时候还可 react 包 在一起；后来从 v15.*之后，官方把类型校验模块抽出出来。

1. 给 `props` 属性提供默认值 和 进行类型校验，需要先运行`cnpm i prop-types --save`

   - prop-types 包中职能很单一，只提供了一些常见的数据类型，用于做类型校验
   - ReactTypes 可以换成别的变量

   > VSCode功能：鼠标放到 ReactTypes 上，按住 Ctrl ，出现类似超链接的样式，点击可以进入包的 .ts 定义文档

   ![1570369507643](E:\MarkDown\FrontEnd\React\images\1570369507643.png)

   ```jsx
   // prop-types 包中职能很单一，只提供了一些常见的数据类型，用于做类型校验
   // ReactTypes 可以换成别的变量，如PropTypes
   import ReactTypes from 'prop-types';
   ```

   ![1570369752370](E:\MarkDown\FrontEnd\React\images\1570369752370.png)

   ![1570369781165](E:\MarkDown\FrontEnd\React\images\1570369781165.png)

   ```jsx
   // 这是创建一个静态的 propTypes(ReactTypes) 对象，在这个对象中，可以把外界传递过来的属性，做类型校验。
   // 如果要为传递过来的属性做类型校验，必须安装 React 提供的第三方包：prop-types。
   // prop-types 大概在 v15.* 之前并没有单独抽离出来，那时候还可 react 包 在一起；后来从 v15.*之后，官方把类型校验模块抽出出来。
   static propTypes = {
           initCount : ReactTypes.number // 使用 prop-types 包，来定义 initCount 为 number 类型
       }
   ```

2. 给组件的 `props` 进行类型校验，isRequired 表示 这个 props 属性值 是必须要传递的

   ```js
     //  进行 props 属性的类型校验,   static propTypes = {}  是固定写法
     static propTypes = {
       initCount: PropTypes.number.isRequired // 规定 外界在传递 initcount 的时候，必须是 number 值类型，否则 ，会在终端报警告
       // isRequired 表示 这个 props 属性值 是必须要传递的
     }
   ```

   ![1570370278336](E:\MarkDown\FrontEnd\React\images\1570370278336.png)

### 生命周期函数详解

**props 和 state 数据；内存中的虚拟DOM；页面上的DOM**

#### componentWillMount()

- 内存中的虚拟DOM还没开始创建，所以不能操作页面上的DOM元素；可以获取属性props和状态state；组件的函数也可以访问，自己的函数已经初始化完成。

```jsx
componentWillMount(){ // 等同于 Vue 中的 create()
        console.log(document.getElementById('myp')); // null
        console.log(this.props.initCount); // 0
        console.log(this.state.msg); // msg
        this.mySelfFunc(); // 自己定义的函数，和生命周期函数
    }
```

#### render()

- 当执行到这个生命周期函数时，即将要开始渲染内存中的虚拟DOM了，当这个函数执行完

```jsx
// 当执行到这个生命周期函数时，即将要开始渲染内存中的虚拟DOM了，当这个函数执行完
    render(){
        // 在 return 之前，虚拟DOM 还没开始创建，页面上也是空的，根本拿不到任何元素
        console.log(document.getElementById('myp')); // null
        return (
            <div>
                <h1>Counter 计数器</h1>
                <button>+1</button>
                <hr/>
                <p id="myp">当前的数量是：{this.props.initCount}</p>
            </div>
        )
        // 当 return 执行完毕，虚拟DOM创建好了，但是还没被挂载到页面中。
```

- 当 return 执行完毕，虚拟DOM创建好了，但是还没被挂载到页面中。

#### componentDidMount()

- 当组件挂载到页面上之后，会进入这个生命周期函数，只要进入这个生命周期函数了，说明页面上已经有可见的DOM元素了。在这个函数中，我们可以放心的去创建页面上的DOM元素了。
- 如果我们想操作DOM元素，最早只能在 componentDidMount 中进行。

```jsx
// 当组件挂载到页面上之后，会进入这个生命周期函数，只要进入这个生命周期函数了，说明页面上已经有可见的DOM元素了。
    componentDidMount(){
        // 在这个函数中，我们可以放心的去创建页面上的DOM元素了。
        // 如果我们想操作DOM元素，最早只能在 componentDidMount 中进行。
        // componentDidMount 相当于 Vue 中的 mounted 函数
        console.log(document.getElementById('myp')); // <p id="myp">...</p>
    }
    // 当组件中执行完 componentDidMount 函数后，就进入到了运行中的状态，所以 componentDidMount 是创建阶段的最后一个函数。
```

- 当组件中执行完 componentDidMount 函数后，就进入到了运行中的状态，所以 componentDidMount 是创建阶段的最后一个函数。

##### 实现点击按钮，数值加1

```jsx
	constructor(props){
        super(props);
        this.state = {
            // msg : 'msg'
            count : props.initCount // 将获取到的初始值复制给状态，不能写this（传递过来的形参）
        };
    }
    static defaultProps = { // 默认值
        initCount : 0
    }
    static propTypes = {
        initCount : ReactTypes.number // 使用 prop-types 包，来定义 initCount 为 number 类型
    }
	render(){
        return (
            <div>
                <h1>Counter 计数器</h1>
                <button onClick={() => {this.addCount()}}>+1</button>
                <hr/>
                <p id="myp">当前的数量是：{this.state.count}</p>
            </div>
        )
    }
	addCount(){
        this.setState({
            count : this.state.count + 1 // 
        })
    }
```

##### 关于绑定事件

- 是 class 内是严格模式，类中的实例方法定义在 prototype 原型上。
- Class 是 C.prototype 这么定义的语法糖，然后才有 strict 模式**不默认绑定**

```jsx
 <button onClick={console.log(this)}>+1</button> // Counter 实例对象
 <button onClick={function(){console.log(this)}}>+1</button> // undefined
 <button onClick={console.log(this.addCount)}>+1</button> // 函数源码 
 <button onClick={() => {console.log(this)}}>+1</button> // Counter 实例对象
```

**例：**类的方法内部如果有this，默认指向类的实例。 但单独使用该方法时，this 指向undefined。（类中默认使用严格模式）

```js
class Logger{
    printName(name = 'there'){
        console.log(this)
    }
}
const logger = new Logger();    //实例化
const printName = logger.printName;    //printName
printName(); // undefined
```

解决方案：

- 1、在构造方法中绑定 this ，这样实例化时 this 就会指向当前实例

```js
class Logger{
    constructor(){
        this.printName = this.printName.bind(this);
    }
}
```

- 2、使用箭头函数

```js
class Logger{
    constructor(){
        this.printName = (name = 'there') => {
            this.print(name);
        }
    }
 }
```

非严格模式：

```js
function Person(){

}
Person.prototype.say = function(){console.log(this)}
Person.prototype.show = function(){
    this.say(); // 调用实例方法要加this
}
var p = new Person();
var say = p.say;
say(); // window
```

![1570434710940](E:\MarkDown\FrontEnd\React\images\1570434710940.png)

#### shouldComponentUpdate()

- 从这里开始，就进入了组件的运行中状态
- 判断组件是否需要更新
  - shouldComponentUpdate 中要求必须返回一个布尔值
  - shouldComponentUpdate 中，则不会继续执行后续的生命周期函数，而是直接退回到了运行中的状态，此时后续的 render 函数并没有被调用，所以，页面不会被更新，组件中的 state 状态却被修改了

	```jsx
    shouldComponentUpdate(nextProps, nextState){
        // 需求：如果 state 中的 count 值是偶数时更新页面，否则不更新。
        // 经过打印测试发现，在 shouldComponentUpdate 中，通过 this.state.count 拿到的值是上一次的值，不是当前最新的。
        console.log(this.state.count + "----------" +nextState.count);// 旧值-----新值
        return nextState.count%2 == 0 ? true : false ;
        // return true;
    }
  ```

	- 需求：如果 state 中的 count 值是偶数时更新页面，否则不更新。	
	-  经过打印测试发现，在 shouldComponentUpdate 中，通过 this.state.count 拿到的值是上一次的值，不是当前最新的。

![1570437031372](E:\MarkDown\FrontEnd\React\images\1570437031372.png)

#### componentWillUpdate()

-  组件将要更新，此时尚未更新，在进入这个生命周期的时候，内存中的虚拟DOM元素，页面上的虚拟DOM元素也是旧的。
- 经过打印分析得到，此时页面上的 DOM 节点，应该慎重操作，你可能操作的是旧DOM

```jsx
	componentWillUpdate(){
        // 经过打印分析得到，此时页面上的 DOM 节点，应该慎重操作，你可能操作的是旧DOM
        // console.log("componentWillUpdate "+document.getElementById('myp').innerHTML);
        console.log("componentWillUpdate "+this.refs.p.innerHTML);
    }
```

#### render()

- 在组件运行阶段，每当调用render函数的时候，页面上的DOM元素还是之前旧的

```jsx
render(){
        // 在组件运行阶段，每当调用render函数的时候，页面上的DOM元素还是之前旧的
        console.log("render  "+(this.refs.p && this.refs.p.innerHTML));// 短路运算
        return (
            ...
        )
        /// 当 return 执行完毕，虚拟DOM创建好了，但是还没被挂载到页面中。
    }
```

#### componentDidUpdate()

- 组件完成了更新，此时state中的数据，虚拟DOM，页面上的DOM都是最新的，此时可以放心大胆的操作页面了。

```jsx
	componentDidUpdate(){
        console.log("componentWillUpdate "+this.refs.p.innerHTML); // 新数据
    }
```

#### componentWillReceiveProps() `TestReceiveProps.jsx`

- 组件将要接收外界传递过来的新的 props 属性值
- 当子组件第一次被渲染到页面上的时候，不会触发这个函数
- 只有当父组件中，通过某些事件，重新修改了传递给子组件的 props 数据之后，才会被触发componentWillReceiveProps
- 在 componentWillReceiveProps 被触发的时候，如果我们使用 this.props 来获取属性值，该数据是旧的，如果想要获取最新的属性值，需要通过 componentWillReceiveProps 参数列表来获取 nextProps

##### 父组件向子组件传值

父组件把 state 中的值通过 props 传给子组件的，当父组件的 state 改变，传递给子组件的 props 对应改变，触发**子组件的该函数**。

![1570440157786](E:\MarkDown\FrontEnd\React\images\1570440157786.png)

```jsx
import React from 'react';
// 父组件
export default class Parent extends React.Component {
    constructor(props){
        super(props);
        this.state = {
            msg : "这是父组件中的 msg 消息"
        };
    }
    render(){
        return <div>
            <h1>这是父组件</h1>
            <button onClick={this.changeMsg}>点击修改父组件的msg</button>
            <hr/>
            <Son pmsg={this.state.msg}></Son>
        </div>
    }
    changeMsg = () =>{
        this.setState({
            msg : "修改后的Msg"
        })
    }
}

// 子组件
class Son extends React.Component{
    constructor(props){
        super(props);
        this.state = {};
    }
    render(){
        return <div>
            <h1>这是子组件------{this.props.pmsg}</h1>
        </div>
    }
    // 组件将要接收外界传递过来的新的 props 属性值
    // 当子组件第一次被渲染到页面上的时候，不会触发这个函数
    // 只有当父组件中，通过某些事件，重新修改了传递给子组件的 props 数据之后，才会被触发componentWillReceiveProps
    // 在 componentWillReceiveProps 被触发的时候，如果我们使用 this.props 来获取属性值，该数据是旧的，如果想要获取最新的属性值，需要通过 componentWillReceiveProps 参数列表来获取 nextProps
    componentWillReceiveProps(nextProps){
        console.log("componentWillReceiveProps 被触发了");
        console.log(this.props.pmsg +"--------"+ nextProps.pmsg);
    }
}
```
## 3.React 中绑定 this 并传参的三种方式

这里的方法是一个普通方法，因此在触发的时候，这里的 this 是undefined

```jsx
export default class Counter extends React.Component{
    // ...
    render(){
        return (
            <div>
                <h1>Counter 计数器</h1>
                <button onClick={this.addCount}>绑定 this 并传参</button> 
                <p id="myp" ref="p">当前的数量是：{this.state.count}</p>
            </div>
        )
    }
    addCount(){
        console.log(this); // 这里的方法是一个普通方法，因此在触发的时候，这里的 this 是undefined
        this.setState({
            count : this.state.count + 1
        })
    }
}
```

### 1、在事件处理函数用 bind 中绑定this并传参：

- bind 的作用：为前面的函数修改函数内部的 this 指向，让函数内部的指向 bind 参数列表中的第一个参数。

[聊一聊call、apply、bind的区别](https://segmentfault.com/a/1190000012772040)

- **bind 和 call/apply 之间的区别：**
  - call/apply 修改完 this 指向，会立即调用前面的函数，但 bind 只是修改this指向，并不会调用。
- bind中第一个参数是用来修改 this 的指向的，后面的所有参数，都会当作将来调用前面函数时候的参数传递进去。

```jsx
<input type="button" value="在事件中绑定this并传参" onClick={this.handleMsg1.bind(this, '🍕', '🍟')} />
```

```jsx
// 在事件中绑定this并传参
    handleMsg1(arg1, arg2) {
        console.log(this);
        // 此时this是个null
        this.setState({
            msg: '在事件中绑定this并传参：' + arg1 + arg2
        });
    }
```

### 2、在构造函数中用 bind 绑定this并传参:

- bind返回值：返回指定的 this 值和初始化参数改造的**原函数拷贝**。bind**不会改变原函数的this**指向。
- 把函数变量的引用重新赋值为**原函数的拷贝值**。

```jsx
    // 修改构造函数中的代码：
constructor(){
    this.handleMsg2 = this.handleMsg2.bind(this, '🚗', '🚚');
}
render中【↓】
    <input type="button" value="在构造函数中绑定this并传参" onClick={this.handleMsg2} />

    // 在构造函数中绑定this并传参
    handleMsg2(arg1, arg2) {
        this.setState({
            msg: '在构造函数中绑定this并传参：' + arg1 + arg2
        });
    }
```

### 3、用箭头函数绑定this并传参：

```jsx
<input type="button" value="用箭头函数绑定this并传参" onClick={() => { this.handleMsg3('👩', '👰') }} />

    // 用箭头函数绑定this并传参
        handleMsg3(arg1, arg2) {
            this.setState({
                msg: '用箭头函数绑定this并传参：' + arg1 + arg2
            });
        }
// 或者
<input type="button" value="用箭头函数绑定this并传参" onClick={()=>{this.handleMsg3('👩', '👰') }} />

    // 用箭头函数绑定this并传参
    handleMsg3=(arg1, arg2) => {
         this.setState({
             msg: '用箭头函数绑定this并传参：' + arg1 + arg2
         });
    }
```

## 4.评论列表：增加评论功能

- 相对于Vue中，把父组件和子组件传递给子组件的普通属性和方法对待区别对待（普通属性用 props，方法属性用 this.$emit('方法名')）。

- React 无论值还是方法，都可以通过 props 来传递。

  ```jsx
  <CmtBox reload={this.loadCmts}></CmtBox>
  ```

- 练习在父组件向子组件传递方法` <CmtBox reloadCmts = {this.loadCmts}></CmtBox>`，子组件调用父组件的方法` this.props.reloadCmts();`。

- 使用 componentWillMount() 方法。在组件尚未渲染的时候，就立即调用数据。

  ![1570526609842](E:\MarkDown\FrontEnd\React\images\1570526609842.png)

- 发表按钮的点击事件：（localStorage 本地存储）

  ```jsx
  postComment = () => {
          // 1. 获取评论人和评论内容
          // 2. 从本地存储中，先获取之前的评论数组
          // 3. 把最新评论，unshift()到列表
          // 4. 再把最新的评论数组保存到本地存储中
          var cmtInfo = {user : this.refs.user.value, content : this.refs.content.value};
          var list = JSON.parse(localStorage.getItem('cmts') || '[]');
          list.unshift(cmtInfo);
          // localStorage.setItem('cmts', '');
          localStorage.setItem('cmts', JSON.stringify(list));
          this.refs.user.value = this.refs.content.value = '';
          this.props.reloadCmts();
      }
  ```

### 扩展：Context特性（用的不多）

- 记住一串单词组合`getChildContextTypes`
- 前3个、后3个、后两个
- 一个方法、两个静态属性

![Context特性](E:\MarkDown\FrontEnd\React\images\Context特性.png)

1. 在父组件中，定义一个 function ，这个 function 有个固定的名称叫 getChildContext，该函数必须返回一个对象，这个对象就是共享给所有子孙的数据。
2. 使用属性校验，规定传递给子组件的数据类型，需要定义一个静态的（static）childContextTypes (固定名称，不要改)
3. 先属性校验，校验一下父组件传递过来的参数类型；这里如果子组件想使用父组件通过 Context 共享的数据，在使用前一定要做一下类型校验。

```jsx
export default class Com1 extends React.Component {
    constructor(props){
        super(props);
        this.state = {
            color : 'red'
        }
    }
    // 1、在父组件中，定义一个 function ，这个 function 有个固定的名称叫 getChildContext，该函数必须返回一个对象，这个对象就是共享给所有子孙的数据。
    getChildContext(){
        return {
            color: this.state.color
        }
    }
    // 2、使用属性校验，规定传递给子组件的数据类型，需要定义一个静态的（static）childContextTypes (固定名称，不要改)
    static childContextTypes = {
        color: ReactTypes.string // 规定了传递给子组件的数据类型
    }
    render(){
        return <div>
            <h1>这是父组件Com1</h1>
            <Com2></Com2>
        </div>
    }
}
class Com2 extends React.Component {
    render(){
        return <div>
            <h3>这是父组件Com2</h3>
            <Com3></Com3>
        </div>
    }
}
class Com3 extends React.Component {
    // 3、先属性校验，校验一下父组件传递过来的参数类型
    static contextTypes = {
        color : ReactTypes.string // 这里如果子组件想使用父组件通过 Context 共享的数据，在使用前一定要做一下类型校验。
    }
    render(){
        return <div>
            <h5 style={{color: this.context.color}}>这是子组件Com3 ---- {this.context.color}</h5>
            {/* <h5>这是子组件Com3 ---- {this.context.color}</h5> */}
        </div>
    }
}
```
