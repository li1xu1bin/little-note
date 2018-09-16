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

## 定位position
position 用于网页元素的定位
1. `static` 默认  
2. `relative` 相对定位，会导致自身位置的相对变化，而不会影响其他元素的位置
3. `absolute` 绝对定位，相对于最近的已定位祖先元素，若无，则相对于最初的包含块
4. `fixed` 相对于窗口定位
5. `sticky` 粘性定位