# SVG

参考：[svg中文文档](https://d3js.org.cn/svg/get_start/#第一章-入门指南)

## 1、入门指南

### 1.1、 图形系统

计算机中描述图形信息的两大图形系统：

- 栅格图形：图形被表示为图片元素或者像素的长方形数组。
- 矢量图形： 图形中图形被描述为一系列几何形状，通过矢量图形阅读器在指定的坐标集上绘制形状

其中在网页中主要有两种作图系统，参考: [W3school](https://www.w3school.com.cn/html/html5_canvas_vs_svg.asp)

- SVG: 使用XML描述2D图形的语言。
    - 所有的图形有关信息被存储为纯文本，具有XML的开放性、可移植性和可交互性。当前稳定的XML和SVG版本都为1.1
    - SVG文档结构是标准的XML文档，根元素svg定义图形的大小，根元素中包含各种的形状元素。
    - 每个被绘制的图形均被视为对象。SVG允许使用单独的属性指定元素的样式。如果SVG对象的属性发生变化，那么浏览器能够自动重现图形。
    - SVG使用g元素对图形进行分组，使用use元素实现元素的复用。

- Canvas: 通过Javascript进行渲染。
    - 逐像素进行渲染
    - 在 canvas 中，一旦图形被绘制完成，它就不会继续得到浏览器的关注。如果其位置发生变化，那么整个场景也需要重新绘制，包括任何或许已被图形覆盖的对象。

### 1.2 Canvas与SVG的比较

- Canvas
    - 依赖分辨率
    - 不支持事件处理器
    - 弱的文本渲染能力
    - 能够以 .png 或 .jpg 格式保存结果图像
    - 最适合图像密集型的游戏，其中的许多对象会被频繁重绘
- SVG
    - 不依赖分辨率
    - 支持事件处理器
    - 最适合带有大型渲染区域的应用程序（比如谷歌地图）
    - 复杂度高会减慢渲染速度（任何过度使用 DOM 的应用都不快）
    - 不适合游戏应用

## 2、在网页中使用SVG

### 2.1、将SVG作为图像

将svg作为图像包含在HTML标记的img元素内，但是这样有一定的局限性：SVG转为栅格图像时与主页面分离，并且无法在两者之间通信(SVG渲染过程与主页面独立)。主页面上的样式对SVG无效，运行在主页面上的脚本无法感知或者修改SVG文档结构。

在CSS中包含SVG，最常用的是background-image属性，应该避免SVG元素文件太大。

### 2.2、将SVG作为应用程序

使用object元素将SVG嵌入HTML文档中，object元素的type属性表示要嵌入的文档类型，对用SVG应该是type="image/svg+xml"。object元素必须有起始标签和结束标签，这两个标签之间的内容为对象数据本身不能被渲染时显示。

## 3、坐标系统

### 3.1、视口 viewport

指文档打算使用的画布区域。在svg标签上使用width和height属性确定大小，可以仅以数字、数字+单位（em、ex、px、pt、pc、cm、mm和in）或者百分比表示。

示例参见: [02_svg.html](./02_svg.html#p1)

```html
<svg height="100px" width="100px" style="background-color: lightgrey"></svg>
```

### 3.2、默认用户坐标

SVG阅读器会设定一个坐标系统，原点`(0,0)`为左上角，x向右递增，y向下递增。为一个纯粹的二维笛卡尔坐标系，点为无线小，网格线为无限细。
SVG中每个单位的坐标均是相互独立的。

### 3.3、指定用户坐标

用户可以通过svg元素的`viewBox`属性自定义坐标系统。
`viewBox`由4个数值组成，分别为最小x、最小y、宽度和高度。即以左上角为原点，设定元素的宽和高。

设定了`viewBox`以后就相当于在SVG中单独做了一个作图的图层，其内的图形就成了该`viewBox`内部的原件，大小等也都变成了viewBox的相对值。

示例参见: [02_svg.html](./02_svg.html#p2)

`perserveAspectRatio`属性，可以设定`viewBox`长宽比例与其内图形的长宽比例之间的关系，有三种选择。

#### 3.3.1、viewBox和viewPort的对齐

值|含义
---|---
xMin|viewport和viewBox左对齐
xMid|viewport和viewBox x中心对齐
xMax|viewport和viewBox右对齐
YMin|viewport和viewBox上边对齐
YMid|viewport和viewBox y中心对齐
YMax|viewport和viewBox下边对齐

x和y可自由组合

#### 3.3.2、viewBox中元素与viewBox的相对关系

1. meet: 按较小的尺寸等比例缩放图形，使图形完全填充SVG。
2. slice: 按较大的尺寸等比例缩放图形，并裁掉超出部分。
3. alignment: 拉伸和压缩绘图，以使用SVG元素大小。

该属性的值由两部分构成，以空格分隔，前一部分表明`viewBox`如何与SVG元素对齐

### 3.4、嵌套坐标系统

可以将另一个svg元素插入到文档中来建立一个新的视口和坐标系统，也就是说svg中可以嵌套另一个svg，每个svg都有自己独立的视口和坐标系统。
