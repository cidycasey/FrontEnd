# React.js-Part1

## 总述

- 介绍、React核心概念、webpack4.x、JSX语法
- 理解React中虚拟DOM的概念
- 理解React中三种Diff算法的概念
- 使用JS中createElement的方式创建虚拟DOM
- 使用ReactDOM.render方法
- 使用JSX语法并理解其本质

## 1.介绍

- React 起源于 Facebook 的内部项目，因为该公司对市场上所有 JavaScript MVC 框架，都不满意，就决定自己写一套，用来架设 Instagram（照片交友） 的网站。做出来以后，发现这套东西很好用，**就在2013年5月开源了**。
- Angular1 2009 年  谷歌    MVC  不支持 组件化开发
- 由于 React 的**设计思想极其独特**，属于革命性创新，性能出众，代码逻辑却非常简单。所以，越来越多的人开始关注和使用，认为它可能是将来 Web 开发的主流工具。
- 搞清楚两个概念：
  - library（库）：小而巧的库，Jquery，优点：船小好调头，可以很方便的从一个库切换到另外的库，但是代码几乎不会改变。
  - Framework（框架）：大而全的是框架，提供了一整套的解决方案，所以项目中间想切换到另外的框架，比较困难。

## 2.前端三大主流框架

> 三大框架一大抄

- Angular.js：出来最早的前端框架，学习曲线比较陡，NG1学习起来比较麻烦，NG2-NG5开始进行了了改革，也提供了组件化开发的概念，也支持TS（TypeScript）进行编辑。
- Vue.js：最火（关注的人比较多），中国人开发的。
- React.js：最流行（用的人比较多），设计很优秀。

## 3.React与Vue对比

### 组件化方面

1. **什么是模块化**：是从代码角度进行分析的；把一些可以复用的代码，抽离为单个的模块；便于项目的维护和开发。

2. **什么是组件化**：是从 **Ul界面 ** 的角度进行分析的；把一些可复用的Ul元素，抽离为单独的组件（放到单独的文件中）；便于项目的维护和开发。

3. **组件化的好处：**随着项目规模的增大，手里的组件越来越多；很方便就能把现有的组件拼接为一个完整的页面。

4. Vue是如何实现组件化：通过.vue文件，来创建对应的组件；

   - template 结构
   - script 行为
   - style 样式

5. React如何使用组件化：React中有组件化的概念，但没有像Vue这样的模板文件；React中一切都是以js来实现的；所以JS要合格；ES6，ES7（async和await）要会用。

### 开发团队方面

- React有FaceBook前端官方团队进行维护和更新，技术实力比较雄厚。
- Vue 第一版，由尤雨溪专门维护，2.x版本 由尤雨溪 为主的开源团队进行开发和维护。

### 社区方面

- React诞生较早，所以社区比较强大，一些常见问题、坑、最优解决方案，文档、博客在社区中都可以方便的找到。
- Vue是近两年才火起来，社区相对较小。

### 移动APP开发体验方面

- React，结合ReactNative，也无缝迁移到移动APP的开发体验（RN用的最多，也是最火最流行的，ins，facebook）。

- Vue，结合Weex，提供了迁移到移动端APP开发的体验（Weex，阿里开发的，目前只是一个小玩具，并没有很成功的大案例）。

## 4.为什么学习React？

1. 和Angualar1相比，React设计很优秀，一切基于JS并且实现了组件化开发的思想。
2. 开发团队实力强悍，不必担心断更的情况。
3. 社区和强大，很多问题都能找到对应的解决发那个。
4. 提供了无缝转到ReactNative上的开发体验，让我们的技术能力得到了拓展；增强了核心竞争力。、
5. 很多企业中，前端项目的技术选型采用的是React.js。

## 5.React中几个核心的概念（50%）

### 5.1虚拟DOM(Virtual Document Object Model)

- **DOM的本质是什么：**浏览器中的概念，浏览器提供的API，用JS对象来**表示**页面上的元素，并提供了**操作**DOM对象的API(拿到div，加上颜色);
- **什么是React中的虚拟DOM：**是框架中的概念，是程序员（封装框架的程序员）用JS对象来模拟页面上的DOM和DOM嵌套；
- 为什么要实现虚拟DOM：为了实现页面中，DOM元素的高效更新。
- DOM和虚拟DOM的区别：
  - **DOM：**浏览器中提供的概念，用JS对象，表示页面上的元素，并提供了操作元素的API。
  - **虚拟DOM：**是框架中的概念；开发框架的程序员，手动用JS对象来模拟DOM元素和嵌套关系。
    - 本质：用JS对象，来模拟DOM元素和嵌套关系。**（面试）**
    - 目的：就是为了实现页面元素的高效更新。**（面试）**

#### 需求：点击列表表头，实现对应表格数据的排序

![虚拟DOM引入图片](E:\MarkDown\FrontEnd\React\images\虚拟DOM引入图片.png)

1. 表格中数据从哪儿来：从数据库查询来的。

2. 查询到的数据，存放到哪儿了：数据在浏览器的内存中存放，而且是以对象数组的形式表示。

3. 这些数据如何渲染到页面上？
   - 方案1：手动for循环整个数组，then 手动拼接`str += ‘<tr></tr>’`
   - 方案2：使用模板引擎，art.template(?????)
   
4. 思考上述方法的性能上的问题？【↓】

5. 如果用户点击了【时间】，想按照时间从大到小排序：
   - 触发点击事件，在事件中，把内存中 对象数组，重新排序；
   - 当排完序后，页面是旧的，但内存中的 对象数组 是新的；
   - 想办法把最新的数组，重新渲染到页面上；（这一步有没有性能上的问题？）
   
6. 分析和总结：上述方案，只是实现了把数据渲染的页面上的能力，但没把性能做到最优；

7. 如何才能把性能做到最优：**按需渲染页面**（只重新渲染更新的数据对应的页面元素）。

8. **如何实现按需渲染 更新？**

   获取内存中，新旧两个DOM树，进行对比，得到需要被按需更新的DOM元素。

##### DOM树概念

一个网页的呈现过程：

- 浏览器请求服务器获取页面HTML代码。
- 浏览器要在内存中，解析DOM结构，并在浏览器内存中，渲染出一颗DOM树。
- 浏览器把DOM树呈现到页面上。

9. 如何获取新旧DOM树，从而实现DOM树的对比？

   分析：浏览器中，并没有直接提供获取DOM树的API；所以，我们无法拿到浏览器内存中的DOM树。

10. 程序员可以手动模拟新旧两棵DOM树。

11. 程序员如何手动模拟DOM树？如何模拟一个DOM元素。JS对象模拟DOM树。

```html
<div id="mydiv" title="dd">
    content
    <p>ddd</p>
</div>
```

```js
var div = {
	tagName:'div',
	attrs:{
		id:'mydiv',
		title:'dd',
		'data-index': '0'
	},
	children:[ //元素嵌套
        'content',
        {
            tagName:'p',
            attrs:{},
            children:[
                'ddd'
            ]
        }
    ]
}
```

12.程序员手动模拟的的新旧DOM树就是React中的虚拟DOM的概念。（用JS对象模拟新旧DOM树）

#### **React中虚拟DOM总结：**

- 什么是虚拟DOM：用JS对象的形式，来模拟页面上DOM嵌套关系（本质）。（虚拟DOM是以JS对象的形式存在。）
- 目的：实现页面元素的高效更新。

### 5.2Diff算法（different）

（新旧虚拟对象对比高效，才能真正实现高效）

- **tree diff：**新旧两棵虚拟DOM树，逐层对比的过程，就是Tree Diff，(每一层节点进行对比)；当整棵DOM逐层对比完毕，则所有需要被按需更新的元素，必然能够找到。

- **component diff：**在进行Tree Diff时，每一层中，组件级别的对比，叫做Component Diff；

  - 如果对比前后，组件的类型相同，则暂时认为组件不需要被更新；
  - 如果对比前后，组件类型不同，则需要移除旧组件，创建新组件，并追加到页面上。

- **element diff：**在进行组件对比时，如果两个组件类型相同，则需要进行元素级别的对比，这叫做Element Diff。

  从上到下，递进对比。

  ![diff](E:\MarkDown\FrontEnd\React\images\diff.png)

Diff算法，提供新旧DOM树对比的最优方案。

## 6.创建基本的webpack4.x项目`npm run dev`

### 创建步骤等

- `E:\01.webpack-base`vscode打开

- 运行`npm init -y` 快速初始化

- 在项目根目录创建`src`**源代码**目录和`dist`产品目录（发布的项目/产品放在这里）

- 在src目录下创建`index.html`,`main.js`webpack打包的入口文件

- 使用npm安装webpack，运行`npm i webpack webpack-cli -D`,(`cnpm i webpack -D`)

  - 全局运行`npm i cnpm -g`（安装cnpm）
  - cli脚手架

  Error: Cannot find module 'webpack-cli'，发现 webpack-cli需要全局安装 即可解决上述问题

  `npm i webpack-cli -g`

- 注意：webpack 4.x 提供了约定大于配置的概念：目的是为了尽量减少配置文件的体积。
  - 默认约定：打包入口时`src`->`index.js`；打包的输出文件是`dist`->`main.js`
  - `4.x`中新增了`mode`选项（必选项），可选值为development ，production

`webpack.config.js`【↓】

```js
// 向外暴露一个打包的配置对象，因为webpack是基于Node构建的，所以webpack支持所有Node API和语法
module.exports = {
    mode : 'development' // 模式 development 开发环境 production 生产环境（压缩）,4.x 新增
    // 在webpack 4.x中，有一个很大的特性，就是约定大于配置，默认的打包入口路径是src->index.js不用写entry，不再用main当入口
}
// export default {} //node中不支持
// 这是ES6中向外导出的API，与之对应的是import ** from '标识符'
```

![1569930984976](E:\MarkDown\FrontEnd\React\images\1569930984976.png)

`index.html`,手动引入`<script src="../dist/main.js"></script>`

### Node 与 Chrome

Node.js是基于Chrome V8 引擎的JavaScript运行环境。

哪些ES6特性 Node 支持呢？

- 如果Chrome浏览器支持哪些，则Node就支持哪些。`export default {} //node中不支持`

### Babel

Babel支持所有ES6高级特性，是个转换器。

### webpack-dev-server

#### 实时打包刷新

`package.json`

```json
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack-dev-server" //这里
  },
```

安装`npm i webpack-dev-server -g`（有版本兼容问题，但是还可以用）

![1569936588563](E:\MarkDown\FrontEnd\React\images\1569936588563.png)

> 实时打包，等待中。`Ctrl+C`终止`终止批处理操作吗(Y/N)? `，输入Y
>

```c
｢wds｣: Project is running at http://localhost:8080/   # 运行在本地8080端口
｢wds｣: webpack output is served from /  # webpack 的输出文件main.js(实时的)运行在根目录（/）
```

![1569935971287](E:\MarkDown\FrontEnd\React\images\1569935971287.png)

没有`main.js`物理文件，但是可以访问，因为`webpack-dev-server`实时打包生成的`main.js`文件在内存（放在内存快）中，所以在项目根目录中看不见。

![1569936008370](E:\MarkDown\FrontEnd\React\images\1569936008370.png)

修改`index.html`中`<script>`。 `<script src="/main.js"></script>`

#### 打包完成后自动打开浏览器

`package.json`

```json
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack-dev-server --open firefox --port 3000 --hot --host 127.0.0.1"
  }
```

`--open`自动打开浏览器（默认打开谷歌浏览器），`--progress`进度，`--compress`压缩，`--host`主机

`--open firefox`,`--open iexplore`

### html-webpack-plugin（安装失败）

`webpack-dev-server`实时打包生成的`main.js`文件在内存（放在内存快）中。

将首页也放入内存。

`npm i html-webpack-plugin -g`

配置文件：`webpack.config.js`：

```js
const path = require('path');
// 导入 在内存中自动生成 index 的插件
const HtmlWebPackPlugin = require('html-webpack-plugin');
// 创建一个插件的实例对象
const htmlPlugin = new HtmlWebPackPlugin({
    template: path.join(__dirname, './src/index.html'), // 源文件,__dirname(双下划线)代表当前文件所属的目录，这里就是根目录
    filename: 'index.html' // 生成的内存中首页的名称
});
//向外暴露一个打包的配置对象，因为webpack是基于Node构建的，所以webpack支持所有Node API和语法
module.exports = {
    mode : 'development', // 模式 development 开发环境 production 生产环境（压缩）
    // 在webpack 4.x中，有一个很大的特性，就是约定大于配置，默认的打包入口路径是src->index.js不用写entry，不再用main当入口
    plugins: [
        htmlPlugin // 在外面new对象
    ]
}
```

> **安装插件问题**
>
> ` compilation.mainTemplate.applyPluginsWaterfall is not a function`
>
> `[webpack4 TypeError: compilation.mainTemplate.applyPluginsWaterfall is not a function ](https://www.skiy.net/201803014976.html)`这个是因为官方 html-webpack-plugin 没有及时更新支持 webpack4 导致的问题：
>
> 解决方案：`npm install webpack-contrib/html-webpack-plugin --save-dev`（连接失败）

> vscode:Ctrl+B关闭侧边栏
>

先运行`cnpm install`安装所有依赖项。

## 7.在项目中使用React(不使用脚手架)

> 拆分终端

   ![1569985147545](E:\MarkDown\FrontEnd\React\images\1569985147545.png)

1. 运行`cnpm i react react-dom -S`安装
   - react：专门用于创建组件（React是组件化的）和虚拟DOM（用JS对象表示的DOM），同时包括组件的生命周期。
   - react-dom：专门进行DOM操作的，主要应用场景，就是`ReactDOM.render()`

2. 在`index.html`页面中，创建容器：

   ```html
   <!-- 创建一个容器，将来渲染的虚拟DOM会放到这个容器内 -->
   <div id="app"></div>
   ```

   > `Target container is not a DOM element.`出现此错误的原因是我将webpack生成的js文件放在了head，此时DOM还没有建立完毕，因此出现 not a DOM element 的错误，所以将js文件放在HTML底部就可以了。**(html-webpack-plugin实时创建首页，会自动在底部引入main.js，插件安装失败，自己的没有。)**

3. 导入包：

   ```js
   //1. 这两个导入的时候，接收的成员名称必须这么写
   import React from 'react'; // 创建组件，虚拟DOM元素，生命周期
   import ReactDOM from 'react-dom'; // 把创建好的组件和虚拟DOM放到页面上展示
   ```

4. 创建虚拟DOM元素：

   ```jsx
   //2. 创建虚拟DOM元素
   /**
    * param1: 创建的元素的类型，字符串，表示元素名称
    * param2：是一个对象或null，表示当前这个DOM元素的属性
    * param3：子节点（文本子节点或其他虚拟DOM）
    * paramn：其他子节点
    * <h1 id="myh1" title="this is a h1">这是一个大大的H1</h1>
   */
   // const myh1 = React.createElement('h1', null, '这是一个大大的H1');// 调用方法有返回值。这是一个 JS 对象。
   const myh1 = React.createElement('h1', {id:'myh1',title:'this is a h1'}, '这是一个大大的H1');// 调用方法有返回值。
   ```

5. 渲染：

   ```js
   // 3. 使用ReactDOM 把虚拟DOM渲染到页面上
   /**
    * param1：要渲染的那个虚拟DOM元素
    * param2：指定页面上一个容器,是一个DOM元素，而不是选择器
   */
   ReactDOM.render(myh1, document.getElementById('app')); 
   ```

## 8.JSX语法

### 如何启用JSX语法？（安装babel插件）

- 安装`babel`插件

  - 运行`npm i babel-core babel-loader@7 babel-plugin-transform-runtime -D`

  - 运行`npm i babel-preset-env babel-preset-stage-0 -D`（`preset`->语法)

    > ```c
    > × Install fail! Error: EPERM: operation not permitted, symlink 'E:\01.webpack-base\node_mod
    > ules\_babel-plugin-syntax-trailing-function-commas@6.22.0@babel-plugin-syntax-trailing-function-commas' -> 'E:\01.webpack-base\node_modules\_babel-preset-env@1.7.0@babel-preset-env\node_modules\babel-plugin-syntax-trailing-function-commas'
    > ```
    >
    > 我的是因为，node_modules目录下有同名文件夹，所以报错。
    >
    > 删除之后安装成功。
  
  > ```c
  > Cannot find module '@babel/core'
  >  babel-loader@8 requires Babel 7.x (the package '@babel/core'). If you'd like to use Babel 6.x ('babel-core'), you should install 'babel-loader@7'.
  > ```
  >
  > - 最简单的办法是：用loader7代替8。`npm i babel-loader@7`（成功）
  >
  > - `babel-loader`最新版从类似于`babel-core`这样的改成`@babel/core`。
  >
  >   `@babel/core、@babel/present-env、@babel/runtime、@babel/plugin-transform-runtime、@babel/plugin-proposal-class-properties、@babel/preset-react`。然后在`.babelrc`文件中添加：(未尝试)
  >
  >   ```json
  >   {
  >       "presets": ["@babel/present-env", "@babel/preset-react`"],
  >       "plugins": ["@babel/transform-runtime", "@babel/plugin-proposal-class-properties"]
  >   }
  >   ```
  
2.  安装能够识别转换 JSX 语法的包`babel-preset-react`(这个很重要)

     - 运行`npm i babel-preset-react -D`
    
- 装了第三方`loader`，需要在`webpack.config.js`配置文件中配置`module`规则，添加`babel-loader`配置项（与`mode`，`plugins`平级，别忘记加`exclude`。）

  > webpack 默认只能打包处理 .js 后缀名类型的文件；像`.png`，`.vue` ，`.css`无法主动处理，所有要配置第三方的`loader`；

  ```js
  module: { // 所有第三方模块的匹配规则，webpack默认只能打包出js文件，
          rules: [
              { test: /\.js|jsx$/, use: 'babel-loader', exclude:/node_modules/} 
              //正则,use:多个用数组，exclude:排除项（千万别忘记添加）
          ] //数组，以 s 结尾的，都是对象
  }
  ```

- 添加babel的配置文件`.babelrc`，一般放到项目的根目录。`.babelirc`文件（符合`json`格式，双引号，不能写注释）安装的`presets`语法文件，`plugin`插件文件。

  ```json
  {
      "presets": ["env", "stage-0", "react"],
      "plugins": ["transform-runtime"]
  }
  ```

> 控制终端`cls`->清空

### JSX语法的本质：

这种在JS中混合写入类似于 HTML 的语法，叫做JSX语法，符合**XML规范**（比HTML严格，单边标签要自闭合）的JS。

并不是直接把 JSX 直接渲染到页面上，而是内部先转换成了 createElement 的形式，再渲染的。

```JSx
// 渲染 页面上的DOM结构，最好的方式，就是写HTML代码
// 注意：在js文件中，默认不能写这种类似与HTML的标记，否则打包失败
// 可以使用babel来转换这些JS标签，转换成React.createElement的形式
// 大家注意：这种在JS中混合写入类似于 HTML 的语法，叫做JSX语法，符合XML规范的JS。
// JSX 语法的本质，还是在运行的时候，被转换成 React.createElement 的形式来执行的
const mytext = <div id="mydiv" title="div aaa">这是一个div元素</div>;
let a = 10;
ReactDOM.render(<div>{a + 2}</div>, document.getElementById('app')); 
```

### 在JSX 中混合写入 JS 表达式

在 JSX 语法中，要把 JS 代码写到`{ }`中 :（相当于`EL`表达式/插值表达式）

**渲染什么情况下需要使用 `{ }`：**

​	当我们需要在 JSX 控制的区域内，写 JS 表达式了，则需要把 JS 代码写到 `{ }`中。

- 渲染数组变量、字符串变量

- 渲染布尔值

- 为属性绑定值

  ```jsx
  let title = "dd";
  let boo = true;
  ReactDOM.render(<div title={title}>{a + 2}<hr/>{boo.toString()}</div>, document.getElementById('app')); 
  ```

- 渲染 JSX 元素，不要想成一个标签，是一个JS对象，也需要用 {} 插入 JSX 中，不需要双引号`""`，加上双引号就变成字符串了。

- 渲染 JSX 元素数组，（**key**属性问题）

  ```jsx
  const h1 = <h1>哈哈哈</h1>;//不要想成一个标签，是一个JS对象，也需要用 {} 插入JSX中
  const arr = [
      <h2>this is h2</h2>,
      <h3>this is h3</h3>
  ]
  ReactDOM.render(<div title={title}>
      {h1}
      {arr}
  </div>, document.getElementById('app')); 
  ```

- 将普通字符串数组，转换为 JSX 数组并渲染到页面上。

  > 数组的forEach没有返回值，map方法返回新数组。

  ```jsx
  const arrStr = ["a", "b", "c"];
  ReactDOM.render(<div title={title}>
      {arrStr.map((item)=>{
          return <h3>{item}</h3>
      })};
  // 或者
  ReactDOM.render(<div title={title}>
      {arrStr.map((item)=> <h3>{item}</h3>)};
  ```

  > 箭头函数右边如果只有一行，可以不用花括号`{}`，也不需要写`return`。
  >
  > JavaScript 语句后面应该加分号吗？下一行首字符是`[、（、+、-、/ `五个符号之一的，前一行必须加分号`;`。`[1, 2, 3].forEach(...);`

```c
 Warning: Each child in a list should have a unique "key" prop.
```

- react 中 key 的作用：

  如果需要保存数据的状态，需要提供唯一标识。（key 的主要效果是diff算法可以有效辨识变化的虚拟DOM）。key 要加给直接被 `map`，`forEach`直接控制的元素（最外层的元素）。

  ```jsx
  {arrStr.map((item, index)=>{
      return <div key={item+index} ><h3>{item}</h3></div>// React中需要把 key 添加给被forEach 或 map 或 for 循环直接控制的元素(最外层div)。
      })}
  ```

### JSX中书写注释

```jsx
{/* 单行注释 */}
{
    // 多行注释，不推荐
}
```

### JSX注意点

- **特殊属性名：**为 JSX 中的元素添加 `class`类名，需要使用`className`来替代`class`；`htmlFor`替换 `label`的`for`属性。（因为`class`和`for`是 JS 里面的关键字）。

  ```jsx
  <p className="myelem">1111111</p>
  <label htmlFor="ooo">111</label>
  ```

- 在 JSX 创建DOM的时候，所有的节点，必须有**唯一的根元素**进行包裹。

- 在 JSX 语法中，标签必须成对出现，如果是单标签，则必须**自闭合**。

## 相关文章

- [2018 年，React 将独占前端框架鳌头？](https://mp.weixin.qq.com/s/gV-w_rRfdBVAqsOpBGZaVw)
- [前端框架三巨头年度走势对比：Vue 增长率最高](https://mp.weixin.qq.com/s/0wXWqKIigaKzMSfy4vJMVw)

- [React数据流和组件间的沟通总结](http://www.cnblogs.com/tim100/p/6050514.html)
- [单向数据流和双向绑定各有什么优缺点？](https://segmentfault.com/q/1010000005876655/a-1020000005876751)
- [怎么更好的理解虚拟DOM?](https://www.zhihu.com/question/29504639?sort=created)
- [React中文文档 - 版本较低](http://www.css88.com/react/index.html)
- [React 源码剖析系列 － 不可思议的 react diff](http://blog.csdn.net/yczz/article/details/49886061)
- [深入浅出React（四）：虚拟DOM Diff算法解析](http://www.infoq.com/cn/articles/react-dom-diff?from=timeline&isappinstalled=0)
- [一看就懂的ReactJs入门教程（精华版）](http://www.cocoachina.com/webapp/20150721/12692.html)
- [CSS Modules 用法教程](http://www.ruanyifeng.com/blog/2016/06/css_modules.html)
- [将MarkDown转换为HTML页面](http://blog.csdn.net/itzhongzi/article/details/66045880)
- [win7命令行 端口占用 查询进程号 杀进程](https://jingyan.baidu.com/article/0320e2c1c9cf0e1b87507b26.html)

