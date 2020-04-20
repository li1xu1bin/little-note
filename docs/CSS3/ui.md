## 圆角
`border-radius:数值|百分比`  

* `border-radius:50%`可以实现一个正圆  

[圆角八个值演示](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Background_and_Borders/Border-radius_generator)  

[可视化演示](https://9elements.github.io/fancy-border-radius/)  

```css
  border-radius : 50px;
  //完整写法如下
  border-radius : 50px 50px 50px 50px / 50px 50px 50px 50px;
  “/”前的四个数值表示圆角的水平半径，后面四个值表示圆角的垂直半径

```


## 阴影

`box-shadow: 水平阴影 垂直阴影 模糊距离 阴影尺寸 阴影颜色 inset(内嵌阴影);`  

* `box-shadow: 10px 10px 5px #888888;`

[阴影演示](http://www.css88.com/tool/css3Preview/Box-Shadow.html)  



## 渐变

线性渐变：`background: linear-gradient(方向|角度, 颜色1 (百分比), 颜色2 (百分比), ...);`

```css
  /* 默认从上到下 */
  background: linear-gradient(red, blue); 

  /* 设定从左到右 */
  background: -webkit-linear-gradient(left, red , blue); /* Safari 5.1 - 6.0 */
  background: linear-gradient(to right, red , blue); /* 标准的语法 */
  
  /* 设定从左上角到右上角 */
  background: linear-gradient(135deg, red , blue); 

  /* 设定百分比，色块效果 */
  background: linear-gradient(red 50%, blue 50%, blue 50%); 

```


径向渐变：`background: radial-gradient(center, shape size, 颜色1 (百分比), 颜色2 (百分比), ...);`

```css
  background: radial-gradient(red 5%, green 15%, blue 60%); /* 标准的语法 */
```
渐变形状，可选参数，可以取值circle或ellipse

渐变大小，可循环参数，可以取值  
closest-side：指定径向渐变的半径长度为从圆心到离圆心最近的边  
closest-corner：指定径向渐变的半径长度为从圆心到离圆心最近的角  
farthest-side：指定径向渐变的半径长度为从圆心到离圆心最远的边  
farthest-corner：指定径向渐变的半径长度为从圆心到离圆心最远的角  
contain：包含，指定径向渐变的半径长度为从圆心到离圆心最近的点。类同于closest-side  
cover：覆盖，指定径向渐变的半径长度为从圆心到离圆心最远的点。类同于farthest-corner    

## 动画
**1. transition 补间动画**  
**2. animation 逐帧动画**  
**3. keyframe 关键帧动画**

#### 补间动画

`transition: property duration timing-function delay;`

property可以是transform，margin，width，left，opacity...

```css
  transition: all 0 ease 0;

```

#### animation结合keyframe
`animation: name duration timing-function delay iteration-count direction;`

  ```css
  div {
     animation:mymove 5s infinite;
  }
  @keyframes mymove
  {
    from {top:0px;}
    to {top:200px;}
  }
  @-webkit-keyframes mymove
  {
    0%   {top:0px;}
    25%  {top:100px;}
    100% {top:200px;}
  }
```
## 滤镜

` filter: none | <filter-function > [ <filter-function>`  
filter-function一个具有以下值可选：
grayscale灰度
sepia褐色
saturate饱和度
hue-rotate色相旋转
invert反色
opacity透明度
brightness亮度
contrast对比度
blur模糊
drop-shadow阴影

[属性详解](http://www.w3cplus.com/css3/ten-effects-with-css3-filter)  

## SVG

[使用 Snap.svg 制作动画](https://aotu.io/notes/2017/01/22/snapsvg/?o2src=juejin&o2layout=compat)  

[SVG 动画精髓](https://www.villainhr.com/page/2017/05/01/SVG%20%E5%8A%A8%E7%94%BB%E7%B2%BE%E9%AB%93?utm_source=tuicool&utm_medium=referral)  

## 工具

[阴影生成器](https://www.themeshock.com/css-drop-shadow/)  

[渐变生成器](http://www.colorzilla.com/gradient-editor/)  

[渐变图案](http://lea.verou.me/css3patterns/#)  

[文字阴影](https://www.mixfont.com/shadows)  

[不规则形状生成器](http://bennettfeely.com/clippy/)  

[CSS滤镜](https://www.cssfilters.co/)  

[加载动画制作](https://loading.io/)  

[JS动画库](https://www.uisdc.com/75-web-animation-tools-1)  
