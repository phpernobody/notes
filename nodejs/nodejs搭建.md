# node快速搭建

## 参考

[express](http://www.expressjs.com.cn/)

## express
1. 安装：`npm install express --save`
2. 例子

		// 引入express框架
		var express = require('express');
		var app = express();
		
		// 路由配置
		app.get('/', function (req, res) {
		  res.send('hellow my node');
		});
		
		app.get('/home/', function (req, res) {
		  res.send('i am home');
		});
		
		// 开启服务器监听
		var server = app.listen(3000, function () {
		  var host = server.address().address;
		  var port = server.address().port;
		
		  console.log('Example app listening at http://%s:%s', host, port);
		})



3. express应用生成器：
	- 安装：`npm install express-generator -g`
	- 输入`express [项目名]`即可生成一整个项目，进入项目目录后运行`npm install`

4. 路由：
	- `app.METHOD(PATH, HANDLER)`,METHOD 是某个 HTTP 请求方式中的一个；PATH 是服务器端的路径；HANDLER 是当路由匹配到时需要执行的函数。
	- `app.all('[path]', function(req, res, next) { // do; next(); })`：用于给http请求加载中间件
	- 路由路径：可以用正则进行匹配，或用简化的正则匹配，详见例子：
	
			// 匹配 acd 和 abcd
			app.get('/ab?cd', function(req, res) {
			  res.send('ab?cd');
			});
			
			// 匹配 abcd、abbcd、abbbcd等
			app.get('/ab+cd', function(req, res) {
			  res.send('ab+cd');
			});
			
			// 匹配 abcd、abxcd、abRABDOMcd、ab123cd等
			app.get('/ab*cd', function(req, res) {
			  res.send('ab*cd');
			});
			
			// 匹配 /abe 和 /abcde
			app.get('/ab(cd)?e', function(req, res) {
			 res.send('ab(cd)?e');
			});
			
			// 匹配任何路径中含有 a 的路径：
			app.get(/a/, function(req, res) {
			  res.send('/a/');
			});
			
			// 匹配 butterfly、dragonfly，不匹配 butterflyman、dragonfly man等
			app.get(/.*fly$/, function(req, res) {
			  res.send('/.*fly$/');
			});

	- 路由句柄：可以使用一个或多个回调，通过next()来连接；也可以用数组形式，详见例子：

			// 多个回调处理
			app.get('/example/b', function (req, res, next) {
			  console.log('response will be sent by the next function ...');
			  next();
			}, function (req, res) {
			  res.send('Hello from B!');
			});
			
			// 通过数组形式
			var cb0 = function (req, res, next) {
			  console.log('CB0');
			  next();
			}
			
			var cb1 = function (req, res, next) {
			  console.log('CB1');
			  next();
			}
			
			var cb2 = function (req, res) {
			  res.send('Hello from C!');
			}
			
			app.get('/example/c', [cb0, cb1, cb2]);
			
			// 混合形式
			var cb0 = function (req, res, next) {
			  console.log('CB0');
			  next();
			}
			
			var cb1 = function (req, res, next) {
			  console.log('CB1');
			  next();
			}
			
			app.get('/example/d', [cb0, cb1], function (req, res, next) {
			  console.log('response will be sent by the next function ...');
			  next();
			}, function (req, res) {
			  res.send('Hello from D!');
			});
	- 响应方法：
		- 方法列表
			- `res.download()`:提示下载文件
			- `res.end()`:终结响应处理流程
			- `res.json()`:发送一个 JSON 格式的响应。
			- `res.jsonp()`:发送一个支持 JSONP 的 JSON 格式的响应
			- `res.redirect()`:重定向请求
			- `res.render()`:渲染视图模板
			- `res.send()`:发送各种类型的响应
			- `res.sendFile()`:以八位字节流的形式发送文件
			- `res.sendStatus()`:设置响应状态代码，并将其以字符串形式作为响应体的一部分发送
		- 创建路由路径的链式路由句柄，例子：

				app.route('/book')
				  .get(function(req, res) {
				    res.send('Get a random book');
				  })
				  .post(function(req, res) {
				    res.send('Add a book');
				  })
				  .put(function(req, res) {
				    res.send('Update the book');
				  });
		- `express.Router()`:创建模块化句柄，例子：

				var express = require('express');
				var router = express.Router();
				
				// 该路由使用的中间件
				router.use(function timeLog(req, res, next) {
				  console.log('Time: ', Date.now());
				  next();
				});
				// 定义网站主页的路由
				router.get('/', function(req, res) {
				  res.send('Birds home page');
				});
				// 定义 about 页面的路由
				router.get('/about', function(req, res) {
				  res.send('About birds');
				});
				
				module.exports = router;


5. 静态文件：
	- `app.use(express.static('[静态文件资源所在主路径]'))`,这时可以通过`http://localhost:3000/具体静态文件路径`
	- 通过`app.use('[虚拟路径]', express.static('[静态文件资源所在主路径]'));`来给访问路径添加一层虚拟路径
	- 例子：
		
			// 目录结构为
			  - app.js
			  public
			    img.png
			
			// app.js中写入
			  app.use(express.static('public'));
			
			这时通过'http://localhost:3000/img.png'可以访问到图片
			
			// 如果在app.js中写入
			  app.use('static', express.static('public'));
			
			这时通过'http://localhost:3000/static/img.png'可以访问到图片

6. 中间件：
	- 功能：
		- 执行任何代码
		- 修改请求和响应对象
		- 终结请求-响应循环
		- 调用堆栈中的下一个中间件
	- 应用级中间件：
		- `app.use()`
		- `app.METHOD()`
	- 路由级中间件：
		- 使用`router.use()`加载
	- 错误处理中间件：
		- 和其他中间件一样，只是多了一个参数：

				app.use(function(err, req, res, next) {
				  console.error(err.stack);
				  res.status(500).send('Something broke!');
				});
	
	- 内置中间件：
		- `express.static(root, [options])`，主要用于做静态资源托管的配置，具体配置见文档；
	- 第三方中间件：
		>通过使用第三方中间件从而为 Express 应用增加更多功能

			var express = require('express');
			var app = express();
			var cookieParser = require('cookie-parser');
			
			// 加载用于解析 cookie 的中间件
			app.use(cookieParser());





