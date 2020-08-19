## 路由方案

```js
  <transition :name="transitionName">
    <keep-alive>
      <router-view v-if="$route.meta.keepAlive" ></router-view>
    </keep-alive>
  </transition>
  <transition :name="transitionName">
      <router-view v-if="!$route.meta.keepAlive" :key="key"></router-view>
  </transition>

  data () {
    return {
      transitionName: ''
    }
  },
  computed: {
    key () {
      // 再次进入时刷新数据
      return this.$route.fullPath + Math.random()
    }
  },
  watch: {
    $route (to, from) {
      // 切换动画
      if (to.meta.index < from.meta.index) {
        // 设置动画名称
        this.transitionName = 'slide-left'
      } else {
        this.transitionName = 'slide-right'
      }
    }
  },
```

```css
// 页面过渡动画
.slide-right-enter-active,
.slide-right-leave-active,
.slide-left-enter-active,
.slide-left-leave-active {
  will-change: transform;
  transition: all 0.5s;
  position: absolute;
  width: 100%;
  left: 0;
}
.slide-right-enter {
  opacity: 0;
  transform: translate3d(100%, 0, 0);
}
.slide-right-leave-active {
  opacity: 0;
  transform: translate3d(-100%, 0, 0);
}
.slide-left-enter {
  opacity: 0;
  transform: translate3d(-100%, 0, 0);
}
.slide-left-leave-active {
  opacity: 0;
  transform: translate3d(100%, 0, 0);
}
```
## 前进页面缓存，后退页面销毁
```js
beforeRouteLeave (to, from, next) {
  if (to.name === 'peopleList') {
    if (!from.meta.keepAlive) {
      from.meta.keepAlive = true
    }
    next()
  } else {
    from.meta.keepAlive = false
    this.$destroy() // 销毁实例
    next()
  }
},
```

## 适配方案

### 移动端适配方案之postcss-px-to-viewport 
不兼容PC端  
npm install postcss-px-to-viewport --save-dev

```js
module.exports = {
  "plugins": {
    "postcss-import": {},
    "postcss-url": {},
    // to edit target browsers: use "browserslist" field in package.json
    "autoprefixer": {},
    'postcss-px-to-viewport': {
      viewportWidth: 750,   // 视窗的宽度，对应的是我们设计稿的宽度，一般是750
      // viewportHeight: 1334, // 视窗的高度，根据750设备的宽度来指定，一般指定1334，也可以不配置
      unitPrecision: 3,     // 指定`px`转换为视窗单位值的小数位数
      viewportUnit: "vw",   //指定需要转换成的视窗单位，建议使用vw
      selectorBlackList: ['.ignore'],// 指定不转换为视窗单位的类，可以自定义，可以无限添加,建议定义一至两个通用的类名
      minPixelValue: 1,     // 小于或等于`1px`不转换为视窗单位，你也可以设置为你想要的值
      mediaQuery: false     // 允许在媒体查询中转换`px`
   }
  }
}
```
