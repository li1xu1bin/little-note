## 边距重叠

边距重叠是指两个或多个盒子(可能相邻也可能嵌套)的相邻边界(其间没有任何非空内容、补白、边框)重合在一起而形成一个单一边界  

## BFC

块级格式化上下文 (Block Fromatting Context)是按照块级盒子布局 

## BFC的原理

  1、内部的box会在垂直方向，一个接一个的放置  
  2、每个元素的margin box的左边，与包含块border box的左边相接触（对于从做往右的格式化，否则相反）  
  3、box垂直方向的距离由margin决定，属于同一个bfc的两个相邻box的margin会发生重叠  
  4、bfc的区域不会与浮动区域的box重叠  
  5、bfc是一个页面上的独立的容器，外面的元素不会影响bfc里的元素，反过来，里面的也不会影响外面的  
  6、计算bfc高度的时候，浮动元素也会参与计算  
  
## 怎么创建bfc（边距重叠产生原因）

  1、float属性不为none（脱离文档流）  
  2、position为absolute或fixed  
  3、display为inline-block,table-cell,table-caption,flex,inine-flex  
  4、overflow不为visible  
  5、根元素的垂直margin不会被重叠  
    
## 防止外边距重叠解决方案
  1、外层元素padding代替  
  2、内层元素透明边框 border:1px solid transparent;  
  3、内层元素绝对定位 postion:absolute:  
  4、外层元素 overflow:hidden;  
  5、内层元素 加float:left;或display:inline-block;  
  6、内层元素padding:1px;  
  
## BFC应用场景
  1、自适应两栏布局
  2、清除内部浮动
  
## 浮动float误解和误用
 float 被设计出来的初衷是用于文字环绕效果，即一个图片一段文字，图片float:left之后，文字会环绕图片  
 但是，后来大家发现结合float + div可以实现之前通过table实现的网页布局，因此就被“误用”于网页布局了
 
## 为何 float 会导致父元素塌陷？
  1、float 的破坏性 —— float 破坏了父标签的原本结构，使得父标签出现了坍塌现象。    
  2、导致这一现象的最根本原因在于：被设置了 float 的元素会脱离文档流，我们的浮动是左右浮动，所以我们的块级元素都是左右排列。其根本原因在于 float 的设计初衷是解决文字环绕图片的问题    
  3、包裹性也是 float 的一个非常重要的特性，普通的 div 如果没有设置宽度，它会撑满整个屏幕，在之前的盒子模型那一节也讲到过。而如果给 div 增加float:left之后，它突然变得紧凑了，宽度发生了变化，把内容中的三个字包裹了——这就是包裹性。为 div 设置了 float 之后，其宽度会自动调整为包裹住内容宽度，而不是撑满整个父容器  清空格，对于高度不同的容器，float 排版出来的网页严丝合缝，

## 清除浮动（clear）

  1、额外添加标签，再最后一个浮动的盒子的后面，新添加一个标签。然后他可以清除浮动  
  2、`Overflow` 清除浮动，是浮动的大盒子（父级标签）再样式里面加: `overflow:hidden`，  常用的`ul` `li`，在`ul`里添加`overflow：hidden`  
  3、`After`伪类清除浮动  
  4、写一个通用的`clearfix`类，直接在需要清除浮动的地方加一个这个样式 ：`<div class="box clearfix">` 
  5、`After before`伪类清除浮动 是大部分大型网站常用的，比如新浪 淘宝 的清除浮动的效果  
```html
	.clearfix:before,.clearfix:after{  
		content:"";  
		display:table;  
		}  
		.clearfix:after{  
		clear:both;  
		}  
		.clearfix{ /*照顾ie6*/  
		zoom:1;  
		}  
		使用：  
		<div class="box clearfix"> 
		
```

## 定位position
position 用于网页元素的定位，可设置 `static/relative/absolute/fixed /inherit ` 

  1、static 默认  
  2、relative，会导致自身位置的相对变化，而不会影响其他元素的位置、大小  
  3、absolute 元素脱离了文档结构，导致父元素坍塌，元素具有“包裹性，元素会悬浮在页面上方，会遮挡住下方的页面内容，设置了 top、left 值时，   元素是相对于最近的定位上下文来定位的，而不是相对于浏览器定位。  
  4、fixed 元素脱离了文档结构，根据 window （或者 iframe）确定位置