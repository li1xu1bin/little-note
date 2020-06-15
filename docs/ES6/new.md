# ES6常用新特性

## let && const 
let 用于变量声明，但是作用域为局部  
let 没有变量提升与暂时性死区，不能重复声明

const 用于声明一个常量，设定后值不会再改变  
  
```js
let arr = [];
for(let i=0;i<2;i++){ // i虽然在全局作用域声明，但是在for循环体局部作用域中使用的时候，变量会被固定，不受外界干扰。
    arr[i] = function(){
        console.log(i); // i 是循环体内局部作用域，不受外界影响。
    }
}
arr[0](); //0
arr[1](); //1

//ES5
var arr = [];
for(var i=0;i<2;i++){
    arr[i] = function(){
        console.log(i); // 执行此代码时，同步代码for循环已经执行完成
    }
}
arr[0](); //2
arr[1](); //2
```

## 箭头函数
不需要 function 关键字来创建函数  
省略 return 关键字  
继承当前上下文的 this 关键字  

 ```js
// JS 普通函数
[1, 2, 3].map(function (item) {
    return item + 1
})

// ES6 箭头函数
[1,2,3].map(x => x + 1)

  ```
箭头函数存在的意义，第一写起来更加简洁，第二可以解决 ES6 之前函数执行中this是全局变量的问题（没有独立的作用域），看如下代码  
  ```js
function fn() {
    console.log('real', this)  // {a: 100} ，该作用域下的 this 的真实的值
    var arr = [1, 2, 3]
    // 普通 JS
    arr.map(function (item) {
        console.log('js', this)  // window 。普通函数，这里打印出来的是全局变量，令人费解
        return item + 1
    })
    // 箭头函数
    arr.map(item => {
        console.log('es6', this)  // {a: 100} 。箭头函数，这里打印的就是父作用域的 this
        return item + 1
    })
}
fn.call({a: 100})
  
  ```

## 模板字符串

1.字符串格式化
```js
//ES5 
var name = 'lux'
console.log('hello' + name)
//es6
const name = 'lux'
console.log(`hello ${name}`) //hello lux
```

2.在ES5时我们通过反斜杠(\)来做多行字符串或者字符串一行行拼接。ES6反引号(``)直接搞定
```js
// ES5
var msg = "Hi \
man!
"
// ES6
const template = `<div>
    <span>hello world</span>
</div>`
```

3.高级用法
```js
// 1.includes：判断是否包含然后直接返回布尔值
const str = 'hahay'
console.log(str.includes('y')) // true

// 2.repeat: 获取字符串重复n次
const str = 'he'
console.log(str.repeat(3)) // 'hehehe'
//如果你带入小数, Math.floor(num) 来处理
// s.repeat(3.1) 或者 s.repeat(3.9) 都当做成 s.repeat(3) 来处理

// 3. startsWith 和 endsWith 判断是否以 给定文本 开始或者结束
const str =  'hello world!'
console.log(str.startsWith('hello')) // true
console.log(str.endsWith('!')) // true

// 4. padStart 和 padEnd 填充字符串，应用场景：时分秒
setInterval(() => {
    const now = new Date()
    const hours = now.getHours().toString()
    const minutes = now.getMinutes().toString()
    const seconds = now.getSeconds().toString()
    console.log(`${hours.padStart(2, 0)}:${minutes.padStart(2, 0)}:${seconds.padStart(2, 0)}`)
}, 1000)
```

## 解构赋值
 ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）  
   ```js
let [a, b, c] = [1, 2, 3];
//等同于
let a = 1;
let b = 2;
let c = 3;

  ```
 对象的解构赋值：获取对象的多个属性并且使用一条语句将它们赋给多个变量
   ```js
 var {
  StyleSheet,
  Text,
  View
} = React;

等同于
var StyleSheet = React.StyleSheet;
var Text = React.Text;
var View = React.Text;
    ```
	
## 类class

class 其实一直是 JS 的关键字（保留字），但是一直没有正式使用，直到 ES6 。 ES6 的 class 就是取代之前构造函数初始化对象的形式，从语法上更加符合面向对象的写法。例如：  

JS 构造函数的写法  

  ```js
function MathHandle(x, y) {
  this.x = x;
  this.y = y;
}

MathHandle.prototype.add = function () {
  return this.x + this.y;
};

var m = new MathHandle(1, 2);
console.log(m.add())
  ```

用 ES6 class 的写法  
  ```js
class MathHandle {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  add() {
    return this.x + this.y;
  }
}
const m = new MathHandle(1, 2);
console.log(m.add())

  ```
  
注意以下几点，全都是关于 class 语法的：


    1、class 是一种新的语法形式，是class Name {...}这种形式，和函数的写法完全不一样
    2、两者对比，构造函数函数体的内容要放在 class 中的constructor函数中，constructor即构造器，初始化实例时默认执行
    3、class 中函数的写法是add() {...}这种形式，并没有function关键字

	
使用 class 来实现继承就更加简单了，至少比构造函数实现继承简单很多。看下面例子  
JS 构造函数实现继承  
  ```js
// 动物
function Animal() {
    this.eat = function () {
        console.log('animal eat')
    }
}
// 狗
function Dog() {
    this.bark = function () {
        console.log('dog bark')
    }
}
Dog.prototype = new Animal()
// 哈士奇
var hashiqi = new Dog()
  ```
ES6 class 实现继承
  ```js
class Animal {
    constructor(name) {
        this.name = name
    }
    eat() {
        console.log(`${this.name} eat`)
    }
}

class Dog extends Animal {
    constructor(name) {
        super(name)
        this.name = name
    }
    say() {
        console.log(`${this.name} say`)
    }
}
const dog = new Dog('哈士奇')
dog.say()
dog.eat()
  ```
注意以下两点：  

    1、使用extends即可实现继承，更加符合经典面向对象语言的写法，如 Java
    2、子类的constructor一定要执行super()，以调用父类的constructor

	
	


## 展开运算符
组装对象或者数组

```js
//数组
const color = ['red', 'yellow']
const colorful = [...color, 'green', 'pink']
console.log(colorful) //[red, yellow, green, pink]

//对象
const alp = { fist: 'a', second: 'b'}
const alphabets = { ...alp, third: 'c' }
console.log(alphabets) //{ "fist": "a", "second": "b", "third": "c"

const first = {
    a: 1,
    b: 2,
    c: 6,
}
const second = {
    c: 3,
    d: 4
}
const total = { ...first, ...second }
console.log(total) // { a: 1, b: 2, c: 3, d: 4 }

```

获取特定项
```js
//数组
const number = [1,2,3,4,5]
const [first, ...rest] = number
console.log(rest) //2,3,4,5
//对象
const user = {
    username: 'lux',
    gender: 'female',
    age: 19,
    address: 'peking'
}
const { username, ...rest } = user
console.log(rest) //{"address": "peking", "age": 19, "gender": "female"
```
## Promise异步编程
Promise 对象是 JavaScript 的异步操作解决方案，为异步操作提供统一接口。它起到代理作用（proxy），充当异步操作与回调函数之间的中介，使得异步操作具备同步操作的接口。Promise 可以让异步操作写起来，就像在写同步操作的流程，而不必一层层地嵌套回调函数。

三个状态：pending（进行中）、fulfilled（已完成）、rejected（已拒绝）
 
 两个过程：
    1、pending→fulfilled（resolve）
    2、pending→rejected（reject）
 
  ```js  
new Promise((resolve,reject)=>{
   $.ajax({
      url : "xxxxx",
	  type : "post"
	  success(res){
	    resolve(res)
	  },
	  error(err){
	    reject（err）
	  }
   });
}).then(()=>{
},()=>{
})
  ```
  ### 执行顺序
  ```js
  console.log('E');

  setTimeout(function(){
    console.log('D')
  },0)

  var promise = new Promise(function(resolve, reject){
      console.log('A')
      resolve()
  }).then(function(){
      console.log('C')
  })
  console.log('B');
  //  E
  //  A
  //  B
  //  C
  //  D
  ```
setTimeout是宏任务【macrotask】  
Promise整体是微任务【microtask】
先执行主线程，然后微任务，宏任务


## 模块化 import 和 export
import导入模块  
export导出模块  
如果只是输出一个唯一的对象，使用export default即可，代码如下  
  ```js
// 创建 util1.js 文件，内容如
export default {
    a: 100
}

// 创建 index.js 文件，内容如
import obj from './util1.js'
console.log(obj)
    ```
如果想要输出许多个对象，就不能用`default`了，且`import`时候要加`{...}`，代码如下  
  ```js
// 创建 util2.js 文件，内容如
export function fn1() {
    alert('fn1')
}
export function fn2() {
    alert('fn2')
}

// 创建 index.js 文件，内容如
import { fn1, fn2 } from './util2.js'
fn1()
fn2()
    ```
    

```js
//全部导入
import people from './example'

//有一种特殊情况，即允许你将整个模块当作单一对象进行导入
//该模块的所有导出都会作为对象的属性存在
import * as example from "./example.js"
console.log(example.name)
console.log(example.age)
console.log(example.getName())

//导入部分
import {name, age} from './example'

// 导出默认, 有且只有一个默认
export default App

// 部分导出
export class App extend Component {};
```

1.当用export default people导出时，就用 import people 导入（不带大括号）

2.当用export name 时，就用import { name }导入（记得带上大括号）

3.一个文件里，有且只能有一个export default。但可以有多个export。

## Set 和 Map 数据结构

1、Set 类似于数组，但数组可以允许元素重复，Set 不允许元素重复  
2、Map 类似于对象，但普通对象的 key 必须是字符串或者数字，而 Map 的 key 可以是任何数据类型

	
####  Set

Set 实例不允许元素有重复，可以通过以下示例证明。可以通过一个数组初始化一个 Set 实例，或者通过add添加元素，元素不能重复，重复的会被忽略。


```js
// 例1
const set = new Set([1, 2, 3, 4, 4]);
console.log(set) // Set(4) {1, 2, 3, 4}

// 例2
const set = new Set();
[2, 3, 5, 4, 5, 8, 8].forEach(item => set.add(item));
for (let item of set) {
  console.log(item);
}
// 2 3 5 4 8
  ```
Set 实例的属性和方法有 

1、size：获取元素数量。  
2、add(value)：添加元素，返回 Set 实例本身。  
3、delete(value)：删除元素，返回一个布尔值，表示删除是否成功。  
4、has(value)：返回一个布尔值，表示该值是否是 Set 实例的元素。   
5、clear()：清除所有元素，没有返回值。

  ```js
const s = new Set();
s.add(1).add(2).add(2); // 添加元素

s.size // 2

s.has(1) // true
s.has(2) // true
s.has(3) // false

s.delete(2);
s.has(2) // false

s.clear();
console.log(s);  // Set(0) {}
  ```
Set 实例的遍历，可使用如下方法
 
1、keys()：返回键名的遍历器。  
2、values()：返回键值的遍历器。不过由于 Set 结构没有键名，只有键值（或者说键名和键值是同一个值），所以keys()和values()返回结果一致。  
3、entries()：返回键值对的遍历器。  
4、forEach()：使用回调函数遍历每个成员
	
  ```js	
let set = new Set(['aaa', 'bbb', 'ccc']);

for (let item of set.keys()) {
  console.log(item);
}
// aaa
// bbb
// ccc

for (let item of set.values()) {
  console.log(item);
}
// aaa
// bbb
// ccc

for (let item of set.entries()) {
  console.log(item);
}
// ["aaa", "aaa"]
// ["bbb", "bbb"]
// ["ccc", "ccc"]

set.forEach((value, key) => console.log(key + ' : ' + value))
// aaa : aaa
// bbb : bbb
// ccc : ccc
   ```
####  Map
 
 Map 的用法和普通对象基本一致，先看一下它能用非字符串或者数字作为 key 的特性。  
   ```js	
const map = new Map();
const obj = {p: 'Hello World'};

map.set(obj, 'OK')
map.get(obj) // "OK"

map.has(obj) // true
map.delete(obj) // true
map.has(obj) // false
    ``` 

Map 实例的属性和方法如下：

1、size：获取成员的数量  
2、set：设置成员 key 和 value  
3、get：获取成员属性值  
4、has：判断成员是否存在  
5、delete：删除成员  
6、clear：清空所有  
	
 ```
const map = new Map();
map.set('aaa', 100);
map.set('bbb', 200);

map.size // 2

map.get('aaa') // 100

map.has('aaa') // true

map.delete('aaa')
map.has('aaa') // false

map.clear()
    ```
Map 实例的遍历方法有：

1、keys()：返回键名的遍历器。  
2、values()：返回键值的遍历器。  
3、entries()：返回所有成员的遍历器。  
4、forEach()：遍历 Map 的所有成员
	
  ```js 
const map = new Map();
map.set('aaa', 100);
map.set('bbb', 200);

for (let key of map.keys()) {
  console.log(key);
}
// "aaa"
// "bbb"

for (let value of map.values()) {
  console.log(value);
}
// 100
// 200

for (let item of map.entries()) {
  console.log(item[0], item[1]);
}
// aaa 100
// bbb 200

// 或者
for (let [key, value] of map.entries()) {
  console.log(key, value);
}
// aaa 100
// bbb 200
```

```js
let map = new Map([
  [1, 'one'],
  [2, 'two'],
  [3, 'three'],
]);

[...map.keys()]
// [1, 2, 3]

[...map.values()]
// ['one', 'two', 'three']

[...map.entries()]
// [[1,'one'], [2, 'two'], [3, 'three']]

[...map]
// [[1,'one'], [2, 'two'], [3, 'three']]

    ```





