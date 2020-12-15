## 原型
1、每个对象（除了null），都有一个_ _proto_ _属性，指向原型对象  
2、每个函数，都有一个 prototype 属性，即原型对象  
3、所有的原型对象都是 Object 的实例，所以 _proto_都指向 Object （构造函数）的原型对象。而 Object 构造函数的_ _proto_ _ 指向 null（停止查找）
	
  ```js
  function Person() {
  }
  var person = new Person();
  // 实例.proto === 构造函数.prototype
  console.log(person.__proto__ === Person.prototype); // true
  console.log(Person.prototype.__proto__ === Object.prototype); // true
  console.log(Object.prototype.__proto__ === null); // true

  ```

### 构造函数
1.构造函数习惯上首字母大写  
2.a.普通函数的调用方式：直接调用 person();  
2.b.构造函数的调用方式：需要使用new关键字来调用 new Person();  
3.内部用this 来构造属性和方法 

### constructor属性

原型对象上的一个指向构造函数的属性。

  ```js
  console.log(person.constructor === Person); // true
  console.log(person.constructor === Person.prototype.constructor); // true

  ```
## 原型链
原型链：原型链就是多个对象通过 __proto__ 的方式连接了起来。
instanceof 的原理是通过判断该对象的原型链中是否可以找到该构造类型的 prototype 类型。
 
 ```js
 var A = function(){};
 var a = new A();
 console.log(a.__proto__); //Object {}（即构造器function A 的原型对象）
 console.log(a.__proto__.__proto__); //Object {}（即构造器function Object 的原型对象）
 console.log(a.__proto__.__proto__.__proto__); //null
  ```

![pic](https://img2018.cnblogs.com/blog/850375/201907/850375-20190708153139577-2105652554.png)


## 继承（本质为原型）
既然要实现继承，那么首先我们得有一个父类
  ```js
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


### 继承的实现方式：

#####  1、原型链继承
核心： 将父类的实例作为子类的原型  
  ```js
function Cat(){ 
}
Cat.prototype = new Animal();//新实例的原型等于父类的实例
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
 ```js
function Cat(name){
  Animal.call(this);//使用call()或apply()
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
	 

#####  3、组合继承（组合原型链继承和借用构造函数继承）
核心：通过调用父类构造，继承父类的属性并保留传参的优点，然后通过将父类实例作为子类原型，实现函数复用
  ```js
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}
Cat.prototype = new Animal();

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


#####  4、实例继承
核心：为父类实例添加新特性，作为子类实例返回
  ```js
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


#####  5、拷贝继承
  ```js
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


#####  6、寄生组合继承
核心：通过寄生方式，砍掉父类的实例属性，这样，在调用两次父类的构造的时候，就不会初始化两次实例方法/属性，避免的组合继承的缺点
  ```js
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

