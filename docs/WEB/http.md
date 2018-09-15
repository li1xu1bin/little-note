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
1. 根据 DNS 服务器解析得到域名的 IP 地址
2. 建立 TCP 链接，发送 HTTP 请求
3. 服务器将 响应报文 回馈，浏览器进行处理

渲染过程  
1. 根据 HTML 结构生成 DOM树
2. 根据 CSS 生成 CSSOM树
3. 根据 DOM树 和 CSSOM树 构建 渲染树
4. 解析js，会阻塞渲染

详细解释：https://www.cnblogs.com/xiongmaoblog/p/6263658.html

## 重绘和回流
* 重绘：当页面中元素样式的改变并不影响它在文档流中的位置时（例如：color、background-color、visibility等），浏览器会将新样式赋予给元素并重新绘制它，这个过程称为重绘。  
* 回流：当渲染树中部分或全部元素的尺寸、结构、或某些属性发生改变时，浏览器重新渲染部分或全部文档的过程称为回流。
* 回流消耗性能开支比重绘大
* 回流必将引起重绘，重绘不一定会引起回流

## 性能优化

1. 减少HTTP请求：压缩合并代码，图片压缩，合并sprite
2. 减少DOM操作
3. 静态资源缓存，Ajax缓存
4. 预加载和懒加载
5. 服务端渲染页面，直接输出数据