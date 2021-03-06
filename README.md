# HttpServer
模拟实现一个简易版http服务器


---
### HTTP服务器
通过浏览器，发送一个标准的HTTP请求，就能够得到一个标准的HTTP响应。

如果请求的是一个html网页，那么就能在浏览器中看到对应的结果。

### 实现的功能
1. 能够接收标准的HTTP请求。
    - GET方法
    - POST方法
2. 能够根据请求做出一个标准的HTTP响应
    - 能够根据url返回一个服务器上的静态文件（html，css，JavaScript，图片...）。
    - 根据请求中的参数（url，body）动态生成一个页面（基于CGI的方法）。

### 模块划分
1. 初始化模块（实现一个TCP服务器）
2. 响应请求模块（使用多线程的方法处理并发送的请求）
    - 读取请求并解析（操作解析字符串）
    - 根据请求内容进行计算
        - 处理静态文件（直接将静态文件内容返回）
        - 处理动态页面（使用CGI的方法实现动态计算生成页面）
    - 把响应的结果可回给客户端（操作和拼接字符串）

### CGI协议
1. 收到的请求是一个GET并带有QUERY_STRING或者POST触发的CGI逻辑。
2. 创建一个子进程，子进程通过exec执行一个本地的可执行程序，通过这个可执行程序完成动态页面的计算。
3. 涉及到一个重要的问题：
    - 子进程需要知道HTTP请求具体什么样子。
        - 需要知道进行替换哪个可执行文件。
        - 需要知道HTTP请求的方法是什么。
        - 需要知道QUERY_STRING是什么。
        - 需要知道body是什么
    - 子进程也要把计算完毕的结果（HTTP响应的body，部分header）反馈给父进程
