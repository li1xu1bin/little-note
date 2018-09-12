
## 适配移动端页面meta viewport

  ```bash
  <meta name="viewport" content="width=device-width">
  ```
  meta viewport 中有6个通用属性：  
  
  1、width 设置layout viewport的宽度 正整数或字符串 'width-device'  
  2、initial-scale 设置页面的初始缩放值，数字或小数  
  3、minimum-scale 允许用户的最小缩放值 数字或小数  
  4、maximum-scale 允许用户的最大缩放值 数字或小数  
  5、user-scaleabel 是否允许用户进行缩放 'no'或‘yes’ 还有2个需要特别注意的两个属性  
  6、target-densitydpi 在andriod 4.0一下的设备中，不支持设置viewport的width，android 自带浏览器支持设置 target-densitydpi来达到目的
  
### meta viewport

rem/viewport/media  query  
在Bootstrap的栅格系统中有：

  ```bash
  /* 超小屏幕（手机，小于 768px） */
/* 没有任何媒体查询相关的代码，因为这在 Bootstrap 中是默认的（还记得 Bootstrap 是移动设备优先的吗？） */
.col-xs-

/* 小屏幕（平板，大于等于 768px） */
@media (min-width: @screen-sm-min) { ... }
.col-sm-

/* 中等屏幕（桌面显示器，大于等于 992px） */
@media (min-width: @screen-md-min) { ... }
.col-md-

/* 大屏幕（大桌面显示器，大于等于 1200px） */
@media (min-width: @screen-lg-min) { ... }
.col-lg-
  ```
  


##  Canvas和svg
### Canvas 
  1、Canvas是基于位图的，它不能够改变大小，只能缩放显示，放大或缩小时图形质量会有所损失  
  2、 依赖分辨率，逐像素进行渲染的  
  3、 Canvas 通过 JavaScript 来绘制2D图形（动态生成）  
  4、 不支持事件处理器。Canvas输出的是一整幅画布，能够以 .png 或 .jpg 格式保存结果图像  
  5、 适合像素处理，动态渲染和大数据量绘制，最适合图像密集型的游戏（频繁重绘对象）  
  6、 如果图形位置发生变化，整个场景需要重新绘制，包括任何或许已被图形覆盖的对象  
 
### svg
  1、SVG 可缩放矢量图形（Scalable Vector Graphics），是一种使用可扩展标记语言（XML）描述2D图形的语言  
  2、不依赖分辨率,逐元素（DOM元素）进行渲染,能够很好的处理图形大小的改变,	放大或缩小时图形质量不会有所损失  
  3、 基于XML，用文本格式的描述性语言来描述图像内容  
  4、 支持事件处理器。SVG绘制出的每个图形元素都是独立的DOM节点，能够方便的绑定事件  
  5、 适合静态图片展示，高保真文档查看和打印的应用场景，不适合游戏应用）  
  6、 如果对象属性发生变化，浏览器能自动重现图形。也就是说，SVG绘图很容易编辑，只需要增加或移除相应的元素即可  

## css动画

#### 动画类型
    1、transition补间动画
    2、keyframe关键帧动画
    3、animation逐帧动画
#### 补间动画
    1、位置-平移（left/right/margin/transform）
    2、方位-旋转（transform）
    3、大小-缩放（transform）
	4、透明度（opacity）
	5、其他线性变换（transform）
#### keyframe关键帧动画
  ```js
@keyframes testAnimation
{
    0%   {background: red; left:0; top:0;}
    25%  {background: yellow; left:200px; top:0;}
    50%  {background: blue; left:200px; top:200px;}
    75%  {background: green; left:0; top:200px;}
    100% {background: red; left:0; top:0;}
}
div {
    width: 100px;
    height: 50px;
    position: absolute;

    animation-name: myfirst;
    animation-duration: 5s;
}
  ```
 #### animation逐帧动画
  ```bash 
.fadeIn{
  animation: fadeIn .5s ease 1s both;
}
@keyframes fadeIn{
  from{
    opacity:0;
  }
  to{
    opacity:1
  }
}
 ```
 
