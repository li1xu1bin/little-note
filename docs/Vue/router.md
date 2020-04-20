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
	to：route即将进入的路由
	from：route即将离开的路由
	next：function这个是必须要调用的
	next()接受参数：
    next()：直接执行下一个钩子，如果执行完了导航状态为comfirmed状态
	next(false)：中断当前导航，回到from的位置
	next('/hello')或next({path:'/hello'})：路由到任意地址，可以携带参数等
	next(error)：会回到router.onError(callback)
   ```  

