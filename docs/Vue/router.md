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
beforeRouteLeave(to,from, next){}

```js
export default {
  template: `...`,
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
