## cookie
数据大小不超过4k  
设定过期时间  
每次都携带在HTTP请求中
同一个域名下存放的数量有限制 
支持设置为 HttpOnly，防止 Cookie 被客户端的 JavaScript 访问  

## sessionStorage 会话存储
数据在当前浏览器窗口关闭后自动删除  
以 key 和 value 的形式进行存储数据，用法类似 localStorage

## localStorage 本地存储
大小限制为 5MB ~ 10MB  
在同源的所有标签页和窗口之间共享数据  
永久存储，浏览器关闭后数据不丢失，除非主动删除数据
  
  ```js
localStorage.setItem("key", "value");
localStorage.getItem("key");
localStorage.removeItem("key");
localStorage.clear();
  ```

## Web SQL

Web SQL 能方便进行对象存储  
Web SQL 支持事务，能方便地进行数据查询和数据处理操作  
HTML5 已经放弃 Web SQL 数据库  

## IndexedDB

存储空间大：存储空间可以达到几百兆甚至更多  
支持二进制存储：它不仅可以存储字符串，而且还可以存储二进制数据  
IndexedDB 有同源限制，每一个数据库只能在自身域名下能访问，不能跨域名访问  
支持事务型：IndexedDB 执行的操作会按照事务来分组的，在一个事务中，要么所有的操作都成功，要么所有的操作都失败  
键值对存储：IndexedDB 内部采用对象仓库（object store）存放数据。所有类型的数据都可以直接存入，包括 JavaScript 对象。对象仓库中，数据以 “键值对” 的形式保存，每一个数据记录都有对应的主键，主键是独一无二的，不能有重复，否则会抛出一个错误  
数据操作是异步的：使用 IndexedDB 执行的操作是异步执行的，以免阻塞应用程序