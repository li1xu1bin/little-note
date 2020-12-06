
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
  ```js
var arr = [1,2,3]
for(var i = 0; i < arr.length; i++){
  console.log(arr[i])
}
  ```

2.forEach循环
  ```js
var arr = [1,2,3]
arr.forEach(function(item,index){
	console.log(item)
	console.log(index)
})
  ```

3.for of循环  
  ```js
var arr = [1,2,3]
for(var key of arr){
	console.log(key)
}
  ```

4.map循环 返回处理后的数组 不改变原数组  
  ```js
var arr = [
  { name:'sun' },
  { name:'moon' },
  { name:'star' }
]
// 返回值
var newArr = arr.map((item,index) =>{
	return item.name
})
console.log(newArr)
// ["sun", "moon", "star"]

// 新增属性
var newArr = arr.map((item,index) =>({
	name: item.name,
	num: item.name + index
}))
console.log(newArr)
  // [
  // { name: "sun", num: "sun0"},
  // { name: "moon", num: "moon1"},
  // { name: "star", num: "star2"},
  // ]
  ```

5.filter循环 返回符合条件的所有元素的值 不改变原数组
  ```js
var arr = [
  { name:'sun', light:true },
  { name:'moon', light:false },
  { name:'star', light:true }
]
// 返回值
var newArr = arr.filter((item,index) =>{
	return item.light
})
console.log(newArr)
  // [
  // { name:'sun', light:true },
  // { name:'star', light:true }
  // ]
  ```  

6.find 返回符合条件的第一个元素的值，无则返回undefined 不改变原数组  
7.findIndex 返回符合条件的第一个元素位置，无则返回-1 不改变原数组 
  ```js
var arr = [1,2,3]

var num = arr.find((item,index) =>{
	return item === 3;
})
var order = arr.findIndex((item,index) =>{
	return item === 3;
})
console.log(num) //3
console.log(order) //2
  ```  
8.reduce累加  

## 操作数组

push()添加最后一项  
pop()移除最后一项

shift()删除第一项  
unshift()添加到第一项

sort()排序  
reverse()反转顺序

slice(起始位置，结束位置) 返回一个新数组
  ```js
arr.slice(0);//返回一个新数组

var num = '12345';
console.log(num.slice(1))//2345
console.log(num.slice(1,3))//23

var arr = ['a','b','c','d'];
console.log(arr.slice(1))//["b", "c", "d"] 删除第一项
console.log(arr.slice(1,3))//["b", "c"]
console.log(arr.slice(0,-1))//['a', "b", "c"] 删除最后一项
  ``` 

splice(index，len，item) 改变原数组

index:数组开始下标  
len: 替换/删除的长度  
item:替换的值，删除操作的话 item为空

```js


var arr = ['a','b','c','d'];
arr.splice(-1,1) 
console.log(arr);//['a','b','c']

var arr = ['a','b','c','d'];
arr.splice(1,2,'ttt');
console.log(arr);//['a','ttt','d']
```

## 遍历对象

1.for...in  
主要用于遍历对象的可枚举属性
```js
const obj = {
    id: 1,
    name: 'zhangsan',
    age: 18
}

for (let key in obj) {
    console.log(key + '-' + obj[key])
}
//id-1
//name-zhangsan
//age-18
```

2.
Object.keys() 返回obj对象由key组成的数组  
Object.values() 返回obj对象由value组成的数组
```
const obj = {
    id: 1,
    name: 'zhangsan',
    age: 18
}

Object.keys(obj);
Object.values(obj);

//["id", "name", "age"]
//[1, "zhangsan", 18]
```

3.
Object.getOwnPropertyNames(obj)
```
const obj = {
    id: 1,
    name: 'zhangsan',
    age: 18
}

Object.getOwnPropertyNames(obj);

//["id", "name", "age"]
```