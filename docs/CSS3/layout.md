
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