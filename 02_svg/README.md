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

SVG阅读器会设定一个坐标系统，原点`(0,0)`为左上角，x向右递增，y向下递增。为一个纯粹的二维类笛卡尔坐标系（更像是笛卡尔坐标系的第四象限），点为无线小，网格线为无限细。
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

## 4、基本形状

1. 线段: [`line`](./02_svg.html#p5)
    使用`x1`, `x2`, `y1`, `y2`来标定起始位点。
    - stroke-width: 线条宽度，可以使用css的`shape-rendering`来控制反锯齿特性。
    - stoke: 线条颜色
    - stroke-opacity: 线条的不透明度。
    - stoke-dasharray: 虚线，一系列数字组成，数字个数为偶数，表示线长-间隙-线长-间隙

2. 矩形: [`rect`](./02_svg.html#p6)
    使用`x`, `y`, `width`, `height`表示矩形
    - fill: 填充颜色
    - fill-opacity: 填充不透明度
    - stroke: 边框颜色
    - stroke-width: 边框宽度，边框骑在分界线上，一半在矩形内，一半在外
    - rx/ry: 圆角矩形，最大值为矩形宽/高的一般，如果只指定其中一个，则默认两值相同。

3. 圆形与椭圆: [`circle/ellipse`](./02_svg.html#p7)
    - 圆形使用`cx`, `cy`, `r`分别标定圆形的位点和半径。
    - 椭圆则用`cx`, `cy`, `rx`, `ry`
    用`rect`做正方形，然后`rx/ry`设定为长宽的一般，也可以做圆形
    - fill: 填充颜色
    - fill-opacity: 填充不透明度
    - stroke: 不透明度
    - stroke-width: 边框宽度，骑边

4. 多边形: [`polygon`](./02_svg.html#p8)
    由一系列points指定边界，会自动封闭
    - fill: 填充颜色
    - fill-opacity: 填充不透明度
    - stroke: 边框颜色
    - stroke-width: 边框宽度
    - fill-rule: 填充规则，如果多边形有交叉时，则需要指定
        - nonzero: 默认, 判断一个点是在多边形内部还是外部时，从这个点画一条到无穷远的射线，然后数这个线和多边形的边有多少次交叉。如果交叉的边线是从右往左画，则总数加1，如果是从左往右则总数减1.如果最后总数为0则认为改点在图形外部，否则在内部。
        - evenodd: 只数射线与多边形边的交叉次数，如果为奇数则认为在多边形内部，否则认为在多边形外部。

5. 折线: [`polyline`](./02_svg.html#p9)
    使用points指定边界，不自动封闭

## 5、文档结构

### 5.1、结构和表现

SVG允许文档表现和文档结构分离，SVG有四中指定的表现信息：

- 内联样式: 元素使用内部style属性
- 内部样式: 内部样式定义在defs元素内部

    ```html
    <svg width="200px" height="200px" >
    <defs>
        <style type="text/css"><![CDATA[
            circle{
                fill:#ccc
            }
        ]]></style>
    </defs>
    <circle cx="10" cy="10" r="5"/>
    </svg>
    ```

- 外部样式: 将样式定义在css文件中，使用选择器来设定相应的元素样式
- 表现属性: 允许以属性的形式指定表现样式，但是**优先级最低**，如果其他三种方式指定了相同的属性，则覆盖该形式

### 5.2 分组和引用

- g: g元素用来将其子元素作为一个组合，可以使文档结构更清晰。除此之外，在g标签中指定的所有样式会应用于组合内的所有子元素，可以不用在所有子元素上指定属性。
- use: use元素用来复用图形中重复出现的元素，需要为use标签的xlink:href指定URI来引用指定的图形元素。同时还要指定x和y属性以表示组合应该移动到哪个位置。use元素并不限制只能使用同一个文件内的对象，xlink:href属性可以指定任何有效的文件或URI。
- defs: 用来定义复用的元素，但是定义在defs内的元素并不会被显示，而是作为模板供其他地方使用。
- symbol: symbol永远不会被显示，也可以用来指定被后续使用的元素，symbol元素可以指定viewBox和preserveAspectRatio属性。在引用时通过为use元素指定width和height属性就可以让symbol元素适配视口大小。
- image: 可以用来包含一个完整的SVG或栅格文件。如果包含一个SVG文件，则视口会基于引用的文件的x,y,width,height属性来建立。如果包含栅格文件则会被缩放以适配该属性指定的矩形。SVG规范要求SVG阅读器支持JPEG和PNG两种栅格文件

## 6、坐标转换系统

### 6.1、translate 平移

translate变换用来对用户坐标进行平移，通过制定transform属性值来设置:`transform = "translate(x,y)"`。

translate工作原理:首先获取整个网络，然后将其移动到画布的新位置而不是移动所在的元素，也就是说移动的是整个坐标系统而不是元素本身。看似比移动元素复杂，其实在使用其他一系列变换时，这种移动整个坐标系的方法从数学和概念上讲，更方便。

### 6.2、scale 缩放

`transform = "scale(value)"`或者`transform="scale(x-value,y-value)"`

仅仅使用scale(n)变换时，网格系统的原点位置并没有变化，只是每个用户坐标都变成了原来的n倍，也就是网格变大了，因此线也会变粗(用户单位并没有变)。

技巧：如果从其他系统传输数据到SVG，则可能必须处理使用笛卡尔坐标表示的矢量图形，在笛卡尔坐标系统中，原点位于左下角，y向上递增，x向右递增。而SVG坐标原点位于左上角，此时使用scale(1,-1)就可以完成两者之间的转换。

### 6.3 rotate 旋转

根据指定的角度旋转坐标系统，默认的坐标系统中，角度的测量顺时针增加，0度为3点钟方向。

注意，除非另行指定，否则旋转以原点为中心。 此时可以通过平移+旋转的方式来指定旋转中心

- `translate(centerX,centerY)`
- `rotate(angle)`
- `translate(-centerX,-centerY)`

但是有个更简单的方式：`rotate(angle,centerX,centerY)`

### 6.4 围绕中心点缩放

`translate(-centerX*(factor-1),-centerY*(factor-1)) scale(factor)`

### 6.5 skewX和skewY变换 倾斜坐标轴

这两个变换用来倾斜某个轴，一般形式为`skewX(angle)`,`skewY(angle)`。这样的结果就是使得x轴和y轴不再垂直。

### 6.6 矩阵变换

计算机图形学中坐标变换都通过矩阵来实现，除上述变换方法之外，还可以直接为变换指定变换矩阵，变换矩阵为matrix(a,b,c,d,e,f)，此时指定的变换矩阵为:

```bash
a  c  e
b  d  f
0  0  1
```

## 7、路径

### 7.1 path

路径: [`path`](./02_svg.html#p10)

 在`d`中使用标签添加一堆的路径映射。可以通过制定一系列相互连接的线、弧、曲线来绘制任意形状的轮廓，这些轮廓也可以填充或者绘制轮廓线，也可以用来定义裁剪区域或蒙版。

命令|参数|说明
---|---|---
M/m|x/y|移动画笔到指定坐标
L/l|x/y|从起点画一条到指定坐标的直线
H/h|x|从起点开始，画一条到特定x轴坐标的横线
V/v|y|在y轴坐标上做一条横线
A/a|rx/ry/x-axis-rotation/large-arc/sweep/x/y|圆弧曲线命令有7个参数，依次表示x方向半径、y方向半径、旋转角度、大圆标识、顺逆时针标识、目标点x、目标点y。大圆标识和顺逆时针以0和1表示。0表示小圆、逆时针
Q/q|x1/y1/x/y|绘制一条从当前点到x,y控制点为x1,y1的二次贝塞尔曲线
T/t|x/y|绘制一条从当前点到x,y的光滑二次贝塞尔曲线，控制点为前一个Q命令的控制点的中心对称点，如果没有前一条则已当前点为控制点。
C/c|x1/y1/x2/y2/x/y|绘制一条从当前点到x,y控制点为x1,y1 x2,y2的三次贝塞尔曲线
S/s|x2/y2/x/y|绘制一条从当前点到x,y的光滑三次贝塞尔曲线。第一个控制点为前一个C命令的第二个控制点的中心对称点，如果没有前一条曲线，则第一个控制点为当前的点
Z|closepath|闭合

### 7.2、marker

[`marker`](./02_svg.html#p11)

marker元素用来在path上添加一个标记，比如箭头之类的。

首先需要定义好marker元素，然后在path中引用，一个marker标记是一个独立的图形，有自己的私有坐标。

```html
<defs>
    <marker id="marker" markerWidth="10" markerHeight="10" refX="0" refY="4" orient="auto">
        <path d="M 0 0 4 4 0 8" style="fill:none;stroke:black;"/>
    </marker>
</defs>

<path d="M 10 20 100 20 A 20 30 0 0 1 120 50 L 120 110"
    style="marker-start:url(#marker);marker-mid:url(#marker);marker-end:url(#marker);fill:none;stroke:black;"/>
```

marker属性|说明
---|---
markerWidth|marker标记的宽度
markerHeight|marker标记的高度
refX/refY|指定marker中的哪个坐标与路径的开始坐标对齐
orient|自动旋转匹配路径的方向，需要设置为auto
markerUnits|这个属性决定标记的坐标系统是否需要根据path的笔画宽度调整，如果设置为strokeWidth，则标记会自动调整大小。如果设置为useSpaceOnUse，则不会自动调整标记的大小。
viewBox/preserveAspectRatio|设置标记的显示效果，比如可以将标记的(0,0)设置在标记网格中心

> path中使用marker-start,marker-mid,marker-end,marker属性来设置标记的位置。如果使用marker属性，则表示同时设置marker-start,marker-mid,marker-end三个属性。

```css
path{
    marker:url(#marker_id);
}

path{
    marker:url(#marker_id);
}
marker#marker_id path{ //marker下的id为marker的元素下的path元素不需要marker标记
    marker:none;
}
```

## 8、图案和渐变

### 8.1、图案

使用图案填充图形，首先要定义一个水平或垂直方向的重复的图案对象，然后用它填充另一个对象或者作为笔画使用。这个图形对象称为"tile"(瓷砖)。

图案对象使用pattern元素定义，pattern元素内部包裹了图案的path元素。定义好之后下一个需要解决的问题是如何排列图案，那就需要使用patternUnits属性.

- patternUtils = objectBoundingBox: 如果希望图案的大小基于要填充对象的大小计算，则需要设置patternUnits属性为objectBoundingBox(0到1之间的小数或百分比)，并需要指定图案左上角的x和y坐标。
- patternUtils = userSpaceOnUse: 除了基于被填充对象尺寸方式之外，还可以按用户单位制定图案的width和height。此时要设置patternUnits值为userSpaceOnUse。
- patternContentUnits属性: 如果想基于被填充图形设置，则需要设置patternContentUnits属性。patternContentUnits属性默认为userSpaceOnUse,当设置patternContentUnits属性为objectBoundingBox时就可以使用百分比来设置图案的大小。

### 8.2、线性渐变

线性渐变是一系列颜色沿着一条直线过渡，在特定的位置指定想要的颜色，被称为渐变点。渐变点是渐变结构的一部分，颜色是表现的一部分。

stop元素有两个必要属性：offset和stop-color。offset属性用来指定在哪个点的颜色应该等于stop-color。offset的取值范围0%-100%。

stop元素的属性：

属性|说明
---|---
offset|必需，取值范围0%-100%
stop-color|必需，对应offset位置点的颜色
stop-opacity|对应offset位置点的不透明度

linearGradient元素属性：

属性|说明
---|---
x1,y1|渐变的起点位置，使用百分比表示，默认的渐变方向是从左到右
x2,y2|渐变的终点位置，使用百分比表示
spreadMethod|如果设置的offset不能覆盖整个对象，该怎么填充。pad:起点或终点颜色会扩展到对象边缘。repeat:渐变重复起点到终点的过程。reflect:渐变按终点-起点-终点的排列重复。

### 8.3、径向渐变

径向渐变的每个渐变点是一个圆形路径，从中心点向外扩散。设置方式与线性渐变大致相同。如果填充对象边界框不是正方形的，则过渡路径会变成椭圆来匹配边界框的长宽比。

radialGradient元素属性：

属性|说明
---|---
cx,cy,r|定义渐变的范围，测量半径的单位是对象的宽高均值，而不是对角线，默认都为50%
fx,fy|0%点所处的圆路径的圆心，默认和cx,cy一样
spreadMethod|pad,repeat,reflect三个值，用来解决绘制范围没有到达图形边缘的情况

## 9、文本

### 9.1、基本概念

文本: [`text`](./02_svg.html#p10)
    通过`x`, `y`指定坐标，`tranform`等进一步设定特性，基本上是通用的特性

术语|说明
---|---
字符|XML中，字符是指带有一个数字值得一个或多个字节，数字值与Unidode标准对应
符号|字符的视觉呈现。每个字符可以有多种视觉呈现
字体|代表某个字符集合的一组符号
基线|字体中所有符号以基线对齐
上坡度|基线到字体中最高字符的顶部距离
下坡度|基线到最深字符底部的距离
大写字母高度、x高度|大写字母高度是指基线上大写字母的高度，x高度是基线到小写字母x顶部的高度

### 9.2、text元素的基本属性：

text元素以指定的x和y值作为元素内容第一个字符的基线位置，默认样式黑色填充、没有轮廓。

属性|说明
---|---
font-family|以空格分割的一系列字体名称或通用字体名称
font-size|如果有多行文本，则font-size为平行的两条基线的距离
font-weight|两个值：bold(粗体)和nromal(默认)
font-style|常用的两个值:italic(斜体)和normal
text-decoration|可能的值:none,underline(下划线),overline(上划线),line-through(删除线)
word-spacing|单词之间的距离
letter-spacing|字母之间的间距
text-anchor|对齐方式：start,middle,end
textLength|设置文本的长度
lengthAdjust|在指定了textLength时，可以通过lengthAdjust属性设置字符的调整方式，值为 spacing(默认)时,只调整字符的间距。当值为spacingAndGlyphs时，同时调整字符间距和字符本身的大小

### 9.3、tspan元素

text元素无法对文本进行换行操作，如果需要分行显示文本，则需要使用tspan元素。tspan元素与html的span元素类似，可以嵌套在文本内容中，并可以单独改变其内部文本内容的样式。

tspan元素除大小，颜色等表现样式之外，还可以设置以下属性：
属性|说明
---|---
dx,dy|x和y方向的偏移
x,y|对tspan进行绝对定位
rotate|旋转字符，可以同时设置多个值，这些值会依次作用在tspan包裹的字母上
baseline-shift|与dy属性设置上下标相比，这个属性更方便，当为super时，为上标。sub时为下标。仅仅在所在的tspan内有效

### 9.4 纵向文本

文本一般从左到右排列，如果需要上下排列，则需要使用`writing-mode`属性。

设置`writing-mode`属性值为`tb(top to bottom)`，可以将文本上下排列。

### 9.5、文本路径

如果要使得文本沿着某条路径排列，则需要使用textPath元素。需要将文本放在textPath元素内部，然后使用textPath元素的xlink:href属性引用一个定义好的path元素。

startOffset属性用来指定文本的起点，当设置为50%，并且设置text-anchor为middle时，文本会被定为在path的中间。

## 10、裁剪和蒙版

### 10.1、裁剪路径

在创建SVG文档时，可以通过指定感兴趣的区域的宽度和高度建立视口，这个视口就会变成默认的裁剪区域，裁剪区域外的任何部分都不会被显示。裁剪区域可以通过clipPath元素建立自己的裁剪区域。

### 10.2 蒙版

SVG的蒙版会变换对象的透明度，如果蒙版是不透明的，则被蒙版覆盖的对象的像素就是不透明的，如果蒙版是半透明的，则对象就是半透明的。

使用mask元素创建蒙版，使用x,y,width,height指定蒙版的尺寸，这些尺寸默认按照objectBoungdingBox计算，如果想根据用户空间坐标计算，则需要设置mask元素的maskUnits值为userSpaceOnUse。

mask之间是想要用作蒙版的任意基本形状、文本图像或路径。这些元素默认坐标使用用户坐标空间表达，如果想要这些元素使用对象的边界框，则设置maskContenUnits属性为objectBoungdingBox即可。

注意与maskUnits属性的区别，maskUnits针对mask元素，maskContenUnits属性针对mask元素内的元素。

> 以下 内容看的非常粗糙，仅将示例代码写了一下，看了一下效果

## 11、滤镜

### 11.1、原理

SVG阅读器处理一个图形对象时，会将对象呈现在位图输出设备上，它可以将对象的描述信息转化为一组对应的像素。在使用滤镜时，SVG阅读器不会直接将图形渲染为最终结果，而是先将像素保存到临时位图中，然后将滤镜指定的操作应用到该临时位图，其结果作为最终图形。

在SVG中，使用filter元素指定一组操作(也叫基元),在渲染图形对象时，将该操作应用在最终图形上。

filter标记之间就是我们想要的滤镜基元，每个基元有一个或多个输入，但是只有一个输出，输入可以是原始图形(SourceGraphic)、图形的阿尔法通道(不透明度，SourceAlpha)或者是前一个滤镜基元的输出。


## SVG动画

