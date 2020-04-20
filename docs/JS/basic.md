
## JS数据类型
1. `Boolean` 布尔值
2. `String` 字符串
3. `Number` 数字
4. `Null` 空
5. `Undefined` 未定义
6. `Object` 引用类型
7. `Symbol` ES6新增

**原始类型 Boolean、String、Number、Undefined、Null**   
**引用类型 Object 包括 Date、Array、Function**     
**原始类型 Symbol 表示独一无二的值**

## null 和 undefined的区别
* null 转为数字类型值为 0
* undefined 转为数字类型值为 NaN(Not a Number)
* null 空的对象 访问一个尚未存在的对象时所返回的值
* undefined 空的变量 访问一个未初始化的变量时返回的值


## typeof 和 instanceof的区别
1、**typeof**  

typeof 用于判断数据类型，返回值为6个字符串，分别为string、Boolean、number、function、object、undefined  
不能将Object、Array和Null区分，都返回object

  ```js  
console.log(typeof 1);               // number
console.log(typeof true);            // boolean
console.log(typeof 'mc');            // string
console.log(typeof function(){});    // function
console.log(typeof []);              // object 
console.log(typeof {});              // object
console.log(typeof null);            // object
console.log(typeof undefined);       // undefined
  ```

2、**instanceof**  
instanceof 用于判断一个变量是否某个对象的实例，返回值是布尔值  
可以区分array，object，function，不能判断number，boolean，string

  ```js
console.log(1 instanceof Number);                    // false
console.log(true instanceof Boolean);                // false 
console.log('str' instanceof String);                // false  
console.log([] instanceof Array);                    // true
console.log(function(){} instanceof Function);       // true
console.log({} instanceof Object);                   // true
  ```

## 遍历数组

1.for循环  
2.forEach循环  
3.for of循环  
4.map循环 返回处理后的数组 不改变原数组  
5.filter循环 返回符合条件的所有元素的值 不改变原数组  
6.reduce累加  
7.find 返回符合条件的第一个元素的值，无则返回undefined 不改变原数组  
8.findIndex 返回符合条件的第一个元素位置，无则返回-1 不改变原数组 

## 操作数组

push()添加最后一项  
pop()移除最后一项

shift()删除第一项  
unshift()添加到第一项

sort()排序  
reverse()反转顺序

slice(起始位置，结束位置) 返回一个新数组

splice(位置，数量，项目) 改变原数组

index:数组开始下标
len: 替换/删除的长度
item:替换的值，删除操作的话 item为空