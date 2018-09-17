## 盒子模型
`box-sizing: content-box`是标准盒子模型  
`box-sizing: border-box` 是IE盒子模型

margin(外边距) - 清除边框外的区域，外边距是透明的。   
border(边框) - 围绕在内边距和内容外的边框。   
padding(内边距) - 清除内容周围的区域，内边距是透明的。   
content(内容) - 盒子的内容，显示文本和图像。

设定width后：
* 在标准的盒子模型中，width指content部分的宽度  
呈现的盒子实际宽度 width = content + paddingx2 + borderx2

* 在IE盒子模型中，width表示content+padding+border这三个部分的宽度  
呈现的盒子实际宽度 width = 设定的width，所以content会缩小

## 定位
position 用于网页元素的定位
1. `static` 默认  
2. `relative` 相对定位，会导致自身位置的相对变化，而不会影响其他元素的位置
3. `absolute` 绝对定位，相对于最近的已定位祖先元素，若无，则相对于最初的包含块
4. `fixed` 相对于窗口定位
5. `sticky` 粘性定位


## 清除浮动
1.添加空div，{clear:both;height:0;overflow:hidden;}  
2.给父级添加 `overflow:hidden`  
3.`after`伪类清除浮动  

```css
.float_div:after{
	content:".";
	clear:both;
	display:block;
	height:0;
	overflow:hidden;
	visibility:hidden;
}
```
4.写一个通用的`clearfix`类，直接在需要清除浮动的地方加这个类 ：

```css
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
```

## BFC
BFC 即 Block Formatting Contexts (块级格式化上下文)  
具有 BFC 特性的元素可以看作是隔离了的独立容器，容器里面的元素不会在布局上影响到外面的元素，并且 BFC 具有普通容器所没有的一些特性。

## 触发BFC
只要元素满足下面任一条件即可触发 BFC 特性：
* body 根元素
* 浮动元素：float 不为none
* 绝对定位元素：position (absolute、fixed)
* display 为 inline-block、table-cells、flex
* overflow 不为visible 

### ①边距重叠
* 同一个 BFC 下外边距会发生折叠  
```css
<head>
div{
    width: 100px;
    height: 100px;
    background: lightblue;
    margin: 100px;
}
</head>
<body>
    <div></div>
    <div></div>
</body>
```
![pic](https://pic4.zhimg.com/80/v2-0a9ca8952c83141250a2d9002e6d2047_hd.png)


因为两个 div 元素都处于同一个 BFC 容器下 (这里指 body 元素) 所以第一个 div 的下边距和第二个 div 的上边距发生了重叠，所以两个盒子之间距离只有 100px，而不是 200px。

**如果想要避免外边距的重叠，可以将其放在不同的 BFC 容器中。**
```css
<div class="container">
    <p></p>
</div>
<div class="container">
    <p></p>
</div>
.container {
    overflow: hidden;
}
p {
    width: 100px;
    height: 100px;
    background: lightblue;
    margin: 100px;
}
```
### ②BFC 可以包含浮动的元素
```html
<div style="border: 1px solid #000;">
    <div style="width: 100px;height: 100px;background: #eee;float: left;"></div>
</div>
```
内部元素浮动，导致父级元素高度塌陷。  
解决方案：给父级添加 `overflow:hidden`，创建BFC。 
 
### ③BFC 可以阻止元素被浮动元素覆盖
```html
<div style="height: 100px;width: 100px;float: left;background: lightblue">我是一个左浮动的元素</div>
<div style="width: 200px; height: 200px;background: #eee">我是一个没有设置浮动, 
也没有触发 BFC 元素, width: 200px; height:200px; background: #eee;</div>
```
![pic](https://pic4.zhimg.com/80/v2-dd3e636d73682140bf4a781bcd6f576b_hd.png)

 如果想避免元素被覆盖，可触第二个元素的 BFC 特性，在第二个元素中加入 overflow: hidden，就会变成：

![pic](https://pic3.zhimg.com/80/v2-5ebd48f09fac875f0bd25823c76ba7fa_hd.png)

[10 分钟理解 BFC 原理](https://zhuanlan.zhihu.com/p/25321647) 



