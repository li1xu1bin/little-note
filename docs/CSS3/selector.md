

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