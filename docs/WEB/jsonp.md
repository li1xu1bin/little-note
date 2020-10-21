## 同源策略

同源：两个文档同源需满足  
协议相同  
域名相同  
端口相同

## Nginx反向代理

通过nginx配置一个代理服务器（域名与domain1相同，端口不同）做跳板机，反向代理访问domain2接口

  ```js
server {
    listen       81;
    server_name  www.domain1.com;
    location / {
        proxy_pass   http://www.domain2.com:8080;  #反向代理
    }
}
  ```

## WebSocket

WebSocket 是一种双向通信协议，在建立连接之后，WebSocket 的 server 与 client 都能主动向对方发送或接收数据

```
onopen 连接建立
onmessage 接收数据
onerror 发生错误
onclose 连接关闭
send() 发送数据
```

## JSONP
利用script标签没有跨域限制的漏洞，动态的创建script标签，再去请求一个带参网址来实现跨域通信

```
(function(){
    var _script = document.createElement('script');
    _script.type = 'text/javascript';
    _script.src = 'http://localhost/demo/';
    document.body.appendChild(_script);
})();
```

## CORS 跨域资源共享

服务端设置 Access-Control-Allow-Origin 就可以开启 CORS

浏览器将CORS请求分为两类：简单请求（simple request）和非简单请求（not-simple-request）。

符合以下任一情况的就是复杂请求：

1.使用方法put/delete/patch/post;

2.发送json格式的数据（content-type: application/json）

3.请求中带有自定义头部；

Options请求出现的情况有两种，复杂请求触发预检请求

1、获取后台服务器支持的HTTP的通信方式

2、对跨域请求进行preflight request(预检请求)。

预检请求首先需要向另外一个域名的资源发送一个Http Options的请求头，以检测实际发送的请求是否是安全的。options请求是浏览器自发起的preflight request(预检请求)。

## postMessage跨域

## webpack配置代理
