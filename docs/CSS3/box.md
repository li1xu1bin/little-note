## 定位position
position 用于网页元素的定位，可设置 `static/relative/absolute/fixed /inherit ` 

  1、static 默认  
  2、relative，会导致自身位置的相对变化，而不会影响其他元素的位置、大小  
  3、absolute 元素脱离了文档结构，导致父元素坍塌，元素具有“包裹性，元素会悬浮在页面上方，会遮挡住下方的页面内容，设置了 top、left 值时，   元素是相对于最近的定位上下文来定位的，而不是相对于浏览器定位。  
  4、fixed 元素脱离了文档结构，根据 window （或者 iframe）确定位置