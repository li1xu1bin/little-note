## 完整的导航解析流程

1. 导航被触发。
2. 在失活的组件里调用 `beforeRouteLeave` 守卫。
3. 调用全局的 `beforeEach` 守卫。
4. 在重用的组件里调用 `beforeRouteUpdate` 守卫 (2.2+)。
5. 在路由配置里调用 `beforeEnter`。
6. 解析异步路由组件。
7. 在被激活的组件里调用 `beforeRouteEnter`。
8. 调用全局的 `beforeResolve` 守卫 (2.5+)。
9. 导航被确认。
10. 调用全局的 `afterEach` 钩子。
11. 触发 DOM 更新。
12. 调用 `beforeRouteEnter` 守卫中传给 `next` 的回调函数，创建好的组件实例会作为回调函数的参数传入。

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