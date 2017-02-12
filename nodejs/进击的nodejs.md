1、url
	
- url.parse([urlString], [query参数是否为对象，boolen], [是否有协议方法,boolean]);
		- url.parse('http://www.baidu.com', true, true);
	- url.resolve([urlJSON]);

2、querystring

- 序列化：querystring.stringify([对象], [参数间的连接符号], [key-value之间的连接符号])
	- querystring.stringify({a:'1',b:'2'}, ',', '#');
	- output：a#1,b#2
- 反序列化：querystring.parse([string], [参数间的连接符号], [key-value之间的连接符号], [解析的最大参数个数])
	- querystring.parse('a#1,b#2', ',', '#', 1)
	- output：{a:1}
- 转义：querystring.escape([要转义的字符串])
- 反转义：querystring.unescape([需要反转义的内容])

3、http

- 在chrome浏览器发起一个网页请求的具体过程
	- chrome搜索自身缓存（小技巧：查看chromeDNS缓存-chrome://net-internals/#dns)
	- 搜索操作系统自身缓存
	- 读取本地HOST文件
	- 浏览器发起一个DNS的一个系统调用
		- 宽带运营商服务器查看本身缓存
		- 运营商服务器发起一个迭代DNS解析请求
		- ...
		- 拿到后返回给操作系统缓存起来
		- 操作系统内核把结果返回给浏览器
	- 浏览器进行“三次握手”
	- TCP/IP连接建立后就可以向服务器发送http请求
	- 拿到服务器返回的数据后浏览器进行解析渲染网页
- http请求方法
	- GET
	- POST
	- PUT
	- DELETE
	- HEAD
	- TRACE
	- OPTIONS
- 状态码
	- 1**：请求已经接受
	- 2**：请求接收并处理
	- 3**：重定向
	- 4**：请求有错误
	- 5**：服务端有错误
- http概念
	- 回调：后续函数作为参数注入到执行函数中；
	- 同步（阻塞）：程序按顺序执行，单线程；
	- 异步（非阻塞）：程序不一定是按顺序执行，有可能前一个函数挂起先执行其他函数

4、javascript作用域和执行上下文
- 作用域：
	- 作用范围，有层级关系，如平常所理解的那样
- 执行上下文：
	- 指this，简单理解就是当前函数的拥有者，每调用一个函数创建一个执行上下文，执行完毕销毁；

5、http性能测试
- 使用apache的ab工具，使用方法如下：
	- 以wamp为例，进入wamp/Apache2/bin,
	- 运行指令ab -n[total request] -c[concurrency number] [test url]

6、小爬虫
- 推荐一个工具：cheerio
- 