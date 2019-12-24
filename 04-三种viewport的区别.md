## viewport

### viewport的概念

移动设备上的viewport就是设备屏幕上能显示我们页面上的区域。

由于移动设备的分辨率非常高，屏幕上的1px往往不是css代表的1px屏幕，屏幕1px=DPR(devicePixelRatio)*css1px

#### layout viewport(布局视图)

不设置`<meta name="viewport" content="width=device-width...">`时的默认显示

移动设备上浏览器为了适应PC端页面的显示，把自身viewport设得更宽，以适应显示页面显示

获取方式：document.body.clientWidth

![layout viewport](https://s2.ax1x.com/2019/12/22/QzdEoq.png)

#### visual viewport(视觉视口)

显示屏幕能显示的屏幕像素px值

获取方式：window.innerWidth

![visual viewport 01](https://s2.ax1x.com/2019/12/22/QzBwXF.png)

注意定义，当浏览器百分比放大或缩小时，window.innerWidth会变化

![visual viewport 02](https://s2.ax1x.com/2019/12/22/QzB6t1.png)

移动端显示时，此时viusal viewport不是设备宽度，而是屏幕像素宽度

### ideal viewport(理想视口)

布局视口的一个理想尺寸，只有当布局视口的尺寸等于设备屏幕的尺寸时，才是理想视口（document.body.clientWidth === window.screen.width）

获取方式：window.screen.width

![ideal viewport](https://s2.ax1x.com/2019/12/22/QzrFIA.png)

注意，理想视口可设置`<meta name="viewport" content="width=device-width...">`实现