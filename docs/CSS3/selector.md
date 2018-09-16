## 基本选择器
* X + Y直接兄弟选择器：在X之后**第一个兄弟节点**中选择满足Y选择器的元素，兼容性： IE7+
* X > Y子选择器： 选择X的**直接子元素**中满足Y选择器的元素，兼容性： IE7+
* X ~ Y兄弟： 选择X之后**所有兄弟节点**中满足Y选择器的元素，兼容性： IE7+

## 伪类与伪元素
伪类(单冒号)包含两种：`状态伪类`和`结构性伪类`。

`状态伪类`是基于元素当前状态进行选择的 :hover :active :focus

`结构性伪类`是过文档结构的互相关系来匹配元素 :first-child :nth-child

伪元素(双冒号)是对元素中的特定内容进行操作，而不是描述状态。 ::before ::after

## CSS选择器优先级
权重可以叠加  
权重：`!important` > 行内样式 > `id` > `class` > tag


## :focus-within
与:focus其不同之处是，只要元素的任何后代元素得到焦点，它就会被匹配  
案例：https://codepen.io/airen/pen/Owjeyz



## :placeholder-shown
表示的是占位文本符可见的状态  
可以利用占位文本符不可见的时候，显示提示信息
  ```css
.input:not(:placeholder-shown) + .prompt {
    max-height: 2em; 
}
  ```