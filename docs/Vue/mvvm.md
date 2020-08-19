## MVC

Model模型（数据）  
View视图（用户界面）  
Controller控制器（业务逻辑）  
M > V > C > M单向通信


## MVVM
Model 数据  
ViewModel 视图模型，连接Model和View，整合Observer、Compile和Watcher  
View 视图

V<->VM>M>VM  
V和VM双向绑定  
关注model变化，通过VM自动去更新Dom状态，开发者不用做繁琐的dom操作

vue响应式原理：Object.defineProperty 将data转为getter/setter

1.实现一个数据劫持 - Observer，能够对数据对象的所有属性进行监听，如有变动可拿到最新值并通知订阅者  
2.实现一个模板编译 - Compiler，对每个元素节点的指令进行扫描和解析，根据指令模板替换数据，以及绑定相应的更新函数  
3.实现一个 - Watcher，作为连接Observer和Compile的桥梁，能够订阅并收到每个属性变动的通知，执行指令绑定的相应回调函数，从而更新视图  
4.MVVM 作为入口函数，整合以上三者

[实现自己的 MVVM](https://juejin.im/post/5e1b3144f265da3e4b5be2e3#heading-1)

### MVVM模式的优点:

低耦合:View可以独立于Model变化和修改,一个ViewModel可以绑定到不同的View上,当View变化的时候Model可以不变,当Model变化的时候View也可以不变  
可重用性: 可以把一些视图逻辑放在一个ViewModel里面,让很多View重用这段视图逻辑  
独立开发: 开发人员可以专注于业务逻辑和数据的开发,设计人员可以专注于页面的设计


### MVVM和MVC的区别:

MVC中Controller演变成MVVM中的ViewModel  
MVVM通过数据来显示视图层而不是节点操作  
MVVM主要解决了MVC中大量的dom操作使页面渲染性能降低,加载速度变慢,影响用户体验


## 生命周期 
Vue实例从开始创建、初始化数据、编译模板、挂载DOM、渲染、更新、渲染、卸载等一系列过程，称为Vue的生命周期

![pic](https://cn.vuejs.org/images/lifecycle.png)

beforeCreate：实例初始化之后，this指向创建实例，不能访问到data、computed、watch、method上订单方法和数据  
created：实例创建完成，可访问data、computed、watch、method上的方法和数据，未挂载到DOM，不能访问到$el属性，$ref属性内容为空数组 

beforeMount：在挂载开始之前被调用，beforeMount之前，会找到对应的template，并编译成render函数  
mounted：实例挂载到DOM上，此时可以通过DOMAPi获取到DOM节点，$ref属性可以访问  

beforeUpdate：响应式数据更新时调用，发生在虚拟DOM打补丁之前  
updated：虚拟DOM重新渲染和打补丁之后调用，组件DOM已经更新，可执行依赖于DOM的操作  

activated：keep-alive开启时调用  
deactivated：keep-alive关闭时调用 

beforeDestroy：实例销毁之前调用。实例仍然完全可用，this仍能获取到实例  
destroy：实例销毁后调用，调用后，Vue实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁

### vue父子组件的渲染顺序

1.加载渲染过程
父beforeCreate->父created->父beforeMount->子beforeCreate->子created->子beforeMount->子mounted->父mounted

2.子组件更新过程
父beforeUpdate->子beforeUpdate->子updated->父updated

3.父组件更新过程
父beforeUpdate->父updated

4.销毁过程
父beforeDestroy->子beforeDestroy->子destroyed->父destroyed

## 组件通信

### 1.父组件向子组件传值

父组件通过props向下传递数据给子组件  
父data -> 子props

```js
//App.vue父组件
<template>
  <div id="app">
    <users :users="users"></users>//前者自定义名称便于子组件调用，后者要传递数据名
  </div>
</template>
<script>
import Users from "./components/Users"
export default {
  name: 'App',
  data(){
    return{
      users:["Henry","Bucky","Emily"]
    }
  },
  components:{
    "users":Users
  }
}
``` 

```js
<template>
  <div class="hello">
    <ul>
      <li v-for="user in users">{{user}}</li>//遍历传递过来的值，然后呈现到页面
    </ul>
  </div>
</template>
<script>
export default {
  name: 'HelloWorld',
  props:{
    users:{           //这个就是父组件中子标签自定义名字
      type:Array,
      default () {
        return []
      },
      required:true
    }
  }
}
</script>
``` 

### 2.子组件向父组件传值（通过事件形式）
子组件向父组件传递通过$emit派发事件    
子$emit -> 父v-on

```js
// 子组件
<template>
  <header>
    <h1 @click="changeTitle">{{title}}</h1>//绑定一个点击事件
  </header>
</template>
<script>
export default {
  name: 'app-header',
  data() {
    return {
      title:"Vue.js Demo"
    }
  },
  methods:{
    changeTitle() {
      this.$emit("titleChanged","子向父组件传值");//自定义事件  传递值“子向父组件传值”
    }
  }
}
</script>
``` 
```js
// 父组件
<template>
  <div id="app">
    <app-header v-on:titleChanged="updateTitle" ></app-header>//与子组件titleChanged自定义事件保持一致
   // updateTitle($event)接受传递过来的文字
    <h2>{{title}}</h2>
  </div>
</template>
<script>
import Header from "./components/Header"
export default {
  name: 'App',
  data(){
    return{
      title:"传递的是一个值"
    }
  },
  methods:{
    updateTitle(e){   //声明这个函数
      this.title = e;
    }
  },
  components:{
   "app-header":Header,
  }
}
</script>
``` 

### 3.兄弟组件之间通信

eventBus 来触发事件和监听事件  
$emit发送数据 $on接收数据

```js
import Vue from 'vue';
export default new Vue();

<div @click="addCart">添加</div>
import Bus from 'bus.js';
export default{
    methods: {
        addCart(event){
            Bus.$emit('getTarget', event.target)
        }
    }
}
// 另一组件
export default{
    created(){
        Bus.$on('getTarget', target =>{
            console.log(target)
        })
    }
}

``` 

### 4.vuex
### 5.$parent / $children
在父组件使用$children访问子组件，在子组件中使用$parent访问父组件

```js
// 父组件
data(){
    return {
        msg: '父组件数据'
    }
},
methods: {
    test(){
        console.log('我是父组件的方法，被执行')
    }
},
mounted(){
    console.log(this.$children[0].child_msg); // 执行子组件方法
}
// 子组件
<div>{{$parent.msg}}</div>

mounted(){
    // 子组件执行父组件方法
    this.$parent.test(); 
}

``` 

### 6.$attrs / $listeners

$attrs--继承所有父组件属性（除了prop传递的属性）  
inheritAttrs--默认值true，继承所有父组件属性（除props），为true会将attrs中的属性当做html的data属性渲染到dom根节点上  
$listeners--属性，包含了作用在这个组件上所有监听器，v-on="$listeners"将所有事件监听器指向这个组件的某个特定子元素  


```js
// 父组件
<children :child1="child1" :child2="child2" @test1="onTest1"
@test2="onTest2"></children>

data(){
    return {
        child1: 'childOne',
        child2: 'childTwo'
    }
},
methods: {
    onTest1(){
        console.log('test1 running')
    },
    onTest2(){
        console.log('test2 running')
    }
}

// 子组件
<p>{{child1}}</p>
<child v-bind="$attrs" v-on="$listeners"></child>

props: ['child1'],
mounted(){
    this.$emit('test1')
}

// 孙组件
<p>{{child2}</p>
<p>{{$attrs}}</p>

props: ['child2'],
mounted(){
    this.$emit('test2')
}
``` 

[vue组件间通信六种方式](https://www.jianshu.com/p/c015141441f4)

