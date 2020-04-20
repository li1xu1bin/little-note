## cookie
数据大小不超过4k
设定过期时间
每次都携带在HTTP请求中

## sessionStorage 会话存储
数据在当前浏览器窗口关闭后自动删除  
以 key 和 value 的形式进行存储数据，用法类似 localStorage

## localStorage 本地存储
永久存储，浏览器关闭后数据不丢失，除非主动删除数据
  
  ```js
localStorage.setItem("key", "value");
localStorage.getItem("key");
localStorage.removeItem("key");
  ```