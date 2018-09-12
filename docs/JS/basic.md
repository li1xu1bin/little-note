
## ECMAScript 6种原始类型
    1、 Boolean 布尔
    2、 String 
    3、 Number
    4、 Null
    5、 Undefined
    6、 Symbol （es6）
	
## 数据类型分为：值类型和引用类型   
    1、值类型变量包括 Boolean、String、Number、Undefined、Null  
    2、引用类型包括了 Object 类的所有，如 Date、Array、Function   

## 类型判断的方法
1、**typeof**  

typeof只有区分值类型的详细类型

  ```js  
var x = 1;
console.log(typeof x);//number
 
var a = undefined;
console.log(typeof a);//undefined
 
var b = null;
console.log(typeof b);//object，（null是空对象引用/或者说指针）。
 
var c = new Object();
console.log(typeof c);//object
 
var e = [1,2,3];
console.log(typeof e);//object 
 
var d = function(){
 // ... 语句块
}
console.log(typeof d);//function
  ```


2、**instanceof**  
用于实例和构造函数的对应。例如判断一个变量是否是数组，使用typeof无法判断，但可以使用[1, 2] instanceof Array来判断。因为，[1, 2]是数组，它的构造函数就是Array。同理：  
  ```js
function Foo(name) {
    this.name = name
   }
var foo = new Foo('bar')
console.log(foo instanceof Foo) // true
  ```

## 变量计算
    1、字符串拼接
    2、 “==”运算
    3、 if语句
    4、 逻辑运算符（&&与 ||或 ！非）


## 对象

#### 对象的定义
&emsp;&emsp;对象是JavaScript的一个基本数据类型，是一种复合值，它将很多值（原始值或者其他对象）聚合在一起，可通过名字访问这些值。即属性的无序集合  

#### 对象的创建（多种方法）
1、对象直接量 / 字面量  2、构造函数 　3、Object.create(原型)
##### 1、对象直接量 / 字面量
  ```js
 var obj = {
   name: 'lyl',
   age: 18
}
console.log(obj.name); // lyl
  ```
  
##### 2、构造函数
（1）、系统自带的的， eg： new Object(), Array(), Number(),Boolean(), Date()...
  ```js
 var obj = new Object();
      obj.name = 'lyl';
      console.log(obj.name); //lyl
  ```
（2）、自定义的：为了和普通函数区分，首字母大写，采用大驼峰式写法（普通函数采用小驼峰式写法）
  ```js
       function Obj (name) {
          this.name = name;
          this.age = 18;
      }
      var obj = new Obj('lyl');
      console.log(obj.name); //lyl
      console.log(obj.age); //18
  ``` 
##### 3、Object.create(原型)； 创建一个继承该原型的实例对象
  
## 作用域和闭包(执行上下文、this)

#### 题目：现在有个 HTML 片段，要求编写代码，点击编号为几的链接就alert弹出其编号
  
  ```
<ul>
    <li>编号1，点击我请弹出1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
</ul>
  ```
一般不知道这个题目用闭包的话，会写出下面的代码：  
  ```js
var list = document.getElementsByTagName('li');
for (var i = 0; i < list.length; i++) {
    list[i].addEventListener('click', function(){
        alert(i + 1)
    }, true)
}
  ```
 实际上执行才会发现始终弹出的是6，这时候就应该通过闭包来解决:  
  ```js
var list = document.getElementsByTagName('li');
for (var i = 0; i < list.length; i++) {
    list[i].addEventListener('click', function(i){
        return function(){
            alert(i + 1)
        }
    }(i), true)
}
  ```
要理解闭包，就需要我们从「执行上下文」开始讲起。

#### 执行上下文
  ```js
 console.log(a)  // undefined
var a = 100

fn('zhangsan')  // 'zhangsan' 20
function fn(name) {
    age = 20
    console.log(name, age)
    var age
}

console.log(b); // 这里报错
// Uncaught ReferenceError: b is not defined
b = 100;
  ```
&emsp;&emsp;在一段 JS 脚本（即一个`<script>`标签中）执行之前，要先解析代码（所以说 JS 是解释执行的脚本语言），解析的时候会先创建一个 全局执行上下文 环境，先把代码中即将执行的（内部函数的不算，因为你不知道函数何时执行）变量、函数声明都拿出来。变量先暂时赋值为undefined，函数则先声明好可使用。这一步做完了，然后再开始正式执行程序。再次强调，这是在代码执行之前才开始的工作。  

&emsp;&emsp;我们来看下上面的面试小题目，为什么a是undefined，而b却报错了，实际 JS 在代码执行之前，要「全文解析」，发现var a，知道有个a的变量，存入了执行上下文，而b没有找到var关键字，这时候没有在执行上下文提前「占位」，所以代码执行的时候，提前报到的a是有记录的，只不过值暂时还没有赋值，即为undefined，而b在执行上下文没有找到，自然会报错（没有找到b的引用）。  

&emsp;&emsp;另外，一个函数在执行之前，也会创建一个 函数执行上下文 环境，跟 全局上下文 差不多，不过 函数执行上下文 中会多出this arguments和函数的参数。参数和arguments好理解，这里的this咱们需要专门讲解  

 总结一下： 
 
    1、范围：一段<script>、js 文件或者一个函数
    2、全局上下文：变量定义，函数声明
    3、函数上下文：变量定义，函数声明，this，arguments
  
#### this
  
&emsp;&emsp;先搞明白一个很重要的概念 —— this的值是在执行的时候才能确认，定义的时候不能确认！ 为什么呢 —— 因为this是执行上下文环境的一部分，而执行上下文需要在代码执行之前确定，而不是定义的时候。看如下例子  
  ```js  
  var a = {
    name: 'A',
    fn: function () {
        console.log(this.name)
    }
}
a.fn()  // this === a
a.fn.call({name: 'B'})  // this === {name: 'B'}
var fn1 = a.fn
fn1()  // this === window
    ```
  this执行会有不同，主要集中在这几个场景中  
  
    1、作为构造函数执行，构造函数中
    2、作为对象属性执行，上述代码中a.fn()
    3、作为普通函数执行，上述代码中fn1()
    4、用于call apply bind，上述代码中a.fn.call({name: 'B'})
  
#### 作用域
ES6 之前 JS 没有块级作用域。例如  
  ```js 
if (true) {
    var name = 'zhangsan'
}
console.log(name) 
    ```
   
从上面的例子可以体会到作用域的概念，作用域就是一个独立的地盘，让变量不会外泄、暴露出去。上面的name就被暴露出去了，因此，JS 没有块级作用域，只有全局作用域和函数作用域。
  ```js 
var a = 100
function fn() {
    var a = 200
    console.log('fn', a)
}
console.log('global', a)
fn()

    ```
全局作用域就是最外层的作用域，如果我们写了很多行 JS 代码，变量定义都没有用函数包括，那么它们就全部都在全局作用域中。这样的坏处就是很容易撞车、冲突。
  ```js 
// 张三写的代码中
var data = {a: 100}

// 李四写的代码中
var data = {x: true}
    ```
这就是为何 jQuery、Zepto 等库的源码，所有的代码都会放在(function(){....})()中。因为放在里面的所有变量，都不会被外泄和暴露，不会污染到外面，不会对其他的库或者 JS 脚本造成影响。这是函数作用域的一个体现。

附：ES6 中开始加入了块级作用域，使用let定义变量即可，如下：
  ```js 
if (true) {
    let name = 'zhangsan'
}
console.log(name)  // 报错，因为let定义的name是在if这个块级作用域
    ```
### 作用域链
首先认识一下什么叫做 自由变量 。如下代码中，console.log(a)要得到a变量，但是在当前的作用域中没有定义a（可对比一下b）。当前作用域没有定义的变量，这成为 自由变量 。自由变量如何得到 —— 向父级作用域寻找。
  ```js 
var a = 100
function fn() {
    var b = 200
    console.log(a)
    console.log(b)
}
fn()
    ```
如果父级也没呢？再一层一层向上寻找，直到找到全局作用域还是没找到，就宣布放弃。这种一层一层的关系，就是 作用域链 。
  ```js 
var a = 100
function F1() {
    var b = 200
    function F2() {
        var c = 300
        console.log(a) // 自由变量，顺作用域链向父作用域找
        console.log(b) // 自由变量，顺作用域链向父作用域找
        console.log(c) // 本作用域的变量
    }
    F2()
}
F1()
    ```
###  闭包
讲完这些内容，我们再来看一个例子，通过例子来理解闭包。
  ```js
function F1() {
    var a = 100
    return function () {
        console.log(a)
    }
}
var f1 = F1()
var a = 200
f1()
    ```
自由变量将从作用域链中去寻找，但是 依据的是函数定义时的作用域链，而不是函数执行时，以上这个例子就是闭包。闭包主要有两个应用场景：

函数作为返回值，上面的例子就是  
函数作为参数传递，看以下例子  
  ```js
function F1() {
    var a = 100
    return function () {
        console.log(a)
    }
}
function F2(f1) {
    var a = 200
    console.log(f1())
}
var f1 = F1()
F2(f1)
    ```
至此，对应着「作用域和闭包」这部分一开始的点击弹出alert的代码再看闭包，就很好理解了。


## 同步 vs 异步

### 异步
异步和同步也是面试中常考的内容，下面笔者来讲解下同步和异步的区别。

##### 同步 vs 异步
先看下面的 demo，根据程序阅读起来表达的意思，应该是先打印100，1秒钟之后打印200，最后打印300。但是实际运行根本不是那么回事。
  ```js
console.log(100)
setTimeout(function () {
    console.log(200)
}, 1000)
console.log(300)
    ```
再对比以下程序。先打印100，再弹出200（等待用户确认），最后打印300。这个运行效果就符合预期要求。
  ```js
console.log(100)
alert(200)  // 1秒钟之后点击确认
console.log(300)
    ```
这俩到底有何区别？—— 第一个示例中间的步骤根本没有阻塞接下来程序的运行，而第二个示例却阻塞了后面程序的运行。前面这种表现就叫做 异步（后面这个叫做 同步 ），即不会阻塞后面程序的运行。

#### 异步和单线程
JS 需要异步的根本原因是 JS 是单线程运行的，即在同一时间只能做一件事，不能“一心二用”。  

一个 Ajax 请求由于网络比较慢，请求需要 5 秒钟。如果是同步，这 5 秒钟页面就卡死在这里啥也干不了了。异步的话，就好很多了，5 秒等待就等待了，其他事情不耽误做，至于那5 秒钟等待是网速太慢，不是因为 JS 的原因。  
讲到单线程，我们再来看个真题： 
  ```js
 var a = true;
setTimeout(function(){
    a = false;
}, 100)
while(a){
    console.log('while执行了')
}
    ```
这是一个很有迷惑性的题目，不少候选人认为100ms之后，由于a变成了false，所以while就中止了，实际不是这样，因为JS是单线程的，所以进入while循环之后，没有「时间」（线程）去跑定时器了，所以这个代码跑起来是个死循环！  

### 前端异步的场景

    1、定时 setTimeout setInterval
    2、网络请求，如 Ajax <img>加载
Ajax 代码示例：  
  ```js
console.log('start')
$.get('./data1.json', function (data1) {
    console.log(data1)
})
console.log('end')
    ```
	
img 代码示例（常用于打点统计）  
  ```js
  
  
console.log('start')
var img = document.createElement('img')
// 或者 img = new Image()
img.onload = function () {
    console.log('loaded')
    img.onload = null
}
img.src = '/xxx.png'
console.log('end')	
  ```
  

## 如何让Nodejs支持对应的ES
  &emsp;&emsp;不同版本的Node.js对Babel有不同的支持，如若是Nodejs支持ES6语法，需要引入babel。因此要安装一些babel的依赖包，如babel-preset-es2015 / babel-core /babel-cli。  
  &emsp;&emsp;ES6对应es2015，ES7对应es2016，ES8对应es2017，同时对应支持的node版本更高  

 #### 检测ES6
 
可以使用es-checker来检测当前Node.js对ES6的支持情况，全局安装es-checker  

  ```js
$ npm install -g es-checker
  ```
  
安装好之后，执行以下命令来查看Node.js对ES6的支持情况:  
  ```js
  $ es-checker
  ```
可以从输出中查看当前版本Node.js(v7.7.4)对ES6的支持情况

 #### 添加es6支持
全局安装babel-cli， 项目安装 babel-preset-es2015: 

  ```js
npm install babel-cli -g
npm install babel-preset-es2015 --save
  ```
安装完之后，还需要添加一个名为.babelrc的配置文件。方便babel-cli使用babel-preset-es2015。文件内容如下：  
  ```js
{
  "presets": ["es2015"],
  "plugins": [
    "add-module-exports"
  ]
}
  ```
 或者在项目入口文件（如app.js）引用下babel:   
   ```js 
  require(‘babel-register‘)({
    presets: [‘es2015‘]
}); 
  ```
  

