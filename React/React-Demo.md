# React Demo - 电影项目（E:\React-DouBan）

## 1、首页样式基本布局

- App.jsx 这是项目的根组件

- main.js 项目的 JS 打包入口文件

- Layout 布局 - 组件概述

  - `Layout`：布局容器，其下可嵌套 `Header` `Sider` `Content` `Footer` 或 `Layout` 本身，可以放在任何父容器中。
  - `Header`：顶部布局，自带默认样式，其下可嵌套任何元素，只能放在 `Layout` 中。
  - `Sider`：侧边栏，自带默认样式及基本功能，其下可嵌套任何元素，只能放在 `Layout` 中。
  - `Content`：内容部分，自带默认样式，其下可嵌套任何元素，只能放在 `Layout` 中。
  - `Footer`：底部布局，自带默认样式，其下可嵌套任何元素，只能放在 `Layout` 中。

  > 注意：采用 flex 布局实现，请注意[浏览器兼容性](https://caniuse.com/#search=flex)问题。

### 图片处理模块的加载（webpack）

npm 安装：

**file-loader****：** 解决引用路径的问题，拿background样式用url引入背景图来说，我们都知道，webpack最终会将各个模块打包成一个文件，因此我们样式中的url路径是相对入口html页面的，而不是相对于原始css文件所在的路径的。这就会导致图片引入失败。这个问题是用file-loader解决的，file-loader可以解析项目中的url引入（不仅限于css），根据我们的配置，将图片拷贝到相应的路径，再根据我们的配置，修改打包后文件引用路径，使之指向正确的文件。

**url-loader：**如果图片较多，会发很多http请求，会降低页面性能。这个问题可以通过url-loader解决。url-loader会将引入的图片编码，生成dataURl。相当于把图片数据翻译成一串字符。再把这串字符打包到文件中，最终只需要引入这个文件就能访问图片了。当然，如果图片较大，编码会消耗性能。因此url-loader提供了一个limit参数，小于limit字节的文件会被转为DataURl，大于limit的还会使用file-loader进行copy

```
npm install --save-dev file-loader url-loader
```

**1. 打包图片**

```
module.exports = {
    {
        test: /\.(png|jpg|jpeg|gif)$/,
        use : [{
            loader : 'url-loader',
            options : {
                limit : 8192,        //把小于8192B的文件打成Base64的格式
                name : '[name].[ext]',
                outputPath : 'images/'  //将文件打包至images路径
            }
        }]
    }
}
```

**2.解决HTML中的图片不会被打包的问题**

npm 安装

```
npm install html-withimg-loader --save
```

在webpack.config.js中配置

```
module.exports = {
    {
        test: /\.(htm|html)$/,
         use:[ 'html-withimg-loader'] 
    }
}
```

## 2、使用路由切换组件页

## 3、刷新页面时选中路由

![1572177214389](E:\MarkDown\FrontEnd\React\images\1572177214389.png)

```jsx
<Menu
     theme="dark"
     mode="horizontal"
     defaultSelectedKeys={window.location.hash.split("/")[1]} {/*刷新时默认选中的页面，对应<Menu.Item key="xxx">*/}
     style={{ lineHeight: '64px' }}
     >
	<Menu.Item key="home"><Link to="/home">首页</Link></Menu.Item>
    <Menu.Item key="movie"><Link to="/movie">电影</Link></Menu.Item>
    <Menu.Item key="about"><Link to="/about">关于</Link></Menu.Item>
</Menu>
```

## 4、完成页面基本布局

- App.jsx 的 container 设置占满剩余高度。height ：‘100%‘

## 5、正在热映、即将上映、TOP250路由切换

- 对应右侧的结构一致，对应同一个组件，只是数据不一样。

## 6、ES6中的fetch API 

- fetch API 是基于 Promise 封装的
- 当使用 fetch API 获取数据的时候，第一个 then 中获取不到数据，是 Response 对象，我们可以调用response.json() 得到一个新的 promise 。

```jsx
componentWillMount() {
        fetch("http://vue.studyit.io/api/getlunbo")
            .then(response => {
                console.log(response);
            })
		return promise;
    }
```

## 7、使用 isLoading 标识

- 豆瓣API : https://douban-api-docs.zce.me/

```jsx
<Spin tip="Loading...">
      <Alert
            message="正在请求电影列表"
            description="精彩内容即将呈现"
            type="info"
      />
</Spin>
```

## 8、使用第三方的fetch-json

Postman Chrome APP

使用 fetch-jsonp 第三方包 来发送 JSONP 请求，它的用法和浏览器内部的 fetch 完全兼容。

```jsx
import fetchJSONP from 'fetch-jsonp';

		const start = this.state.pageSize*(this.state.nowPage-1);
        const url = `https://api.douban.com/v2/movie/in_theater${this.state.movieType}?start=${start}&count=${this.state.pageSize}`;
        fetchJSONP(url)
            .then(response => response.json())
            .then(data => {
                // console.log(data);
                this.setState({
                    isLoading:false,
                    movies: data.subjects,
                    total:data.total
                })
            });
```

- webpack本身是支持json文件读取的，可打包完成后却报这样的错误

  ```c
  Unexpected token, expected ";"
  ```

- 这个报错是babel-loader在报错，也就是说，打包的时候，babel-loader去解析了json文件。babel是js的编译工具，可以把js、ts、react的语法，根据需求编译成浏览器识别的代码。json不属于js，但现在它被当作了js去解析，因此报出了上边的错误。

  ```js
  module: { // 所有第三方模块的匹配规则，webpack默认只能打包出js文件，
          rules: [
              { test: /\.js|jsx$/, use: 'babel-loader', exclude:[/node_modules/, /\.json$/]} ,//正则,use:多个用数组，exclude:排除项（千万别忘记添加），排除json数据
              
          ] //数组，以 s 结尾的，都是对象
      }
  ```

## 9、获取数据渲染基本模块

- 把每个电影设成单独的组件

- ```jsx
  return <div style={{display: 'flex', flexWrap: 'wrap'}}>
                  {this.state.movies.map(item => {
                      return <MovieItem {...item} key={item.id}></MovieItem>
                      {/* ...解构赋值，属性都传递过去 */}
                  })}
              </div>
  ```

- ```jsx
  return (
          <div className="box">
              <img src="./images/movieImg.jpg" alt="" className="movieImg"/>
              <h4>电影名称：{this.props.title}</h4>
              <h4>上映年份：{this.props.year}年</h4>
              <h4>电影类型：{this.props.genres.join(",")}</h4>
              <Rate disabled defaultValue={this.props.rating.average/2} />
          </div>
          );
  ```

## 10、美化电影列表项

```css
.box {
    border: 1px solid #ccc;
    text-align: center;
    width: 170px;
    margin: 2px;
    box-shadow: 0 0 6px #ddd;
    padding: 4px;
    cursor: pointer;
    transition: all 0.3 case; /* 过度动画*/
}
.box:hover { /*悬浮动画，旋转，缩放，透明度*/
    transform: rotate(1deg) scale(1.03);
    opacity: 0.9;
}
.movieImg {
    width: 100px;
    height: 140px;
}
```

## 11、点击不同分类重新加载

```jsx
componentWillReceiveProps(nextProps) {
        // console.log(nextProps.match);
        // 每当地址栏变化的时候，重置地址栏里的参数，重置完成可以重新发起请求。
        this.setState({
            isLoading: true,
            nowPage: parseInt(nextProps.match.params.page) || 1,
            movieType: nextProps.match.params.type // 电影类型，路由匹配规则中提供的参数
        },function(){
            this.loadMovieListByTypeAndPage();
        })
    }
```

## 12、设置电影类型的刷新

```jsx
<Menu
          mode="inline"
          defaultSelectedKeys={window.location.hash.split('/')[2]}
          // defaultOpenKeys={['sub1']}
          style={{ height: '100%', borderRight: 0 }}
        >
          <Menu.Item key="in_theaters"><Link to="/movie/in_theaters/1">正在热映</Link></Menu.Item>
          <Menu.Item key="coming_soon"><Link to="/movie/coming_soon/1">即将上映</Link></Menu.Item>
          <Menu.Item key="top250"><Link to="/movie/top250/1">TOP250</Link></Menu.Item>
        </Menu>
```

## 13、实现分页，编程式导航

```jsx
 {/* 分页 */}
            <Pagination defaultCurrent={this.state.nowPage} total={this.state.total} pageSize={this.state.pageSize} onChange={(page)=>{  this.pageChanged(page)}} />
```

```jsx
pageChanged = (page) => {
        // 由于使用 BOM 对象，实现了跳转，最好使用路由，实现编程式导航        
        // window.location.href = `#/movie/${this.state.movieType}/${page}`;
        // console.log(this.state.nowPage+"d"+page);
        // 使用 react-router-dom 实现编程式导航
        this.props.history.push(`/movie/${this.state.movieType}/${page}`);
    }
```

## 14、电影的详情页-路由中的Switch组件

- 在  `MovieItem` 组件中，`return` 的 div 上加 onClick 事件。直接在 `MovieItem` 组件上加 onClick 事件不行。

  ![1572269316918](E:\MarkDown\FrontEnd\React\images\1572269316918.png)
- 从父组件传递 history 属性。

  ```jsx
  return <div>
                  <div style={{display: 'flex', flexWrap: 'wrap'}}>
                  {this.state.movies.map(item => {
                      return <MovieItem {...item} key={item.id} history={this.props.history}></MovieItem>
                      {/* ...解构赋值，属性都传递过去 */}
                  })}
              </div>
  ```

- 在匹配路由规则时，我们提供了两个参数 

  exact 精确匹配也会从上到下把路由匹配规则匹配一遍

  使用路由中的 Switch 组件，如果前面的路由匹配到了，则放弃后面的路由

  MovieContainer.jsx

  ```jsx
  		<Switch>
              <Route path="/movie/detail/:id" component={MovieDetail}></Route>
              <Route path="/movie/:type/:page" component={MovieList} ></Route>
            </Switch>
  ```


## 15、实现返回按钮的功能

```jsx
	render() {
        return (
            <div>
                <Button type="primary" onClick={()=>{this.goBack()}}>
                    <Icon type="left" />
                    返回列表
                </Button>
            </div>
        );
    }
    goBack = () =>{
        this.props.history.go(-1);
    }
```

## 16、实现详情页面内容

- componentWillReceiveProps(nextProps)  加载数据
- this.state.isLoading 判断是否异步获取完成



## 注：React Native

手机共屏软件：**Total Control**

