## 微信小程序

### App的生命周期
在app.js调用App方法注册小程序实例  
onLaunch、onShow、onHide

### Page的生命周期
使用Page()构造  
onLoad、onShow、onReady、onHide、onUnload

### 组件的生命周期
created、attached、detached


### 路由跳转
wx.navigateTo 打开新页面  
wx.redirectTo 页面重定向  
wx.navigateBack 页面返回  
wx.switchTab Tab 切换  
wx.reLaunch 重启动  

wx.createSelectorQuery() 获取界面上的节点信息
```js
const query = wx.createSelectorQuery()
query.select('#the-id').boundingClientRect(function(res){
  res.top // #the-id 节点的上边界坐标（相对于显示区域）
})
query.selectViewport().scrollOffset(function(res){
  res.scrollTop // 显示区域的竖直滚动位置
})
query.exec()//执行请求
```

### 授权登陆
wx.login 获取code登陆凭证  

wx.getUserInfo 获取用户信息userInfo rawData signature encryptedData iv  

发送给服务器 换取openid和session_key

### 微信支付
```js
wx.requestPayment({
  timeStamp: '',//时间戳
  nonceStr: '',//随机字符串
  package: '',//统一下单接口返回的 prepay_id 参数值，提交格式如：prepay_id=***
  signType: 'MD5',//签名算法
  paySign: '',//签名
  success (res) { },
  fail (res) { }
})
```

### 性能优化  
1.setData 数据不要太大 不要频繁调用  
2.减少图片大小，图片懒加载  
3.减少wx.request请求和onPageScroll事件

## 微信公众号

### 授权登陆

1. 引导用户跳转到微信授权网页

2. 通过code换取网页授权access_token

3. 根据token获取用户信息

```js
this.$router.onReady(() => {
  let token = this.$route.query.token
  localStorage.setItem('token', token)
})
```

### 微信支付
```js
wx.config({
  debug: false,
  appId: res.data.appId,
  timestamp: res.data.timestamp,
  nonceStr: res.data.noncestr,
  signature: res.data.signature,
  jsApiList: ['chooseWXPay']
})
wx.ready(res => {
  wx.checkJsApi({
    jsApiList: ['chooseWXPay'],
    success: res => {
      console.log('checked api:', res)
    },
    fail: err => {
      console.log('check api fail:', err)
    }
  })
})
wx.chooseWXPay({
  // 支付签名时间戳，注意微信jssdk中的所有使用timestamp字段均为小写。但最新版的支付后台生成签名使用的timeStamp字段名需大写其中的S字符
  timestamp: data.timeStamp,
  // 支付签名随机串，不长于 32 位
  nonceStr: data.nonceStr,
  // 统一支付接口返回的prepay_id参数值，提交格式如：prepay_id=\*\*\*）
  package: data.package,
  // 签名方式，默认为'SHA1'，使用新版支付需传入'MD5'
  signType: data.signType,
  // 支付签名
  paySign: data.paySign,
  // 支付成功后的回调函数
  success: function (res) {
  },
  // 支付取消回调函数
  cencel: function (res) {
  },
  // 支付失败回调函数
  fail: function (res) {
  }
})

```

## Uniapp

### 生命周期
应用生命周期 onLaunch onShow onHide onError  
页面生命周期 onLoad onShow onReady onHide onUnload 
onPullDownRefresh（下拉刷新） onReachBottom（触底） onPageScroll（页面滚动）  
组件生命周期 与vue标准组件的生命周期相同

### 页面通讯
uni.$emit 触发全局的自定事件  
uni.$on 监听全局的自定义事件  
uni.$once 监听全局的自定义事件,只触发一次，在第一次触发之后移除监听器  
uni.$off 移除全局自定义事件监听器

```
  uni.$emit('update',{msg:'页面更新'})

  uni.$on('update',function(data){
    console.log('监听到事件来自 update ，携带参数 msg 为：' + data.msg);
  })
```

### 条件编译

```
// #ifdef  %PLATFORM%
平台特有的API实现
// #endif

<!--  #ifdef  %PLATFORM% -->
平台特有的组件
<!--  #endif -->

/*  #ifdef  %PLATFORM%  */
平台特有样式
/*  #endif  */
```
### pages.json

subPackages 分包加载配置  
preloadRule 分包预载配置

