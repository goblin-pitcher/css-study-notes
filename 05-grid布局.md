## 网格容器上的属性

### grid-template

+ grid-template-columns/grid-template-rows

  - 使用空格分隔多个值来定义网格的列和行

  - ````css
    .container {
        grid-template-columns: <track-size>...|<line-name><track-size>...;
        grid-template-rows: <track-size>...|<line-name><track-size>...;
    }
    ````

    * track-size是指**轨道**宽度

    * track-size设为auto时，auto代表剩下的宽度，没有剩余空间的情况下，auto默认宽度是内容宽度 

    * 同理，`<track-size>`以fr为单位时，若没有剩余空间，默认宽度也是由内容撑开

    * 注意，假设代码如下：

      ````html
      <div class="container">
          <div class="item"><span></span></div>
          <div class="item"><span></span></div>
          	......
          <div class="item"><span></span></div>
          <div class="item"><span></span></div>
      </div>
      ````

      此处说的网格由内容撑开，内容并不是指的`<span></span>`中的内容，`.container`划分了网格，`<div class="item"></div>`便是网格中的**内容**。

      比如设置`<track-size>`为150px，`.item { width:100px; }`，可以看到网格中会有50px的空白区域。

      比如给item的宽高设为100%，会发现item的百分比是以网格为准的。

    * line-name是指可以为网格线命名，如

      ````css
      .container {
          grid-template-columns: [one] 10% [two second] 100px [three][four] auto [end];
      }
      ````

      1. []中的内容即为网格线的命名，`[two second]`表示网格线有两个别名——two和second，

         `[three][four]`也可表示两个别名

      2. 注意line-name和track-size的排布，两条线之间的是轨道

      3. 别名的作用主要体现在**划分网格区域**

+ template简写

  - grid-template: none|<del>subgrid</del>|`<grid-template-rows>/<grid-template-columns>`

### gap

- gap: `<grid-row-gap>  <grid-column-gap>;`
  - grid-gap是老版本，逐渐被gap取代

+ 网格之间的间距，只对网格之间的间距起作用，边沿的间距不起作用

### 居中

+ items的居中：
  + 行方向：justify-items
  + 列方向：align-items
  + 缩写：place-items: `<align-items> <justify-items>;`
+ content的居中：
  	- 行方向：justify-content
  	- 列方向：align-content
   - 缩写：place-content: `<align-content>  <justify-content>;`
     - align-content/justify-content的值和flex布局一样，都有space-around和space-between，不同的是多了一个space-evenly
     - spaced-evenly: 在每个grid item之间设置均等宽度的空白间隙，**包括外边缘**

### grid-auto

> 当items数量多于grid-template预设的网格数时，会生成新的网格容纳多出的items，grid-auto是控制生成多余网格规则的属性。

+ grid-auto-columns/grid-auto-rows

  - 指定自动生成的**隐式网格轨道**大小
    * 隐式网格轨道：在显式网格轨道的定位超出指定网格范围的行或列时被创建
  - grid-auto-columns/grid-auto-rows：`<track-size>...;`

+ grid-auto-flow

  - 控制自动布局算法的工作方式

  - ````css
    .container {
        grid-auto-flow: row|column|row dense|column dense;
    }
    ````

  - row/column：依次填充每行/列，根据需要添加新行/列

  - dense：尝试用网格填充完空白区域

    * 当空间不够时会尝试压缩，空间多余会尝试伸展
    * 类似与flex布局的flex-grow和flex-shrink的结合

### grid缩写

+ ````css
  .container {
      /* []中的内容代表可省略 */
  	grid:none|<grid-template-rows>/<grid-template-columns>|<grid-auto-flow>[<grid-auto-rows>[/<grid-auto-columns>]];
  }
  ````

### css函数

+ repeat()
  - 只能用于grid-template-columns/grid-template-rows
  - 使用方实`grid-template-columns: repeat(type, value)`
    * type:
      1. `<number>`：整数，确定重复的次数
      2. `<auto-fill>`：根据网格容器长度，确定每行/列填充多少个
      3. `<auto-fit>`：根据网格项的长度，准确定每行/列填充多少个
    * value: 
      1. `<length [<name>]>`，length为长度值，单位可以是px,%,fr，`<name>`为网格名，可省略
      2. max-content：网格轨道长度自适应内容最大的单元格
      3. min-content：网格轨道长度自适应内容最小的单元格
      4. auto：空间充足时占满剩余空间，空间不足时作为min-content
+ fit-content()
  - 实现方实：`grid-template-columns: fit-content(value)`
    * value为长度值，可以是px/%
  - 实现效果：网格宽度 = 内容 < value ? 内容：value
+ minmax()
  - 定义长宽范围的**闭区间**
  - 实现方式：`grid-template-columns: minmax(minVal, maxVal)`
    - 特殊情况：minVal大于maxVal时，执行`(minVal > maxVal) && (maxVal = minVal)`
    - 当maxVal单位是fr时，可能会出现（minVal > maxVal）的特殊情况
    - 根据[测试](https://codepen.io/goblin-pitcher/pen/abOdLad)来看，配置为`repeat(auto-fill/auto-fit, minmax(minVal, maxVal))`时，都以maxVal作为宽度，区别如下：
      - auto-fill根据网格容器长度，确定每行/列填充多少个，填充完后，因为仍有剩余空间，因此会继续生成网格
      - auto-fit根据网格项的长度，准确定每行/列填充多少个，网格项填充完后，即使有剩余空间，也不会生成多余网格。

----

## 网格上的属性

### start/end

- 使用特定的网格线确定grid item在网格内的位置
- 前面命名网格线的作用通过此属性体现

- ````css
  .item {
      grid-(row|column)-(start|end): <line>|span [line]|auto;
  }
  ````

- `<line>：<number>|<name>`，用于标识网格线

  * 注意，网格线编号number和nth-child、nth-of-type等css属性一样，是从1开始的。

  * ````css
    .item:nth-child(2){
         /*设置此属性后，第二个item及以后的item全部后移*/
        grid-column-start: 3;
        grid-column-end: 5; 
        /*设置属性后，该项下移，其后面的item自动填充该item下移导致的空位*/
        grid-row-start: 2;
        grid-row-end: 5;
    }
    ````

  * 若grid-column-start和grid-column-end只设置一个，则item**默认只占用一个网格项**

  * 参见[例子](https://codepen.io/goblin-pitcher/pen/abOdLad)

  * number数值可以为0或负数，为0时不起作用，为负数时从尾部开始（不建议用）

- span `<number>`: 网格项将跨越指定数量的网格轨道

  - ````css
    .item:nth-child(2){
         /*start line不变，end line是start line后面两格的线*/
        grid-column-start: span 2;
    }
    .item:nth-child(2){
         /*start end不变，start line是end line前面两格的线*/
        grid-column-end: span 2;
    }
    ````

- span `<name>`: 网格项将跨越一些轨道，直到碰到指定名字的网格线

- auto：自动布局，默认跨越一个轨道

- **注意**，控制网格线的属性能使布局生成隐式网格项

- **注意**，网格项可以相互重叠，用z-index控制它们的堆叠顺序

- 简写：

  * ````css
    .item {
        /*‘/’前后的空格建议不省略，避免部分浏览器不能识别*/
        grid-column: <grid-column-start> / <grid-column-end>;
        grid-row: <grid-row-start> / <grid-row-end>;
    }
    ````

  * 

### grid-area

- 前面grid-template-areas属性中的命名在此处引用

- ````
  .item {
  	grid-area: <name>|<row-start>/<column-start>/<row-end>/<column-end>;
  }
  ````

  - `<row-start>/<column-start>/<row-end>/<column-end>`：可以是数字，也可以是网格线的名字，注意属性的书写顺序。
  - 参见[例子](https://codepen.io/goblin-pitcher/pen/dyooOKd)

### self

虽然container属性能控制item里内容的布置，但有时候部分item需要有定制化的样式

+ justify-self
  - 沿着行轴对齐grid item里的内容
  - justify-self: `<start>|<end>|<center>|<stretch>`;
+ align-self
  - align-self: `<start>|<end>|<center>|<stretch>`;