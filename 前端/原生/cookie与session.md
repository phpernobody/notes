## cookie
#### 概念
http是无状态的协议，但是现实业务却需要一定的状态，否则无法区分用户之间的身份，为此最早的解决方案就是cookie。cookie是保存在客户端的，客户端和服务端都可以去修改（除非服务端有设置httpOnly）

#### cookie的处理步骤
1. 服务器向客户端发送cookie
2. 客户端将cookie保存起来
3. 之后客户端的每次请求都会将cookie发送给服务器
4. 服务器根据客户端发送的cookie知晓用户的状态，同时服务端也可以重新设置cookie

#### 服务端如何设置cookie
>通过header设置，格式：`Set-Cookie: <name>=<value>; Path=/; Expires=<UTC>; Domain=<.domain.com>; maxAge=<millisecond>; HttpOnly=<boolean>`

- path：表示cookie影响到的路径，当访问路径不能匹配时，浏览器则不发送这个Cookie
- Expires：过期时间，cookie过期的具体时间点，格式为UTC
- maxAge：过期时间，单位为ms，表示设置的毫秒数之后cookie失效
- HttpOnly：当设置为true时，客户端无法更改cookie
- Secure：当设置为true时，cookie只在https中有效

#### 缺点
- 安全上：cookie支持客户端修改，简单地通过cookie来表示用状态有可能被人为修改后获得一些特殊权限
- 性能上：cookie设置多了，报头大，对性能产生一定的影响，而且同一个域名下的静态页面都会自动带上一些无用的cookie

## session
#### 概念
为了解决cookie的缺陷，session应运而生，session的数据只保留在服务器端，客户端无法修改

#### 缺陷
无法将信息与客户端统一起来

#### 解决方案
- 目前最常用的方案，就是通过token来统一信息，客户端通过cookie存储token，服务端将token作为session信息的键，举例：
	- 客户端请求服务端
	- 服务端解析请求信息，发现cookie中没有token，生成唯一token，并将session信息作为token键的值存入
	- 服务端向客户端发送cookie，内容为token
	- 客户端保存token
	- 客户端请求服务端
	- 服务端检查cookie，根据token值取出对应的session信息
	- ...

- 还有一种方式是通过查询字符串来实现的，基本逻辑与token的方式一样
- 通过token的方案容易被伪造token，所以需要升级一下，通过私钥加密的方法对token进行签名，详细方法另查

## 简单比较两者
cookie相当于客户端的一个重要信息存储空间，服务端和客户端都可以去读写
session相当于服务端的一个重要信息存储空间，只有服务端能够读写，客户端想要获取只能向服务端请求