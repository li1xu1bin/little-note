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


