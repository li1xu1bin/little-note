## $nextTick
Vue是异步渲染  
data改变之后，DOM不会立刻渲染  
$nextTick会在DOM渲染之后被触发，获取最新DOM节点
```js
new Vue({
  el: '.app',
  data: {
    msg: 'Hello Vue.',
    msg1: '',
    msg2: '',
    msg3: ''
  },
  methods: {
    changeMsg() {
      this.msg = "Hello world."//只有msg2会更新到
      this.msg1 = this.$refs.msgDiv.innerHTML
      this.$nextTick(() => {
        this.msg2 = this.$refs.msgDiv.innerHTML
      })
      this.msg3 = this.$refs.msgDiv.innerHTML
    }
  }
})
```


## slot插槽

父组件
```
<template>
  <div>
    我是父组件
    <slotOne1>
      <p style="color:red">我是父组件插槽内容</p>
    </slotOne1>
  </div>
</template>
```

子组件(slotOne1)  

```
<template>
  <div class="slotOne1">
    <div>我是slotOne1组件</div>
    <slot></slot>
  </div>
</template>
```
当组件渲染的时候，子组件的`<slot></slot>`将会被替换

### 具名插槽
多个插槽，指定name  

子组件
```
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```

父组件  

```
<base-layout>
  <template v-slot:header>
    <h1>Here might be a page title</h1>
  </template>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template v-slot:footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>
```
现在 `<template>` 元素中的所有内容都将会被传入相应的插槽。任何没有被包裹在带有 v-slot 的 `<template>` 中的内容都会被视为默认插槽的内容。

### 作用域插槽

子组件的变量能给父组件访问

子组件
```
<span>
  <slot v-bind:user="user">
    {{ user.lastName }}
  </slot>
</span>
```

父组件
```
<current-user>
  {{ user.firstName }}
</current-user>
```

旧写法
```
<template>
  <div>
    我是作用域插槽的子组件
    <slot :data="user"></slot>
  </div>
</template>

<script>
export default {
  name: 'slotthree',
  data () {
    return {
      user: [
        {name: 'Jack', sex: 'boy'},
        {name: 'Jone', sex: 'girl'},
        {name: 'Tom', sex: 'boy'}
      ]
    }
  }
}
</script>
```

```
<template>
  <div>
    我是作用域插槽
    <slot-three>
      <template slot-scope="user">
        <div v-for="(item, index) in user.data" :key="index">
        {{item}}
        </div>
      </template>
    </slot-three>
  </div>
</template>
```

## 混入mixins

```
import { statMixin } from '../common/mixins'
export default {
    mixins: [statMixin]
}
```

data 将进行递归合并，对于键名冲突的以组件数据为准：

```js
// mixinA 的 data
data() {
    obj: {
        name: 'bubuzou',
    },
}

// component A
export default {
    mixins: [mixinA],
    data(){
        obj: {
            name: 'hello',
            age: 21
        },
    },
    mounted() {
        console.log( this.obj )  // { name: 'bubuzou', 'age': 21 }    
    }
}
```

对于生命周期钩子函数将会合并成一个数组，混入对象的钩子将先被执行：
```js
// mixin A
const mixinA = {
    created() {
        console.log( '第一个执行' )
    }
}

// mixin B
const mixinB = {
    mixins: [mixinA]
    created() {
        console.log( '第二个执行' )
    }
}

// component A
export default {
    mixins: [mixinB]
    created() {
        console.log( '最后一个执行' )
    }
}
```

## 自定义指令
```js
<button v-auth="['user']">提交</button>

// auth.js
const AUTH_LIST = ['admin']

function checkAuth(auths) {
    return AUTH_LIST.some(item => auths.includes(item))
}

function install(Vue, options = {}) {
    Vue.directive('auth', {
      // inserted：被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。
        inserted(el, binding) {
            if (!checkAuth(binding.value)) {
                el.parentNode && el.parentNode.removeChild(el)
            }
        }
    })
}

export default { install }


import Auth from './utils/auth'
Vue.use(Auth)

```

## 自定义插件

为 Vue 添加全局功能，一般是添加全局方法/全局指令/过滤器等  
通过 install 方法给 Vue 添加全局功能  
通过全局方法 Vue.use() 使用插件  

```js
//所谓vue的插件，就是一个js对象
let myplugin={
    install:function(Vue,Options){
        // 添加属性与方法
        //这里我写的$testProp等加了$符号的，表示他为vue全局的，但实际上不加也可以的，访问时也不加就行了
        Vue.prototype.$myoption='我是来自插件的属性',
        Vue.prototype.$myfn=function(){
            console.log('我是来自插件的方法')
        }
        // 添加全局混入
        Vue.mixin({
            mounted() {
                console.log('组件创建成功')
            },
        })
        // 添加全局指令
        Vue.directive('dir',{
            inserted:function(ele){
                ele.style.border='2px solid green'
            }
        })
    }
}
export default myplugin;

import myplugin from 'js'
Vue.use(myplugin)

```
