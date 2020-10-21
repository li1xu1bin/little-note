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

## 导航守卫

### 全局守卫钩子

```js
const router = new VueRouter({....})
// 全局前置守卫
router.beforeEach((to, from, next) => {
	// do something
})
// 全局解析守卫
router.beforeResolve((to, from, next) => {
	// do something
})
// 全局后置守卫
router.afterEach((to, from) => {
	// do something
})
```  
    to：route 即将进入的路由
	from：route 即将离开的路由
	next：function
    next()：直接执行下一个钩子，如果执行完了导航状态为comfirmed状态
	next(false)：中断当前导航，回到from的位置
	next('/hello')或next({path:'/hello'})：路由到任意地址，可以携带参数等

### 路由独享守卫
独享守卫是定义在单独的某一个路由里的 beforeEnter 

```js
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
  ]
})
```  

### 路由组件守卫

beforeRouteEnter(to, from, next) {}  
beforeRouteUpdate(to, from, next) {}  
beforeRouteLeave(to, from, next){}

```js
<template>
\\\
</template>

export default {
  beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
  },
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  }
}
``` 

## 路由传参

1、通过<router-link>标签中的to传参

```
<router-link :to="{name:'view',query:{articleId:item.articleId}}">

```

2、query传递的参数会通过?xxx = xxx展示

```
this.$router.push({
    path: '/back/article/new',
    query: {
      articleId: val
    }
})
```

3、通过param来传递参数

```
this.$router.push({
    name:'Home',
    params:{
        id:id
    }
})
```

## 实例解决

1.实现页面的不重新加载
<keep-alive>包裹动态组件时，会缓存不活动的组件实例，而不是销毁它们，也就是所谓的第一次进入页面加载，返回等第二次进入页面不加载
```
<keep-alive>
		<router-view v-if="$route.meta.keepAlive"></router-view>
</keep-alive>
```

2.实现页面返回原来的位置
```js
// 跳转离开前，记录当前页面的位置
beforeRouteLeave(to, from, next) {
  this.scroll = this.$refs.medicalListContainer.scrollTop;
  next()
},
// 页面进入前
beforeRouteEnter(to, from, next) {
    next(vm => {
      vm.$refs.medicalListContainer.scrollTop = vm.scroll;
    })
}
```

## 动态权限路由
```
addRoutes
```