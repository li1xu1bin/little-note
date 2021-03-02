
## 水平居中
1. inline元素，设置`text-align: center;`
2. 固定宽度块状元素，设置左右`margin:auto`
3. 不定宽度块状元素，设置`display:inine`
	
## 垂直居中
1. 父元素高度确定的inline元素（单行文本），设置`line-height`等于height
2. 父元素高度确定的inline元素（多行文本），先设置`display:table-cell`，再设置`vertical-align:middle`
 
## 完全居中

### 负margin法
* 定高，定宽
* position:absolute（fixed）
* margin-left 是 width的一半 的负值

```css
.box {
  position: absolute;/*或fixed*/
  width: 200px;
  height: 200px;
  background: red;
  top: 50%;
  left: 50%;
  margin-top: -100px;
  margin-left: -100px;
}
```

### transform法
* 不定高，不定宽
* transform:translate(x,y)

```css
.box {
  position: absolute;
  top:50%;
  left:50%;
  transform: translate(-50%,-50%);
}
```

### 绝对居中法
* 不定高，不定宽
* margin:auto

```css
.box {
  position: absolute;/*或fixed*/
  width: 100px;
  height: 100px;
  background: red;
  top:0;
  right:0;
  bottom:0;
  left:0;
  margin: auto;
}
```
### flexbox法
* 不定高，不定宽

```css
.box {
  display:flex;
  justify-content:center;
  align-items:center;
}
```

### 伪类法
* 定高，不定宽
* 使用before，after伪元素

```css
.box{
    display: block;
    height: 100px;
}
.content::before{
    content: '';
    display: block;
    vertical-align: middle;
    height: 100%;
}
.content::after{
    content: '';
    display: block;
    vertical-align: middle;
    height: 100%;
}
.box .content{
    height: 33px;
    line-height: 33px;
    text-align: center;
}
```


## Flexbox布局

[我的笔记](http://note.youdao.com/noteshare?id=e2e70277f9465af4ae1c2fa2404bb336)

[Flex 布局语法教程](http://www.runoob.com/w3cnote/flex-grammar.html)

[Flexbox 池塘 可视化学习](http://flexboxfroggy.com/)

[Flexbox 快速上手记](http://www.shejidaren.com/css3-flexbox-quick-learning.html)

[我们就来谈谈Flexbox布局](http://tgideas.qq.com/webplat/info/news_version3/804/7104/7106/m5723/201603/443362.shtml)

## Grid布局

[我的笔记](https://note.youdao.com/share/?id=64a62390087f3cf8c8725f80fb5b2504&type=note#/)

[Grid 布局指南](https://www.jianshu.com/p/d183265a8dad)

[Grid 布局调试器](https://alialaa.github.io/css-grid-cheat-sheet/)

[Grid 花园 可视化学习](http://cssgridgarden.com/)

[渐进增强的 CSS 布局](https://juejin.im/post/5987acfd6fb9a03c502288f3)

[Grid 构建圣杯布局](https://www.w3cplus.com/css3/holy-grail-layout-css-grid.html)

## 双飞翼布局
三栏布局，左右固定，中间自适应

实现：
left、center、right三种都设置左浮动  
设置center宽度为100%（中间外包一层）  
left设置负边距为100%，right设置负边距为自身宽度    
内部content的margin值为左右两个侧栏留出空间，margin值大小为left和right宽度

```
  #left, #right, #center {
    float: left;
  }
  #center {
    width: 100%;
    background: rgb(206, 201, 201);
  }
  #left {
    width: 200px;
    margin-left: -100%;
    background: rgba(95, 179, 235, 0.972);
  }
  #right {
    width: 150px;
    margin-left: -150px;
    background: rgb(231, 105, 2);
  }
  .content {
    margin: 0 150px 0 200px;
  }

 <div id="container">
    <div id="center" class="column">
      <div class="content">#center</div>
    </div>
    <div id="left" class="column">#left</div>
    <div id="right" class="column">#right</div>
  </div>

```



[让CSS flex布局最后一行列表左对齐的N种方法](https://www.zhangxinxu.com/wordpress/2019/08/css-flex-last-align/)  


## Flex布局
flex: 1 = 0 1 auto  
flex: flex-grow flex-shrink flex-basis   
flex: 扩展 收缩 项目长度  
元素宽度等于 扩展值+项目长度
```css
  .container {
    width: 600px;
    height: 300px;
    display: flex;
  }
  .left {
    flex: 1 2 300px;
    background: red;
  }
  .right {
    flex: 2 1 200px;
    background: blue;
  }
```
剩余的空间：600 - (300 + 200) = 100。  
子元素的 flex-grow 的值分别为 1，2， 剩余空间用3等分来分  
100 / 3 = 33.3333333  
所以 left = 300 + 1 * 33.33 = 333.33  
right = 200 + 2 * 33.33 = 266.67  