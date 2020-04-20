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

## JSONP
利用script标签没有跨域限制的漏洞，动态的创建script标签，再去请求一个带参网址来实现跨域通信

## CORS

服务端设置 Access-Control-Allow-Origin 就可以开启 CORS

## postMessage跨域

## webpack配置代理
