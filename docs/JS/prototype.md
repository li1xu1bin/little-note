

#### 对JavaScript原型的理解
    1、所有的引用类型（数组、对象、函数），都具有对象特性，即可自由扩展属性（null除外）
    2、所有的引用类型（数组、对象、函数），都有一个__proto__属性（隐式原型），属性值是一个普通的对象  
    3、所有的函数，都有一个prototype属性，属性值也是一个普通的对象
	  4、所有的引用类型（数组、对象、函数），__proto__属性值指向它的构造函数的prototype属性值
	
通过代码解释一下，看结果。  
  
  ```js
  // 要点一：自由扩展属性
	var obj = {}; obj.a = 100;
	var arr = []; arr.a = 100;
	function fn () {}
	fn.a = 100;

	// 要点二：__proto__
	console.log(obj.__proto__);
	console.log(arr.__proto__);
	console.log(fn.__proto__);

	// 要点三：函数有 prototype
	console.log(fn.prototype)

	// 要点四：引用类型的 __proto__ 属性值指向它的构造函数的 prototype 属性值
	console.log(obj.__proto__ === Object.prototype)

  ```
  
#### 原型
  ```js
  // 构造函数
function Foo(name, age) {
    this.name = name
}
Foo.prototype.alertName = function () {
    alert(this.name)
}
// 创建示例
var f = new Foo('zhangsan')
f.printName = function () {
    console.log(this.name)
}
// 测试
f.printName()
f.alertName()
  ```
&emsp;&emsp;执行printName时很好理解，但是执行alertName时发生了什么？这里再记住一个重点 当试图得到一个对象的某个属性时，如果这个对象本身没有这个属性，那么会去它的`__proto__`（即它的构造函数的prototype）中寻找，因此f.alertName就会找到Foo.prototype.alertName  
&emsp;&emsp;那么如何判断这个属性是不是对象本身的属性呢？使用hasOwnProperty，常用的地方是遍历一个对象的时候.
  ```
var item
for (item in f) {
    // 高级浏览器已经在 for in 中屏蔽了来自原型的属性，但是这里建议大家还是加上这个判断，保证程序的健壮性
    if (f.hasOwnProperty(item)) {
        console.log(item)
    }
}
  ```
#### 原型链
  ```
f.printName()
f.alertName()
f.toString()
  ```
&emsp;&emsp;因为f本身没有toString()，并且`f.__proto__`（即Foo.prototype）中也没有toString。这个问题还是得拿出刚才那句话——当试图得到一个对象的某个属性时，如果这个对象本身没有这个属性，那么会去它的`__proto__`（即它的构造函数的prototype）中寻找。  
&emsp;&emsp;如果在`f.__proto__`中没有找到toString，那么就继续去`f.__proto__.__proto__`中寻找，因为`f.__proto__`就是一个普通的对象而已嘛！

    1、f.__proto__即Foo.prototype，没有找到toString，继续往上找
    2、f.__proto__.__proto__即Foo.prototype.__proto__。Foo.prototype就是一个普通的对象，因此Foo.prototype.__proto__就是Object.prototype，在这里可以找到toString
    3、因此f.toString最终对应到了Object.prototype.toString
	
&emsp;&emsp;这样一直往上找，你会发现是一个链式的结构，所以叫做“原型链”。如果一直找到最上层都没有找到，那么就宣告失败，返回undefined。最上层是什么 —— `Object.prototype.__proto__ === null`  

#### 原型链中的this
&emsp;&emsp;所有从原型或更高级原型中得到、执行的方法，其中的this在执行时，就指向了当前这个触发事件执行的对象。因此printName和alertName中的this都是f。  

#### 继承（本质为原型）
既然要实现继承，那么首先我们得有一个父类
  ```
// 定义一个动物类
function Animal (name) {
  // 属性
  this.name = name || 'Animal';
  // 实例方法
  this.sleep = function(){
    console.log(this.name + '正在睡觉！');
  }
}
// 原型方法
Animal.prototype.eat = function(food) {
  console.log(this.name + '正在吃：' + food);
};
  ```
#### 继承的实现方式：  
#####  1、原型链继承
核心： 将父类的实例作为子类的原型  
  ```
function Cat(){ 
 }
Cat.prototype = new Animal();
Cat.prototype.name = 'cat';

//　Test Code
var cat = new Cat();
console.log(cat.name);
console.log(cat.eat('fish'));
console.log(cat.sleep());
console.log(cat instanceof Animal); //true 
console.log(cat instanceof Cat); //true
  ```
特点： 
 
     1、非常纯粹的继承关系，实例是子类的实例，也是父类的实例
     2、父类新增原型方法/原型属性，子类都能访问到
     3、简单，易于实现
	 
缺点：
 
     1、要想为子类新增属性和方法，必须要在new Animal()这样的语句之后执行，不能放到构造器中
     2、无法实现多继承
     3、来自原型对象的引用属性是所有实例共享的（详细请看附录代码： 示例1）
     4、创建子类实例时，无法向父类构造函数传参
#####  2、构造继承
 核心：使用父类的构造函数来增强子类实例，等于是复制父类的实例属性给子类（没用到原型）  
 ```
 function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}

// Test Code
var cat = new Cat();
console.log(cat.name);
console.log(cat.sleep());
console.log(cat instanceof Animal); // false
console.log(cat instanceof Cat); // true
  ```
特点：

     1、解决了1中，子类实例共享父类引用属性的问题
     2、创建子类实例时，可以向父类传递参数
     3、可以实现多继承（call多个父类对象）
缺点：

     1、实例并不是父类的实例，只是子类的实例
     2、只能继承父类的实例属性和方法，不能继承原型属性/方法
     3、无法实现函数复用，每个子类都有父类实例函数的副本，影响性能
	 
#####  3、实例继承
核心：为父类实例添加新特性，作为子类实例返回
  ```
function Cat(name){
  var instance = new Animal();
  instance.name = name || 'Tom';
  return instance;
}

// Test Code
var cat = new Cat();
console.log(cat.name);
console.log(cat.sleep());
console.log(cat instanceof Animal); // true
console.log(cat instanceof Cat); // false
  ```
 特点：

    1、不限制调用方式，不管是new 子类()还是子类(),返回的对象具有相同的效果

缺点：

    1、实例是父类的实例，不是子类的实例
    2、不支持多继承


#####  4、拷贝继承
  ```
function Cat(name){
  var animal = new Animal();
  for(var p in animal){
    Cat.prototype[p] = animal[p];
  }
  Cat.prototype.name = name || 'Tom';
}

// Test Code
var cat = new Cat();
console.log(cat.name);
console.log(cat.sleep());
console.log(cat instanceof Animal); // false
console.log(cat instanceof Cat); // true
  ```
特点：支持多继承  
缺点：

    1、效率较低，内存占用高（因为要拷贝父类的属性）
    2、无法获取父类不可枚举的方法（不可枚举方法，不能使用for in 访问到）

#####  5、组合继承
核心：通过调用父类构造，继承父类的属性并保留传参的优点，然后通过将父类实例作为子类原型，实现函数复用
  ```
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}
Cat.prototype = new Animal();

// 感谢 @学无止境c 的提醒，组合继承也是需要修复构造函数指向的。

Cat.prototype.constructor = Cat;

// Test Code
var cat = new Cat();
console.log(cat.name);
console.log(cat.sleep());
console.log(cat instanceof Animal); // true
console.log(cat instanceof Cat); // true
  ```
特点：

    1、弥补了方式2的缺陷，可以继承实例属性/方法，也可以继承原型属性/方法
    2、既是子类的实例，也是父类的实例
    3、不存在引用属性共享问题
    4、可传参
    6、函数可复用
缺点：调用了两次父类构造函数，生成了两份实例（子类实例将子类原型上的那份屏蔽了）  
推荐指数：★★★★（仅仅多消耗了一点内存）

#####  6、寄生组合继承
核心：通过寄生方式，砍掉父类的实例属性，这样，在调用两次父类的构造的时候，就不会初始化两次实例方法/属性，避免的组合继承的缺点
  ```
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}
(function(){
  // 创建一个没有实例方法的类
  var Super = function(){};
  Super.prototype = Animal.prototype;
  //将实例作为子类的原型
  Cat.prototype = new Super();
})();

// Test Code
var cat = new Cat();
console.log(cat.name);
console.log(cat.sleep());
console.log(cat instanceof Animal); // true
console.log(cat instanceof Cat); //true

Cat.prototype.constructor = Cat; // 需要修复下构造函数
  ```
特点：堪称完美
缺点：实现较为复杂

