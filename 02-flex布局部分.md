# flex布局笔记整理

## 了解-webkit-box

利用postcss进行css代码的向后兼容时，display:flex兼容后的代码常会带有display：-webkit-box。
部分移动端内核较低，只支持老版本的box布局，不支持flex布局。
box布局是flex布局的前身，与float不同，float超出边界时会自动换行，box和flex不会。

### box与flex区别

#### 容器部分

1. 子元素总宽度超过父元素时：
	box => 子元素溢出父元素边界
	flex => 子元素被挤压
	[查看对比](https://codepen.io/goblin-pitcher/pen/YzKJXGX)
2. 修改排列时的主轴：
	box => -webkit-box-orient:  (horizontal | vertical)
	flex => flex-direction: (row | column)
	(效果相同，差异仅是属性名不同)
3. 排列时的顺序:
	box => -webkit-box-direction: (normal | reverse)
	flex => flex-direction: (row-reverse | column-reverse)
	注： flex的反转效果，以row-reverse为例，是从容器最右边开始排列的，box反转效果是从容器最左边排列的([查看对比](https://codepen.io/goblin-pitcher/pen/PoYyqjW))
4. 主轴富余空间管理：
	box => -webkit-box-pack: 
	/* start: 富余空间在右边 *
	/* end: 富余空间在左边 *
	/* center: 富余空间在两边 *
	/* justify； 富余空间在项目之间 *
	flex => justify-content:
	/* flex-start 富余空间在左边 *
	/* flex-end 富余空间在右边 *
	/* center 富余空间在两边 *
	/* space-between, space-around, space-evenly 不同形态的富余空间在项目之间 *
5. 侧轴富余空间管理：
	box => -weblit-box-align: 
	/* start: 富余空间在下面 *
	/* center： 富余空间在两边 *
	/* end：富余空间在上面 *
	flex => align-items:
	/* flex-start: 富余空间在下面 *
	/* center: 富余空间在两边 *
	/* flex-end: 富余空间在上面 *
	/* baseline: 基线对其 *
	/* stretch: 等高布局 *

**	注：4、5中是相对于主轴和侧轴，并非固定是X轴还是y轴 **

#### 项目(item)部分

1. 拉伸与压缩
	box => -webkit-box-flex （既能控制拉伸也能控制压缩）
	flex => flex-grow (只能控制拉伸)、flex-shrink (只能控制压缩)
2. 宽度设置
	box => width
	flex => width或flex-basis,flex-basis优先级高于width

## flex布局基础

flex大部分属性规则都不复杂，参考下图使用即可。需要注意的属性是flex-grow和flex-shrink。
![图片描述](https://image-static.segmentfault.com/182/968/1829681889-5d81d10c0bfcf_articlex)

### flex-grow的计算方式

flex-grow的作用： 当主轴方向存在富余空间时，将富余空间赋予对应item
flex-grow默认值： 0
flex-grow触发条件： 主轴方向存在富余空间
flex-grow对于富余空间计算： 富余空间根据flex-grow数值进行分配
例：
假设content宽度为600px，flex布局，其内部有4个item，item初始宽度分别为50px、100px、150px、200px。
第1个item的flex-grow:4;
第4个item的flex-grow:1;
Q：求4个item的宽度分别为多少？
解： 
1. 计算富余空间宽度：
     `富余 = 600px - (50+100+150+200)px = 100px`
2. 计算单位flex-grow的分配宽度：
	 `单位flex-grow宽度 = 富余/(flex-grow总和) = 100px/(4+1) =20px`
3. 按照flex-grow数值分配宽度：
	 `第1个item宽度 = 初始宽度 + 单位flex-grow宽度*flex值 = 50px + 20px*4 = 130px`
	 `第4个item宽度 = 初始宽度 + 单位flex-grow宽度*flex值 = 200px + 20px*1 = 220px`
因此，4个item宽度分别为130px、100px、150px、220px。可[查看实例](https://codepen.io/goblin-pitcher/pen/VwZELNm)

### flex-shrink的计算方式

flex-shrink的作用： 当容器flex-wrap为nowrap，且item初始宽度之和大于容器宽度时，容器会对其中的item进行压缩，flex-shrink决定压缩时的规则。
flex-shrink默认值： 1
flex-shrink触发条件：容器宽度不够，需要压缩内容显示
flex-shrink计算方式：
1. 计算压缩空间
2. 每个item权重 = flex-shrink * 宽度， 根据权重分配压缩空间
3. 是否有item会被压缩至0宽度，若有则假设这些item宽度为0，重新进行压缩空间分配计算
** 注：当item中有内容时，item被压缩的最小宽度为内容宽度**

例：假设content宽度为300px，flex布局，其内部有4个item，item初始宽度分别为50px、100px、150px、200px。
Q1：4个item实际显示的宽度为多少？
item1的flex-shrink设为10;
item4的flex-shrink设为5;
Q2: 4个item的宽度分别为多少？
解：
Q1: 
1. 计算压缩空间宽度
	`压缩 = (50+100+150+200)px - 300px = 200px`
2. 分配压缩空间
	`4个item宽度比为1:2:3:4，初始flex-shrink为1`
	`单位权重宽度 = 200px/(1*1+2*1+3*1+4*1) = 20px`
	`第1个item宽度 = 初始宽度 - 单位权重宽度*权重 = 50px - 20px*1*1 = 30px`
	`第2个item宽度 = 初始宽度 - 单位权重宽度*权重 = 100px - 20px*2*1 = 60px`
	`第3个item宽度 = 初始宽度 - 单位权重宽度*权重 = 150px - 20px*3*1 = 90px`
	`第4个item宽度 = 初始宽度 - 单位权重宽度*权重 = 200px - 20px*4*1 = 120px`
3. 是否需要重新计算
	`由于步骤2计算的item宽度都大于等于0，因此不需要重新计算`
因此，4个item宽度分别为 30px、60px、90px、120px。

Q2：

1. 计算压缩空间宽度
	`压缩 = (50+100+150+200)px - 300px = 200px`
2. 分配压缩空间
	`4个item宽度比为1:2:3:4，item1、item4的flex-shrink分别为10、5，其余flex-shrink为1`
	`单位权重宽度 = 200px/(1*10+2*1+3*1+4*5) = 40/7px`
	`第1个item宽度 = 初始宽度 - 单位权重宽度*权重 = 50px - 40/7px*1*10 = -7.14px`
3. 是否需要重新计算
	`由于第1个item计算后的宽度为-7.14px<0，因此需要重新分配压缩空间`
4. 计算压缩空间宽度
 	`由于上一步中item1宽度小于0，将item1宽度当作0，重新计算压缩空间宽度`
	`压缩 = (100+150+200)px - 300px = 150px`
5. 分配压缩空间
	`单位权重宽度 = 150px/(2*1+3*1+4*5) = 6px`
	`第1个item宽度 = 0`
	`第2个item宽度 = 初始宽度 - 单位权重宽度*权重 = 100px - 6px*2*1 = 88px`
	`第3个item宽度 = 初始宽度 - 单位权重宽度*权重 = 150px - 6px*3*1 = 132px`
	`第4个item宽度 = 初始宽度 - 单位权重宽度*权重 = 200px - 6px*4*5 = 80px`
6. 是否需要重新计算
	`由于步骤5计算的宽度都大于等于0，因此不需要重新计算`
因此，4个item宽度分别为 0、88px、132px、80px。可[查看实例](https://codepen.io/goblin-pitcher/pen/RwberKv)
注意：给item添加文字后，宽度无法压缩为0，压缩空间计算将更为复杂。