## 参考
[Flex 布局教程：语法篇](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)

[Flex 布局教程：实例篇](http://www.ruanyifeng.com/blog/2015/07/flex-examples.html)

[A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)

[A Visual Guide to CSS3 Flexbox Properties](https://scotch.io/tutorials/a-visual-guide-to-css3-flexbox-properties)

## 语法篇笔记
>Flex是Flexible Box的缩写，意为"弹性布局"，用来为盒状模型提供最大的灵活性

#### 1、特点
- 任何一个容器都可以指定为Flex布局
- 行内元素也可以使用Flex布局
- Webkit内核的浏览器，必须加上`-webkit-`前缀
- 设为Flex布局以后，子元素的`float`、`clear`和`vertical-align`属性将失效

#### 2、概念
>采用Flex布局的元素，称为Flex容器（flex container），简称"容器"。它的所有子元素自动成为容器成员，称为Flex项目（flex item），简称"项目"。
![容器图](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071004.png)

>容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。主轴的开始位置（与边框的交叉点）叫做m`ain start`，结束位置叫做`main end`；交叉轴的开始位置叫做`cross start`，结束位置叫做`cross end`。项目默认沿主轴排列。单个项目占据的主轴空间叫做`main size`，占据的交叉轴空间叫做`cross size`。

#### 3、容器的属性
以下6个属性设置在容器上。

- flex-direction
- flex-wrap
- flex-flow
- justify-content
- align-items
- align-content

##### 3.1、flex-direction
>属性作用：决定主轴的方向（即项目的排列方向）。

![示例图](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071005.png)

属性值：

- row（默认值）：主轴为水平方向，起点在左端
- row-reverse：主轴为水平方向，起点在右端
- column：主轴为垂直方向，起点在上沿
- column-reverse：主轴为垂直方向，起点在下沿


##### 3.2、flex-wrap
>属性作用：默认情况下，项目都排在一条线（又称"轴线"）上。`flex-wrap`属性定义，如果一条轴线排不下，如何换行。

![示例图](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071006.png)

属性值：

- nowrap（默认值）：不换行
- wrap：换行，第一行在上方
- wrap-reverse：换行，第一行在下方

##### 3.3、flex-flow
>属性作用：`flex-flow`属性是`flex-direction`属性和`flex-wrap`属性的简写形式，默认值为`row nowrap`。

格式：

	.box {
	  flex-flow: <flex-direction> || <flex-wrap>;
	}



##### 3.4、justify-content
>属性作用：定义了项目在主轴上的对齐方式。

![示例图](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071010.png)

属性值：

- flex-start（默认值）：左对齐
- flex-end：右对齐
- center：居中
- space-between：两端对齐，项目之间的间隔相等
- space-around：每个项目两侧的间隔相等，项目之间的间隔比项目与边框的的间隔大一倍


##### 3.5、align-items
>属性作用：定义项目在交叉轴上如何对齐。

![示例图](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071011.png)

属性值：
- flex-start：交叉轴的起点对齐
- flex-end：交叉轴的终点对齐
- center：交叉轴的中点对齐
- baseline：项目的第一行文字的基线对齐
- stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度

##### 3.6、align-conten
>属性作用：定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。

![示例图](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071012.png)

属性值：

- flex-start：与交叉轴的起点对齐
- flex-end：与交叉轴的终点对齐
- center：与交叉轴的中点对齐
- space-between：与交叉轴两端对齐，轴线之间的间隔平均分布
- space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍
- stretch（默认值）：轴线占满整个交叉轴


#### 4、项目的属性
以下6个属性设置在项目上：

- order
- flex-grow
- flex-shrink
- flex-basis
- flex
- align-self

##### 4.1、order
>属性作用：定义项目的排列顺序。数值越小，排列越靠前，默认为0。

![示例图](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071013.png)

属性值：<integer>


##### 4.2、flex-grow
>属性作用：定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。

![示例图](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071014.png)

属性值：<number>

**tips**：

- 如果所有项目的flex-grow属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的flex-grow属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。


##### 4.3、flex-shrink
>属性作用：定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。

![示例图](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071015.jpg)

属性值：<number>

**tips**:

- 属性作用：定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。
- 负值对该属性无效。


##### 4.4、flex-basis
>属性作用：定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。

属性值：<length> | auto

**tips**：

- 它可以设为跟width或height属性一样的值（比如350px），则项目将占据固定空间。


##### 4.5、flex
>属性作用：flex属性是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。后两个属性可选。

属性值：none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]

**tips**：

- 该属性有两个快捷值：auto (1 1 auto) 和 none (0 0 auto)
- 建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值


##### 4.6、align-self
>属性作用：align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。

![示例图](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071016.png)

属性值：auto | flex-start | flex-end | center | baseline | stretch;


## 实例篇笔记

