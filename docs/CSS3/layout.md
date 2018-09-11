
## 水平居中
1、inline 元素用text-align: center;即可，如下：

  ```bash
.container {
   text-align: center;
}
  ```

2、固定宽度块状元素，可以设置左右margin值为auto来使用，如下：

  ```bash
.item {
    width: 1000px;
    margin: auto; 
}
  ```
3、不定宽度块状元素

  1、在元素外加入 table 标签（完整的，包括 table、tbody、tr、td），该元素写在 td 内，然后设置 margin 的值为 auto  
  2、给该元素设置 displa:inine 方法   
  3、父元素设置 position:relative 和 left:50%，子元素设置 position:relative 和 left:50%  
	
## 垂直居中
1、inline 元素可设置line-height的值等于height值，如单行文字垂直居中：

  ```bash
.css文件
.outerBox{
  width:200px;
  height:100px;
  border: 1px solid #000;
  text-align:center; /* 水平居中 */
  line-height: 100px; /* line-height=height */
}
.html文件
<div class="outerBox">
  center text 
</div>
  ```
2、vertical-align实现文本的垂直居中  
可以使用vertical-align实现垂直居中，不过vertical-align仅适用于内联元素和table-cell元素，因此之前需要转化  

  ```bash
.outerBox{
  width:200px;
  height:100px;
  border: 1px solid #000;
  text-align:center; /* 水平居中 */
  display:table-cell; /*转化成table-cell元素*/
  vertical-align:middle;  
  /*垂直居中对齐，vertical-align适用于内联及table-cell元素*/
}
  ```
3、绝对定位元素，可结合left和margin实现，但是必须知道尺寸
  ```bash
.container {
    position: relative;
    height: 200px;
}
.item {
    width: 80px;
    height: 40px;
    position: absolute;
    left: 50%;
    top: 50%;
    margin-top: -20px;
    margin-left: -40px;
}...

  ```
4、绝对定位可结合transform实现居中。
优点：不需要提前知道尺寸  
缺点：兼容性不好  
  ```bash
.container {
    position: relative;
    height: 200px;
}
.item {
    width: 80px;
    height: 40px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    background: blue;
}
  ```
5、绝对定位结合margin: auto，不需要提前知道尺寸，兼容性好


  ```bash
.container {
    position: relative;
    height: 300px;
}
.item {
    width: 100px;
    height: 50px;
    position: absolute;
    left: 0;
    top: 0;
    right: 0;
    bottom: 0;
    margin: auto;
}
  ```
