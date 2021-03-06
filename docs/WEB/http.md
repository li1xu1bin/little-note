## HTTP状态码

### 2XX 请求成功
* 200 OK 服务器已成功处理了请求

### 3XX 请求重定向
* 301 Moved Permanently 请求的网页已永久移动到新位置

### 4XX 客户端错误
* 400 Bad Request 错误请求，服务器无法理解请求
* 401 Unauthorized 未授权，要求登录身份验证
* 403 Forbidden 服务器禁止请求
* 404 Not Found 服务器找不到请求的网页

### 5XX 服务端错误
* 500 Internal Server Error 服务器遇到错误，无法完成请求
* 502 Bad Gateway 错误网关，从上游服务器收到无效响应
* 503 Service Unavailable 服务器目前无法使用
* 504 Gateway Timeout 网关超时，没有及时从上游服务器收到请求

## 加载到渲染

### 从浏览器地址栏输入url到显示页面的步骤
加载过程  
1. 浏览器查找缓存
2. 根据 DNS 服务器解析得到域名的 IP 地址
3. 建立 TCP 链接，发送 HTTP 请求
4. 服务器将 响应报文 回馈，浏览器进行处理

渲染过程  
1. 根据 HTML 结构生成 DOM树
2. 根据 CSS 生成 CSSOM树
3. 根据 DOM树 和 CSSOM树 构建 渲染树
4. 解析js，会阻塞渲染

详细解释：https://www.cnblogs.com/xiongmaoblog/p/6263658.html

## 重绘和回流
* 重绘：当页面中元素样式的改变并不影响它在文档流中的位置时（例如：color、background-color、visibility等），浏览器会将新样式赋予给元素并重新绘制它，这个过程称为重绘。visibility: hidden;  
* 回流：当渲染树中部分或全部元素的尺寸、结构、或某些属性发生改变时，浏览器重新渲染部分或全部文档的过程称为回流。display: none;
* 回流消耗性能开支比重绘大
* 回流必将引起重绘，重绘不一定会引起回流

## 防抖和节流
* 防抖：对于短时间内连续触发的事件（上面的滚动事件），防抖的含义就是让某个时间期限（如上面的1000毫秒）内，事件处理函数只执行一次。
```js
function debounce(fn,delay){
    let timer = null //借助闭包
    return function() {
        if(timer){
            clearTimeout(timer) 
        }
        timer = setTimeout(fn,delay) // 简化写法
    }
}
// 然后是旧代码
function showTop  () {
    var scrollTop = document.body.scrollTop || document.documentElement.scrollTop;
　　console.log('滚动条位置：' + scrollTop);
}
window.onscroll = debounce(showTop,1000) 
```

* 节流：如果短时间内大量触发同一事件，那么在函数执行一次之后，该函数在指定的时间期限内不再工作，直至过了这段时间才重新生效。
```js
function throttle(fn,delay){
    let valid = true
    return function() {
       if(!valid){
           //休息时间 暂不接客
           return false 
       }
       // 工作时间，执行函数并且在间隔期内把状态位设为无效
        valid = false
        setTimeout(() => {
            fn()
            valid = true;
        }, delay)
    }
}
function showTop  () {
    var scrollTop = document.body.scrollTop || document.documentElement.scrollTop;
　　console.log('滚动条位置：' + scrollTop);
}
window.onscroll = throttle(showTop,1000) 
```
防抖：只判断最后一次，停下来才执行    
节流：隔一段时间才判断一次  
防抖：是将多次执行变为最后一次执行  
节流：是将多次执行变为每隔一段时间执行

## 性能优化

1. 减少HTTP请求：压缩合并代码，图片压缩，合并sprite
2. 减少DOM操作
3. 静态资源缓存，Ajax缓存
4. 预加载和懒加载
5. 服务端渲染页面，直接输出数据

[前端性能优化最佳实践](https://csspod.com/frontend-performance-best-practices/)  

#### 页面内容  
减少 HTTP 请求数  
减少 DNS 查询  
避免重定向  
缓存 Ajax 请求  
延迟加载  
预先加载  
减少 DOM 元素数量  
划分内容到不同域名  
尽量减少 iframe 使用  
避免 404 错误  
#### 服务器  
使用 CDN  
添加 Expires 或 Cache-Control 响应头  
启用 Gzip  
配置 Etag  
尽早输出缓冲  
Ajax 请求使用 GET 方法  
避免图片 src 为空  
Cookie  
减少 Cookie 大小  
静态资源使用无 Cookie 域名  
#### CSS  
把样式表放在 <head> 中  
不要使用 CSS 表达式  
使用 <link> 替代 @import  
不要使用 filter  
#### JavaScript  
把脚本放在页面底部  
使用外部 JavaScript 和 CSS  
压缩 JavaScript 和 CSS  
移除重复脚本  
减少 DOM 操作  
使用高效的事件处理  
#### 图片  
优化图片  
优化 CSS Sprite  
不要在 HTML 中缩放图片  
使用体积小、可缓存的 favicon.ico  
#### 移动端  
保持单个文件小于 25 KB  
打包内容为分段（multipart）文档  

## SEO
1. 语义化的 HTML 代码  
2. 网站结构布局优化  
3. 合理的 title、description、keywords  
4. 非装饰性图片必须加 alt  
5. 启用GZIP压缩，提高网站的加载速度  
6. SSR技术  

## HTTP缓存
浏览器第一次向一个web服务器发起http请求后，服务器会返回请求的资源，并且在响应头中添加一些有关缓存的字段如：Cache-Control、Expires（过期时间）、Last-Modified、ETag、Date等等。

强缓存：浏览器直接从本地缓存中获取数据，不与服务器进行交互。  
控制字段有 Expires 和 Cache-Control  

协商缓存：浏览器发送请求到服务器，服务器判定是否可使用本地缓存。  
控制字段有 Etag / If-None-Match 和 Last-Modified / If-Modified-Since

## HTTP和HTTPS区别
http：80端口  
https：443端口，加多一层SSL协议  
内容加密 验证身份 保证数据完整性

## XSS 和 CSRF

### XSS 跨站脚本攻击
XSS：恶意攻击者往 Web 页面里插入恶意 Script 代码，当用户浏览该页之时，嵌入其中 Web 里面的 Script 代码会被执行，从而达到恶意攻击用户的目的。

防范方法：
1.HttpOnly 防止劫取 Cookie  
2.用户的输入检查  
3.服务端的输出检查  

### CSRF 跨站请求伪造
劫持受信任用户向服务器发送非预期请求的攻击方式。
CSRF：攻击者借助受害者的 Cookie 骗取服务器的信任，可以在受害者毫不知情的情况下以受害者名义伪造请求发送给受攻击服务器，从而在并未授权的情况下执行在权限保护之下的操作。


防范方法：
1.验证码  
2.Referer Check  
3.Token 验证