# Node.js Day 1

## Node.js 介绍

### Node.js 是什么

nodejs.org  技术类型的网站：org；io

- Node.js® is a JavaScript runtime built on [Chrome's V8 JavaScript engine](https://v8.dev/).
  - Node.js 不是一门语言，不是库，不是框架
  - Node.js 是个 JavaScript 运行的环境
  - That is to say, Node.js 可以解析和执行 JavaScript 代码
  - 以前只有浏览器可以解析执行 JavaScript 代码，也就是说，现在的 JavaScript 可以 完全脱离浏览器来运行，一切归功于：Node.js
  - 构建于 Chrome 的 V8 引擎之上
    - **JavaScript引擎**是一个专门处理[JavaScript](https://zh.wikipedia.org/wiki/JavaScript)脚本的[虚拟机](https://zh.wikipedia.org/wiki/虚拟机)，一般会附带在[网页浏览器](https://zh.wikipedia.org/wiki/网页浏览器)之中。
    - 代码只是具有特定格式的字符串而已，引擎可以认识它，引擎可以帮你去解析和执行
    - Google Chrome 的 V8 引擎是目前公认的解析执行 JavaScript 代码最快的
    - Node.js 的作者把Google Chrome 的 V8 引擎移植了出来，开发了独立的 JavaScript 运行时环境
- 浏览器种的 JavaScript
  - EmcaScript（核心，语法）
    - 基本语法：if  var function Object Array
  - BOM
  - DOM
- Node.js 中的 JavaScript
  - 没有DOM，BOM，只有EcmaScript
  - 在 Node 这个 JavaScript 执行环境中为 JavaScript 提供了一些服务器级别的操作 **API** （做服务端那些事）构建 web 服务器
    - 如：文件读写
    - 网络服务的构建
    - 网络通信
    - http 服务器
- Node.js uses an event-driven, non-blocking I/O model that makes id lightweight and efficient.
  - 事件驱动、非阻塞I/O模型（异步操作）
  - 轻量和高效
- Node.js' package ecosystem（生态系统）, npm, is the largest ecosystem of open source libraries in the world.
  - npm 是基于 Node.js 开发的
  - Node.js 的 npm 是世界上最大的开源库生态系统（常用第三方包放在这个平台上）
  - 绝大多数 JavaScript 相关的包都存放在 npm 上，目的是让开发人员更方便的下载使用

### Node.js 能做什么

- WEB 服务器后台
- 开发命令行工具：（在命令行下运行管理版本的软件）
  - git（c）
  - npm（node）
  - hexo（node）
- 游戏服务器，接口服务器
- 对于前端开发工程师来讲，接触 node 最多的是它的命令行工具
  - 自己写的很少，主要使用别人开发的第三方工具
  - webpack、gulp、npm

### 预备知识

- HTML + CSS , JavaScript
- 简单的命令行操作
  - cd、dir（列出目录）、ls（列出目录）、mkdir、rm
- 具有服务器开发经验更佳

### 一些资源

- 《深入浅出Node.js》（荐）
  - 偏理论，没有实战内容，理解原理底层有帮助
  - 作者：朴灵
- 《Node.js 权威指南》
  - API 讲解，也没有实战和业务
- JavaScript 标准参考教程（alpha）：http://javascript.ruanyifeng.com
- Node 入门（有趣，荐，mini Book）：https://www.nodebeginner.org/index-zh-cn.html
- 官方API文档：https://nodejs.org/dist/latest-v12.x/docs/api/
- 中文文档（版本比较旧，凑活看）：http://nodeclass.com/api/node.html
- CNODE社区：https://cnodejs.org
- CNODE-新手入门：https://cnodejs.org/getstart 《包教不包会》

### 课程能学到啥

- B/S
- **模块化编程**
  - RequireJS
  - SeaJS
  - `@import('文件路径')`
  - 以前认知的 JavaScript 只能通过 `script` 标签来加载，在 Node 中可以像`@import()`一样引用加载 JavaScript 脚本文件
- Node 常用 API
- 异步编程
  - 回调函数
  - Promise
  - async
  - generator
- Express Web开发框架（×）
- EcmaScript 6
  - 在课程总穿插讲解

## 起步安装

### 下载安装

- Long Time Support：LTS（长期支持版）
- 重新安装会覆盖
- 确认安装成功：cmd：`node --version`

### Hello World

- 打开命令行窗口
  - shift + 右键：在此处打开命令行窗口
  - cd 定位
  - git Bush Here

<img src="E:\MarkDown\FrontEnd\Node\images\1571755550573.png" alt="1571755550573" style="zoom: 50%;" />

- node  filename.js (文件名不能为 node.js，不建议使用中文)

注：命令行里，tab 键自动补齐