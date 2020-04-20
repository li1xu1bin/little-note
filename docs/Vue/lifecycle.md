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

