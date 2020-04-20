## 语义化标签
  `<header>` 定义文档的头部区域  
  `<footer>` 定义文档的尾部区域  
  `<nav>`    定义文档的导航  
  `<section>`定义文档中的节   
  `<article>`定义页面独立的内容区域  
  `<aside>`  定义了侧边栏内容    
  `<nav>`    定义了文档的导航  

  语义化的好处：代码结构清晰，有利于SEO  
  低版本IE兼容方案：  
  1.通过`createElement`添加自定义标签,将display设为block  
  2.引入`html5shiv`

## 多媒体增强
  `<video>`  视频    
  `<audio>`  音频    

## 表单增强
  新的表单控件：calendar 、date 、time 、email 、url 、search 

## 本地存储
  `localStorage`  持久存储数据，浏览器关闭不删除，只能主动删除，用于实现标签间通信    
  `sessionStorage`  数据在当前浏览器窗口关闭后自动删除  

```js
	 localStorage.setItem("key","value"); //存储数据信息到本地
	 localStorage.getItem("key"); //读取本地存储的信息
	 localStorage.removeItem("key"); //删除本地存储的信息
	 localStorage.clear(); //清空所有存储的信息
```
  `cookie`  可设置过期时间，大小不超过4k  

## Canvas

[特效站](http://fff.cmiscm.com/#!/main)  

## 离线存储
离线，通过创建 cache manifest 文件，创建应用程序缓存