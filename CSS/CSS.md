# CSS

## css垂直居中

![center](images\center.png)

## css3弹性盒子属性

![flexBox](images\flexBox-1572674197542.png)

## css 的 z-index生效条件

1. 设置元素的position值为relative/absolute/fixed
2. 当opacity不为1或0时
3. 当transform不为none时
4. 当父元素设置display: flex | inline-flex时，子元素设置z-index

1.Z-index的值设置为auto时,不建立新的堆叠上下文,当前堆叠上下文中生成的div的堆叠级别与其父项的框相同。

2.当Z-index的值设置为一个整数时,该整数是当前堆叠上下文中生成的div的堆栈级别。该框还建立了其堆栈级别的本地堆叠上下文。这意味着后代的z-index不与此元素之外的元素的z-index进行比较。

ps: 通俗讲就是,当一个div的Z-index为整数时,它的子元素和外界元素进行比较时,采用父元素的Z-index进行比较, 和兄弟元素比较采用自身的Z-index。当一个div的Z-index为auto时,如果它和它的兄弟进行比较,采用它父元素的Z-index。

## CSS的盒子模型

（1）两种， IE 盒子模型、标准 W3C 盒子模型；IE 的content部分包含了 border 和 pading;

（2）盒模型： 内容(content)、填充(padding)、边界(margin)、 边框(border)。

![boxW3C](images\boxW3C.png)

下面：IE盒模型

![boxIE](images\boxIE.png)

​		css3的box-sizing属性给了开发者选择盒模型解析方式的权利。W3C的盒模型方式被称为“content-box”，IE的被称为“border-box”，使用box-sizing: border-box;就是为了在设置有padding值和border值的时候不把宽度撑开。

CSS伪元素

![伪元素](images\伪元素.png)

## 解释下CSS sprites，以及你要如何在页面或网站中使用它。

CSS Sprites【[spraɪts]】其实就是把网页中一些背景图片整合到一张图片文件中，再利用CSS的“background-image”，“background-repeat”，“background-position”的组合进行背景定位，background-position可以用数字能精确的定位出背景图片的位置

## 实现圆形、三角形

1. border-radius

2. HTML5 Canvas arc() 方法

   ```js
   var c=document.getElementById("myCanvas");
   var ctx=c.getContext("2d");
   ctx.beginPath();
   ctx.arc(100,75,50,0,2*Math.PI);
   ctx.stroke();
   ```

CSS绘制三角形—border法https://www.jianshu.com/p/9a463d50e441

[纯CSS画的基本图形（矩形、圆形、三角形、多边形、爱心、八卦等）](https://www.cnblogs.com/ming1025/p/7363074.html)https://www.cnblogs.com/ming1025/p/7363074.html

## 前缀兼容私有属性

- 1、-moz代表firefox浏览器私有属性
- 2、-ms代表ie浏览器私有属性
- 3、-webkit代表safari、chrome私有属性
- 4、-o- 代表 Opera 私有属性

这些是为了兼容老版本的写法，比较新版本的浏览器都支持直接写：border-radius。

## CSS伪类

```css
a:link {color: #FF0000}		/* 未访问的链接 */
a:visited {color: #00FF00}	/* 已访问的链接 */
a:hover {color: #FF00FF}	/* 鼠标移动到链接上 */
a:active {color: #0000FF}	/* 选定的链接 ，按下鼠标未松开*/
```

**提示：**在 CSS 定义中，a:hover 必须被置于 a:link 和 a:visited 之后，才是有效的。

**提示：**在 CSS 定义中，a:active 必须被置于 a:hover 之后，才是有效的。

## CSS可以继承的属性

字体系列属性、文本系列属性（text-indent、text-align、line-height）、元素可见性、列表布局属性、光标属性

1、字体系列属性

　　font-family：字体系列

　　font-weight：字体的粗细

　　font-size：字体的大小

　　font-style：字体的风格

2、文本系列属性

　　text-indent：文本缩进

　　text-align：文本水平对齐

　　line-height：行高

　　word-spacing：单词之间的间距

　　letter-spacing：中文或者字母之间的间距

　　text-transform：控制文本大小写（就是uppercase、lowercase、capitalize这三个）

　　color：文本颜色

3、元素可见性：

　　visibility：控制元素显示隐藏；opacity

4、列表布局属性：

　　list-style：列表风格，包括list-style-type、list-style-image等

5、光标属性：

　　cursor：光标显示为何种形态

## 选择器

相邻兄弟选择器（Adjacent sibling selector）可选择紧接在另一元素后的元素，且二者有相同父元素

## 百分比单位

- 乘以 包含块 的宽度**margin，padding，left，right，**text-indent，width，max-width，min-width
- 乘以 包含块 的高度**top，bottom，**height，max-height，min-height
- 乘以 元素的字体大小：line-height
- 乘以 元素的行高：vertical-align

**关于包含块（含块）的概念**，不能简单地理解成是父元素。

- 如果是静态定位（static）和相对定位（relative），包含块一般就是其父元素。
- 但是对于绝对定位（absolute）的元素，包含块应该是离它最近的position为absolute，relative，或者fixed的祖先元素。

  absolute定位元素的包含块为最近的position为**非static的祖辈元素**，若祖辈元素中没有定位元素，则包含块为初始包含块，在浏览器中为浏览器视口。
- 对固定定位（fixed）的元素，它的包含块是视口（viewport）。fixed定位是absolute定位的一种特殊表现。fixed定位元素的包含块为初始包含块，即**视口**。


两者包含块都为视口时，但有什么不同之处呢？

- fixed定位元素相对于视口偏移，不随着body主体内容的滚动而滚动。

- 而absolute定位元素相对视口偏移时，仅指的是相对于滚动条处于顶端

### rgba() 和 opacity 的透明效果有什么不同？

rgba()和opacity都能实现透明效果，但最大的不同是opacity作用于元素，以及元素内的所有内容的透明度，

而rgba()只作用于元素的颜色或其背景色。（设置rgba透明的元素的子元素不会继承透明效果！）

![optimize](images\optimize.png)

[前端网页性能最佳实践](https://www.cnblogs.com/developersupport/p/webpage-performance-best-practices.html)

web性能优化方法(https://blog.csdn.net/baidu_30668495/article/details/83055911) 

