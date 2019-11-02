# React.js-Part4

## 主要内容

1. React 中的路由 react-router-dom
2. UI 框架 Ant Design UI的设计语言

## 1.介绍

npm官方的包：https://www.npmjs.com/package/react-router-dom

github上的仓库：https://github.com/ReactTraining/react-router

<img src="E:\MarkDown\FrontEnd\React\images\1570541108491.png" alt="1570541108491" style="zoom:33%;" />

<img src="E:\MarkDown\FrontEnd\React\images\1570541198537.png" alt="1570541198537" style="zoom: 33%;" />

官方文档：https://reacttraining.com/react-router/web/guides/quick-start

Examples 例子；Guides 指南；API

Guides > Quick Start

## 2.react-router-dom基本使用

- 安装包：npm install react-router-dom

- 主入口文件 index.js 导入App 根组件

  ```jsx
  import App from '@/App.jsx'
  ReactDOM.render(<App />, document.getElementById('app'));
  ```

- 导入路由模块 

  - HashRouter 表示一个路由的跟容器，将来所有与路由相关的东西，都要包裹在 HashRouter 里，且一个网站只需使用 HashRouter 一次（出现一次）。
  - Route 表示一个路由规则，其上有两个重要的属性：path component
  - Link 表示一个路由的链接， 就像 vue 中的 <router-link to=''></router-link>

  ```jsx
  import {HashRouter, Route, Link} from 'react-router-dom'
  // var router = new VueRouter({
  //     routes:[
  //         {path: '', component: ''}
  //     ]
  // })
  ```
  
- `#` 表示已经启用路由

<img src="E:\MarkDown\FrontEnd\React\images\1570542713545.png" alt="1570542713545" style="zoom:50%;" />

> VS code 快速创建：【vscode中一键生成react代码块以及快速补全react代码】https://blog.csdn.net/qq_38111015/article/details/89638967

- 当使用HashRouter 把APP根组件的元素包裹起来之后，网站就已经启用路由了

  在一个HashRouter 中，只能有唯一的根元素(自己没有报错)

  在一个网站中，只要使用 唯一一次 `<HashRouter></HashRouter>` 就行了

- Link to=“  ” 对应 Route path=“  ”

- Route 创建的标签就是路由规则，其中 path 表示要匹配的路由，components 表示要展示的组件

  在view 中有个 router-view 的路由标签，专门放置匹配到的路由组件，但在react-router中，并没有类似这样的标签，而是直接把Route 标签当作 {占位符}

  Route 有两个身份，一个是**路由匹配规则**，一个是它是**占位符**，表示将来匹配到的组件都放到这个位置 

```jsx
render(){
        return (<HashRouter>
        <div>
            <h1>这是网站的APP组件</h1>
            <hr/>
            <Link to="/home">首页 </Link> 
            <Link to="/movie">电影 </Link> 
            <Link to="/about">关于</Link>
            <hr/>
            <Route path="/home" component={Home}></Route>
            <hr/>
            <Route path="/movie" component={Movie}></Route>
            <hr/>
            <Route path="/about" component={About}></Route>
        </div>
	</HashRouter>);
```

## 3.路由传参（匹配路由参数）

点击电影：电影类型/电影ID ：/movie/TOP250/10

```jsx
{/* 注意：默认情况下，路由中的规则是模糊匹配，如果路由可以部分匹配成功，就会展示这个路由对应的组件。 如果想让路由规则进行精确匹配，可以为Route，添加 exact 属性，表示启用精确匹配模式*/}
{/* 如果要匹配参数，可在匹配规则中，使用 : 冒号修饰符，表示这个位置匹配到的是参数 */}
	<Route path="/movie/:type/:id" component={Movie} exact></Route>
```

如果想从路由规则中，提取匹配的参数进行使用，可以使用 this.props.match.params.xxx

```jsx
export default class Movie extends Component {
    constructor(props){ // 进入组件，就会调用 construct 构造函数，可以把匹配参数保存到 state 中。
        super(props);
        this.state = {
            routeParams: props.match.params
        }
    }
    render() {
        console.log(this); // 组件
        return (
            // 输出参数type 和 id
            // 如果想从路由规则中，提取匹配的参数进行使用，可以使用 this.props.match.params.xxx
        <div> Movie  --- {this.state.routeParams.type} ---- {this.state.routeParams.id}</div>
        );
    }
}
```

<img src="E:\MarkDown\FrontEnd\React\images\1571135214692.png" alt="1571135214692" style="zoom:50%;" />

## 4.Ant Design

- 支持环境

  - 现代浏览器和 IE9 及以上（需要 [polyfills](https://ant.design/docs/react/getting-started-cn#兼容性)）。

  - 支持服务端渲染。
  - [Electron](https://electronjs.org/)

- Electron：使用JavaScript，HTML和CSS构建跨平台的桌面应用。VS Code，GitHub，Atom 内部是用网页进行开发的。

- 安装：npm i antd

- 导入

  ```jsx
  import { DatePicker } from 'antd';
  import 'antd/dist/antd.css'; // 样式
  ```

- 开发文档：https://ant.design/docs/react/introduce-cn

- 一般，使用第三方UI组件，它们的样式表文件是以 .css 结尾，所以我们不为 css 文件启用模块化。推荐不手写 css 文件，而是自己手写 scss 文件，这样只需为 scss 或 less 文件启用模块化。

- **按需导入：**

  由于 直接使用 Ant Design 的全部包，体积过大，所以建议按需导入，这样能减少 bundle.js（main.js） 文件的体积。

<img src="E:\MarkDown\FrontEnd\React\images\1571141435699.png" alt="1571141435699" style="zoom:50%;" />

- 使用 [babel-plugin-import](https://github.com/ant-design/babel-plugin-import)（推荐）。

  npm i babel-plugin-import -D

  ```js
  // .babelrc or babel-loader option
  {
    "plugins": [
      ["import", {
        "libraryName": "antd",
        "libraryDirectory": "es",
        "style": "css" // `style: true` 会加载 less 文件
      }]
    ]
  }
  ```

  然后只需从 antd 引入模块即可，无需单独引入样式。等同于下面手动引入的方式。

  ```jsx
  // babel-plugin-import 会帮助你加载 JS 和 CSS
  import { DatePicker } from 'antd';
  ```

- 手动引入

  ```jsx
  import DatePicker from 'antd/es/date-picker'; // 加载 JS
  import 'antd/es/date-picker/style/css'; // 加载 CSS
  // import 'antd/es/date-picker/style';         // 加载 LESS
  ```

<img src="E:\MarkDown\FrontEnd\React\images\1571134534256.png" alt="1571134534256" style="zoom: 50%;" />