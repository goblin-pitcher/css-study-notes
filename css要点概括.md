## css要点概括

### 块级元素和行级元素

#### 不同类型元素的特性

* 块级元素(block)：
	-  独占一行
	-  可设置宽、高，可自动换行
	-  默认宽度为100%
	-  一般是容器类的标签，如：div、p、h(\d)等



* 行级(内联)元素(inline)：
	- 可与其他非块级元素并排
	- 宽高不能设置，由内容撑开，不会自动换行
	- 一般与文本相关的标签都是行级标签，如：span、a、b、em等


* 行内块元素(inline-block):
	- 同时具有块级元素和行级元素的特性
	- 可与其他非块级元素并排
	- 默认宽度由内容决定
	- 可设置宽高，不会自动换行
	- 可设置vertical-align(参考【居中】文件夹的【vertical-align居中.html】)
	- 常见的行内快元素有：img、input、td

#### 元素类型的转换

通过修改css的display可改变元素元素类型

#### 元素的嵌套

一般情况下块级元素可以包含行内元素、行内块元素、块级元素，行内元素不能包含块级元素，只能包含行内元素及行内块元素。
有些特殊的块级元素不能包含块级元素,只能包含行内元素：h1~h6、p、dt。
**注：虽然一般浏览器对元素的嵌套做了兼容处理，随意嵌套也不会报错，但为了提高解释器的效率，开发时最好遵循嵌套规则**

### 选择器

#### 常见选择器

常见选择器有：元素选择器、兄弟/子元素选择器、属性选择器、伪类/伪元素选择器。

参考手册[css选择器](https://www.runoob.com/cssref/css-selectors.html)

#### 样式的继承

利用选择器设置样式时，了解样式的继承可有效减少css代码。

+ 不可继承的：
	- display、margin、border、padding、background、height、min-height、max- height、width、min-width、max-width、overflow、position、left、right、top、 bottom、z-index、float、clear、table-layout、vertical-align、page-break-after、 page-bread-before和unicode-bidi
	- 主要是背景相关、边框相关(包括margin、padding)、定位相关


+ 所有元素可继承：
	- visibility和cursor


+ 内联元素可继承：
	- letter-spacing、word-spacing、white-space、line-height、color、font、 font-family、font-size、font-style、font-variant、font-weight、text- decoration、text-transform、direction
	- 主要是字体相关的样式


+ 块状元素可继承：
	- text-indent和text-align


+ 列表元素可继承：
	- list-style、list-style-type、list-style-position、list-style-image


+ 表格元素可继承：
	- border-collapse

#### 优先级

优先级权重：
+ !important优先级：最高
+ 内联样式(style)优先级：1000
+ id选择器优先级：100
+ 类和伪类优先级：10
+ 元素选择器优先级：1
+ 通配符优先级：0
+ 继承的样式没有优先级

样式优先级的比较方式：
1. 样式权重比较：
一般而言，权重值越高，样式的优先级越高。
权重计算方式：
`#id .class p{}`优先级权重为：100+10+1=1 1 1
**注意，权重的计算不可进位**
`#id .class .class ... .class`若有12个class,优先级权重为:
100+10*12 = 1 12 0
`#id1 #id2`权重为： 100+100=2 0 0
权重比较： 1 12 0 < 2 0 0
2. 权重相等时，后加载的优先级高于先加载的 
3. !important样式优先级高于所有权重，继承样式可被任意权重样式覆盖
4. 都有!important时，则比较该样式的权重
5. 同样是继承样式，最近祖先的优先级更高(如继承自父元素的样式优先级高于继承自祖父元素的样式)。

注意事项：
对于伪类而言，由于link、visit、hover、active伪类经常会有重叠，而同等权重时，优先级依赖于加载顺序，因此这些伪类在开发时，顺序从前到后分别为：link、visit、hover、active。

#### 练习

纯css实现邮箱验证：
[https://codepen.io/goblin-pitcher/pen/BayKboL](https://codepen.io/goblin-pitcher/pen/BayKboL)

nth-of-type、nth-child和not选择器的使用：
[https://codepen.io/goblin-pitcher/pen/MWgOKzp](https://codepen.io/goblin-pitcher/pen/MWgOKzp)

### 盒子模型

![盒子模型](https://www.runoob.com/images/box-model.gif)

#### box-sizing

+ content-box:
	- box-sizing的默认值为content-box
	- Element width = width + border + padding
+ border-box:
	- Element width = width

**注：一般而言，width:50%;这类百分比，是相对于父元素的content的宽度/高度,当元素定位为fixed或absolute时，宽、高的百分比是相对于定位元素的宽高。
（可参考 [盒子模型]文件夹[百分比计算.html]）**

#### border

border为border-width、border-style、border-color的缩写。
相关联系参考：
[css实现三角形](https://codepen.io/goblin-pitcher/pen/ZEzKbWL)

#### padding

需要注意的是，内联元素的padding设置不影响布局.
可参考【盒子模型】文件夹【内联元素的padding.html】

#### margin

注意事项：
+ BFC
+ 块级元素可利用margin:auto水平居中(参考【居中】文件夹的【margin居中.html】)

#### 默认样式

一般而言，即使不设置样式，不同浏览器中的盒模型都有各自的默认样式，具体可参考【盒子模型】文件夹【默认样式.html】
**注意，利用伪元素和vertical-align实现垂直居中时，即使伪元素宽度设为0，伪元素与垂直居中元素之间依旧有间距，这也是默认样式的影响，给容器设置font-size:0;可消除影响。**

### 浮动和BFC