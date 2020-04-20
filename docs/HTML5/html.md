## Doctype 
1. <!doctype>声明不是一个HTML标签，用于告诉浏览器当前HTML版本
2. 浏览器检查doctype决定使用兼容模式还是标准模式对文档进行渲染
3. HTML4基于SGML，<!doctype>声明指向一个DTD
2. HTML5不基于SGML，所以不用指定DTD

## Meta

```
<!DOCTYPE html> H5标准声明，使用 HTML5 doctype，不区分大小写
<head lang="en"> 标准的 lang 属性写法
<meta charset="utf-8"> 声明文档使用的字符编码
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1"/> 优先使用 IE 最新版本和 Chrome
<meta name="description" content="不超过150个字符"/> 页面描述
<meta name="keywords" content=""/> 页面关键词
<meta name="author" content=""/> 网页作者
<meta name="robots" content="index,follow"/> 搜索引擎抓取
```

## Viewport

  ```
<meta name="viewport" content="width=device-width,initial-scale=1, maximum-scale=1,minimum-scale=1,user-scalable=no"> 

  ```
`width`：控制 viewport 的大小，可以指定的一个值，如 600，或者特殊的值，如 device-width 为设备的宽度（单位为缩放为 100% 时的 CSS 的像素）。  
`initial-scale`：初始缩放比例，也即是当页面第一次 load 的时候缩放比例。  
`maximum-scale`：允许用户缩放到的最大比例。  
`minimum-scale`：允许用户缩放到的最小比例。  
`user-scalable`：用户是否可以手动缩放。  

## H5幻灯片

[fullpage](https://github.com/kisnows/fullpage) 

[移动端开发](https://github.com/jtyjty99999/mobileTech) 

[H5单页面手势滑屏切换原理](http://www.cnblogs.com/onepixel/p/5300445.html)  

## 工程化


[git - 简明指南](http://www.runoob.com/manual/git-guide/)  

[30分钟学会使用grunt打包前端代码](http://www.cnblogs.com/yexiaochai/p/3603389.html) 

[Chrome 开发者工具](http://www.css88.com/doc/chrome-devtools/) 

