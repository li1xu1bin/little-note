

## 圆角
### border-radius

## 阴影
box-shadow


## 渐变

## 动画

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
 
