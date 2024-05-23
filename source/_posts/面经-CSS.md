---
title: 面经-CSS
date: 2022-08-01
categories: 面经
tags: [面经,CSS]

---

## HTML5新增了哪些新特性

1. 添加了语义化标签，如\<header\>、\<footer\>、\<section\>等，让网页结构更加清晰
2. 新增\<video\>和\<audio\>两个标签，让网页可以直接播放视频或音频而不用借助flash等插件。
3. 新增了\<canvas\>标签，可以使用JS在其中绘图
4. 新增了Web Workers API。可以让JS在后台执行，不影响页面性能
5. 新增了localstorge和sessionstorge两种本地存储方式
6. 新增了Web Socket API。可以让服务器与客户端实现实时全双工通信
7. 新增了一些表单控件

## 盒模型

盒模型有标准盒模型与IE盒模型。

标准盒模型：盒子总宽度为width+padding+border+margin

即width/height指明了内容区高度与宽度，不包含padding与border

IE盒模型：盒子总宽度为width+margin

即width/height指明了内容区加内边距加边界的长度

使用box-sizing: content-box|border-box|inherit: 来指定使用哪种盒模型

## CSS选择器及其优先级

CSS常用的选择器有id选择器(#box)，类选择器(.one)，标签选择器(div)，后代选择器(#box div)，伪类选择器(:hover)，伪元素选择器(:before)。CSS选择器的优先级有物种

!important>内联>id选择器>类选择器>标签选择器

## CSS中的长度单位

px：表示像素，绝对单位

em：相对长度单位，等于当前对象内文本的字体尺寸，会继承父级元素的字体大小

rem：相对单位，相对的是HTML根元素

vh/vw:根据窗口宽高分成100份，100vw即为满宽

## 隐藏页面元素的方法

display：none 不可见，不占据空间，不响应点击事件

visibility：hidden 不可见，占据页面空间，不响应点击事件

opacity：0 不可见，占据页面空间，响应点击事件

## BFC

即块级格式化上下文是Web页面的可视化CSS渲染的一部分，是块盒子的布局过程发生的区域，也是浮动元素与其他元素交互的区域。它是一个独立的渲染区域，具有一套属于自己的渲染规则，决定了元素如何对齐内容进行布局，以及与其他元素的关系和相互作用。BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之亦然。

BFC的一些独特规则：

- BFC内部会有margin塌陷，外界块和BFC内部的块不存在margin塌陷
- 每个元素的左外边距会与包含块的左边界相接触，即使浮动元素也是如此
- BFC的区域不会与float元素的区域重叠
- 计算BFC高度时，浮动子元素也参与计算

BFC的触发条件：

- 根元素，即HTML元素本身就是一个BFC
- 浮动元素：float值为left、right
- overflow值不为visible，为auto、scroll、hidden
- 弹性盒、表格、栅格布局是BFC
- position的值为absolute或者fixed

## 定位方式

使用position确定元素的定位方式，其可选值有：

static：按照正常文档流定位

relative：相对定位，相对正常位置定位

absolute：相对第一个被定位的祖先元素定位

fixed：相对于浏览器窗口定位

## 回流和重绘

回流：布局引擎会根据各种样式计算每个盒子在页面上的大小与位置

重绘：计算好盒模型的位置，大小和其他属性后，在页面上进行绘制

回流一定会触发重绘，但重绘不一定会触发回流

减少回流与重绘的方法：

- 要设定元素的样式，通过改变元素的class类名(集中处理样式变更)
- 使用transform代替top、left(transform不会触发回流重绘)
- 对于复杂动画，对其设置position：fixed/absolute，尽可能使其脱离文档流，减少对其他元素的影响
- 避免使用table布局，table中每个元素的变动都会导致整个table重新计算

## 水平垂直居中的几种方式

1. 定位+margin:auto  需要知道子级宽高。将父级设置为相对定位relative，子级设置为绝对定位absolute，并将四个定位属性设为0，让子元素的占位撑满父元素，再给子元素设置宽高，margin设置为auto，可实现元素居中
2. 定位+margin负值  需要知道子级宽高。将父级设置为相对定位relative，子级设置为绝对定位absolute，top与left设置为50%，再设置margin为负的一半宽高，可实现元素居中。
3. 定位+transform  不需知道子级宽高。将父级设置为相对定位relative，子级设置为绝对定位absolute，top与left设置为50%，再设置transform属性为translate(-50%,-50%)，可实现元素居中
4. table布局  父级元素设置为display:table-cell，子级元素设置为display:inline-block，再在父级中设置vertical-align:middle，text-align:center
5. flex布局\grid布局  父级元素设置为flex，设置align-item:center；justify-content:center

## CSS3.0新增了哪些特性

1. Flex布局
2. Grid布局
3. 2D/3D变换  用CSS可实现元素的平移、旋转、缩放变换
4. 媒体查询  CSS可根据不同的设备和屏幕尺寸应用不同的样式
5. 动画及过渡  使用CSS可以实现平滑的动画和过渡效果
6. 新的选择器  包括属性选择器[attribute=value]、伪类选择器:focus、:first-child等
7. 阴影和圆角  CSS可实现元素的阴影与圆角效果

## CSS媒体查询

CSS3的特性，可以根据不同的设备和屏幕尺寸应用不同的样式。媒体查询可以检测设备屏幕的宽度、高度、方向、分辨率、屏幕比例等信息。

查询方法为@media (max-width:360px) and (device-height:500px)

## 浏览器渲染解析机制

1. 解析HTML，生成DOM树
2. 解析CSS，生成CSS规则树
3. 根据DOM树与CSS规则树生成渲染树
4. 回流：根据渲染树，进行回流，计算盒子的位置和大小信息
5. 重绘：根据渲染树和盒子的几何信息计算页面的像素信息
6. 将像素信息发送给GPU，展示在页面上

## 响应式设计

响应式网站设计是一种网络页面设计布局，页面的设计与开发应当根据用户行为以及设备环境进行相应的响应和调整。

实现方法：

1. 媒体查询
2. 使用百分比来定义样式
3. 使用vh、vw

## CSS提高性能的方法

- 内联首屏关键CSS 将一些关键CSS内联到HTML中，使其不必等待CSS文件的下载
- 资源压缩 使用webpack等模块化工具，将CSS代码进行压缩，使文件变小，降低了浏览器的加载时间
- 合理使用选择器 避免使用过深的选择器，使用了id选择器就不应再使用嵌套选择器等
- 尽量避免使用@import。@import用于导入外部样式表(另外一种是link)，它会影响浏览器的并行下载，使得页面加载时增加额外的延迟

## CSS预编语言

CSS预处理器是扩展CSS语言的工具。通过添加一些额外的语法和功能，使得开发者可以更加轻松、高效地编写CSS代码。减少重复的CSS代码，提高代码的复用性和可维护性。常见CSS预处理器有Sass、Less、Stylus等

主要功能：

- 变量：可以定义和使用变量，以便在样式表中多次使用同一个值
- 嵌套：将样式表中的规则嵌套在一起，以便更加清晰地表示元素之间的关系。
- 混入：类似于函数，可以在样式表中重复使用相同的样式
- 继承：可以定义和使用继承规则，从而避免重复编写相同的样式
- 运算符：可以使用数学运算符，例如加减乘除，从而更加方便地进行样式计算

## Canvas和SVG的区别

- SVG是矢量图，Canvas是标量图，所以可以向Canvas中引入png，jpg图片，而SVG不行
- Canvas没有图层的概念，有修改就会导致整个画布的重新渲染，而SVG可以单独修改某一标签
- Canvas主要通过JS脚本来对整个画布操作的，而SVG则是基于xml元素的
- SVG发布日期较早，功能较为完善
- Canvas不能给某个图形添加事件处理函数，而SVG可以
