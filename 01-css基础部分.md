## css基础部分

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
	- 可设置vertical-align(参考【居中】文件夹的【[vertical-align居中.html](https://github.com/goblin-pitcher/css-study-notes/blob/master/%E5%B1%85%E4%B8%AD/vertical-align%E5%B1%85%E4%B8%AD.html)】)
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
`#id .class p{}`优先级权重为：100+10+1=1 1 1；**注意，权重的计算不可进位**
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

#### 浏览器对css选择器的解析

+ CSS 选择器的解析是从右向左解析的。
+ 若从左向右的匹配，发现不符合规则，需要进行回溯，会损失很多性能。
+ 若从右向左匹配，先找到所有的最右节点，对于每一个节点，向上寻找其父节点直到找到根元素或满足条件的匹配规则，则结束这个分支的遍历。
+ 两种匹配规则的性能差别很大，是因为从右向左的匹配在第一步就筛选掉了大量的不符合条件的最右节点（叶子节点），而从左向右的匹配规则的性能都浪费在了失败的查找上面。
+ 而在 CSS 解析完毕后，需要将解析的结果与 DOM Tree 的内容一起进行分析建立一棵 Render Tree，最终用来进行绘图。
+ 在建立 Render Tree 时（WebKit 中的「Attachment」过程），浏览器就要为每个 DOM Tree 中的元素根据 CSS 的解析结果（Style Rules）来确定生成怎样的 Render Tree。


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
可参考【盒子模型】文件夹【[内联元素的padding.html](https://github.com/goblin-pitcher/css-study-notes/blob/master/%E7%9B%92%E5%AD%90%E6%A8%A1%E5%9E%8B/%E5%86%85%E8%81%94%E5%85%83%E7%B4%A0%E7%9A%84padding.html)】

#### margin

注意事项：
+ BFC
+ 块级元素可利用margin:auto水平居中(参考【居中】文件夹的【[margin居中.html](https://github.com/goblin-pitcher/css-study-notes/blob/master/%E5%B1%85%E4%B8%AD/%E6%B0%B4%E5%B9%B3%E5%B1%85%E4%B8%ADmargin.html)】),垂直居中可结合position完成(参考【居中】文件夹的【[居中position+margin.html](https://github.com/goblin-pitcher/css-study-notes/blob/master/%E5%B1%85%E4%B8%AD/%E5%B1%85%E4%B8%ADposition%2Bmargin.html)】)

#### 默认样式

一般而言，即使不设置样式，不同浏览器中的盒模型都有各自的默认样式，具体可参考【盒子模型】文件夹【[默认样式.html](https://github.com/goblin-pitcher/css-study-notes/blob/master/%E7%9B%92%E5%AD%90%E6%A8%A1%E5%9E%8B/%E9%BB%98%E8%AE%A4%E6%A0%B7%E5%BC%8F.html)】

**注意，利用伪元素和vertical-align实现垂直居中时，即使伪元素宽度设为0，伪元素与垂直居中元素之间依旧有间距，这也是默认样式的影响，给容器设置font-size:0;可消除影响。**

### 浮动

#### 浮动相关特性：

+ 浮动元素会脱离文档流，浮动元素浮于普通元素之上
+ 浮动元素的边界是父元素边框
+ 浮动元素上方有块级元素时，浮动元素不会浮动到上方元素那一行
+ 浮动元素会自动换行（多个float时才会换行，块级元素和浮动一行时不换行）
+ 浮动元素不会盖住文字（文字自动环绕）

**注意，块级元素脱离文档流后，默认宽高都由内容撑开，如修改float、position，但与行级元素不同的是，它仍旧能设置宽高**

demo：
+ 参见【浮动】文件夹下的【[float.html](https://github.com/goblin-pitcher/css-study-notes/blob/master/%E6%B5%AE%E5%8A%A8/float.html)】

#### 高度塌陷

原因：
+ 由于浮动元素脱离文档流，父元素中无其他子元素时，content高度为0，引起高度塌陷。

解决方案：
+ 父元素开启BFC
+ 父元素下添加伪元素，设置清除浮动，避免浮动造成的高度塌陷
	- 注意，伪类是after,且display需设为block,块级元素放在最后，清除浮动的同时可以保证父元素高度能完整包含浮动元素

demo: 
+ 参见【浮动】文件夹下的【[高度塌陷.html](https://github.com/goblin-pitcher/css-study-notes/blob/master/%E6%B5%AE%E5%8A%A8/%E9%AB%98%E5%BA%A6%E5%A1%8C%E9%99%B7.html)】

#### 清除浮动

作用：
+ 消除其他元素对**当前**元素的影响(该属性只作用于元素本身)
+ clear设为left、right、both、none(默认)和inherit；

注意事项：

+ 当有左浮动和右浮动时，css设置`clear: both;`可清除对该元素**影响最大**的浮动

### BFC

BFC:块级元素上下文。它是一个独立的渲染空间，只有块级元素参与，它规定了内部块级元素的布局，且这个区域是一个隔离的独立容器
IE6及以前的版本没有BFC，IE6中hasLayout属性和BFC效果一样,hasLayout开启方式：**zoom:1;**

#### BFC的特性

+ BFC区域不会与float box重叠
+ 计算BFC高度时，浮动元素也参与计算（清除浮动）
+ BFC时一个独立的容器，内部元素不会受到外面影响，反之也如此

#### 触发BFC的条件

+ 根元素(html)
+ 设置元素浮动
	- 副作用：脱离文档流
+ 设置元素绝对定位
	- 副作用：脱离文档流，整体样式受影响较大
+ 设置元素为inline-block
	- 副作用：宽高由内容撑开，不利于布局
+ 设置overflow为非visiable
	- 由于设为scroll会显示滚动条，，副作用最小的方式是设为hidden

#### 外边距(margin)重叠现象

发生条件：

+ 块级元素
+ margin重叠

解决方式：
1. 将不需要重叠的元素变为非块级元素
	- display: inline-block;
2. 阻隔margin重叠
	- 利用BFC、脱离文档流、元素、伪元素等方式阻隔重叠

注意事项：
1. BFC的隔绝效果是将内部与外部隔绝，而margin是外部样式。
2. 当父元素和子元素margin重叠时，利用BFC可将其内部的子元素margin和外部的BFC的margin隔绝
3. 当兄弟元素的margin重叠时，由于margin是外部属性，元素设为BFC并不能阻止与兄弟元素的margin重叠
4. 兄弟元素间的margin重叠，给元素设置float、绝对定位的解决方式，其本质不是因为BFC(原因见3)，而是因为脱离文档流，隔绝了文档流内的元素对其本身的影响，相当于隔绝了margin,而将display设为inline-block的解决方式，其本质是将元素变为非块级元素，而margin的重叠是块级元素的规则

demo:
[margin重叠-1](https://github.com/goblin-pitcher/css-study-notes/blob/master/BFC/margin%E9%87%8D%E5%8F%A0-1.html)
[margin重叠-2](https://github.com/goblin-pitcher/css-study-notes/blob/master/BFC/margin%E9%87%8D%E5%8F%A0-2.html)

总结：

+ 兄弟元素间的margin重叠解决方式：
	- 元素变为非块级元素
		- display:inline-block;
	- 隔绝重叠的margin
		- 脱离文档流(float、绝对定位)
		- 添加父元素，该父元素设置border、生成BFC或利用伪元素隔绝


+ 父子元素之间的margin重叠解决方式：
	- 元素变为非块级元素
		- display:inline-block;
	- 隔绝重叠的margin
		- 父元素生成BFC
		- 父元素设置border或利用伪元素隔绝

**注意，虽然解决方式都是形成非块级元素和隔绝margin，但隔绝的方式和原理都不一样**

**改变margin重叠的规则，如父元素设置`display：flex; flex-direction:column`，同样能解决margin重叠问题，因为flex布局内没有margin重叠的规则**


