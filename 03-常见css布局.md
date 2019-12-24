# 常见css布局

## 固定布局

宽高、margin、padding等都是固定的

## 流式布局

宽高、margin、padding等都是百分比或计算属性，也叫百分比布局

## 定位布局

绝对定位、相对定位（absolute）和固定定位（fixed）

## 响应式布局

css3媒体查询是响应式方案的核心。

### 媒体查询的方式

+ `<meta>`标签相关
	- `<meta name="viewport" content="viewport-fit=cover...">`
		* 用于解决iphoneX/11等机型虚拟home键带来的问题
		* 使用后，css中可调用constant(safe-area-inset-top/right/bottom/left)等属性
		* 预留底部位置：
			````
			body {
 				padding-bottom: constant(safe-area-inset-bottom);
			}
			// 或
			body {
				height: calc(60px(假设值) + constant(safe-area-inset-bottom));
			}
			````
+ `<link rel="styleshee" type="text/css" href="index.css" media="screen">`
	- 注意，media可以设为all、screen或print
	- media="screen":在屏幕上显示时，应用index.css
	- media="print":在打印时，显示样式index.css

+ @media（媒体查询选择器）
	- 媒体类型查询
		````
		// 屏幕模式下
		@media screen{
			/*屏幕显示的相关规则*/
			.content {
				border: 10px solid;
			}
		}
		// 打印时
		@media print{
			.content {
				border: 10px solid;
			}
		}
		````

### 媒体相关可配置属性

查看链接：[@media 查询](https://www.runoob.com/cssref/css3-pr-mediaquery.html)

### 兼容性相关内容

一般使用`@media`时，应该采用:

````
@media only screen and (max-width: 500px) {
    #app {
        padding: 20px;
}
````

而非 

````
@media screen and (max-width: 500px) {
    #app {
        padding: 20px;
}
````

因为一些较老版本浏览器虽然支持`@media`，但是不支持 and/or 等语法，如果用下面那种方式，很有可能自动忽略and后面的内容，对所有情况下都执行#app的样式。
此种情况下使用`@media only`开头可让only及其后面的匹配语句全部被忽略，使得其被浏览器解读为：

````
@media {
    #app {
        padding: 20px;
}
````
从而使#app样式失效。

## 多列布局（分栏布局）