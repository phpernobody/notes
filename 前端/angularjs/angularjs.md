学习网站：
	1、汇智网，比较基础：http://www.hubwiz.com/course/547c3e3b88dba0087c55b4e5/
	2、angular中文社区：http://www.angularjs.cn/
	3、慕课网：http://www.imooc.com/index/search?words=angular


概念：
	1、{{}}与ng-bind的区别：
		{{}}会先显示未解析的内容，之后才显示解析后的内容；ng-bind会直接显示解析后的内容。
	2、ng-app带属性值与不带属性值区别：
		ng-app带属性值，浏览器不会自动加载，不带属性值，会自动加载。
	3、mvvm（model-view-viewmode）：
	4、作用域：
	
	5、$scope：控制器的对象参数，把一个DOM元素连接到控制器上的对象，也指明了一个作用域，一般在声明一个控制器的时候会创建一个$scope，其作用域范围与DOM元素一一对应，如果没有创建$scope，则作用域范围为父级$scope；
		监听（数据->界面）：scope.$watch(watchExpression, listener, [objectEquality]);
		应用（界面->数据）：		
	6、$rootScope：最顶级的$scope；



	7、AngularJS内置jqLite（精简版JQ），如果不愿意使用，可以在引用AngularJS之前引入JQ，将自动把jaLite升级为JQ；
	8、AngularJS启动应用时，会将模板（包含指令的HTML文件）和指令实现（JS文件）拼装起来，最终形成试图输出；
	9、指令：指令分为两部分，一是框架自带的，另一部分是我们自己定义，类似于自己定义的标签；指令可以看做是一个API，可以重复调用。

	
	
过滤器：
	格式：
		{{ value | 过滤形式 }}
	currency：格式化数字为货币格式；
	filter：从数组项中选择一个子集；
		    例：
    			<div ng-app="" ng-init="friends = [
    					{name:'tom', age:16},
    					{name:'jerry', age:20}, 
    					{name:'garfield', age:22}]">
    					输入过滤:<input type="text" ng-model="name" >
    					<ul style="list-style-type:none">
    	   					<li>   姓名，年龄</li>
    					<li  ng-repeat="x in friends | filter:name">   
    	   					{{ x.name + ' , ' + x.age }}
    					</li>
    					</ul>
    			</div>
	lowercase：格式化字符串为小写；
	orderBy：根据某个表达式排列数组；
	uppercase：格式化字符串为大写；


注入依赖：
	概念：
		注入器：
			功能：1、集中存储所有的provider的配方；2、按需提供功能组件的实例；
		服务组件（provider）：一个服务提供者就是一个组件，组件是以js构造函数的形式封装；
		配方：名称和类的组合信息就是一个配方；

	应用：
		创建一个注入器：
			1、angular.injector(modules, [strictDi]);
			2、如果angular框架已经启动，可通过下面方法获得：
				var element = angular.element(dom_element);
				var injector = element.injector();
		通过注入器调用api：
			1、invoke():
				angular.injector(['ng']).invoke(function($http){
    				//do sth. with $http
				});
			2、get()：
				var my$http = angular.injector(['ng']).get('$http');
		注入方式：
			1、参数名注入（缺点：js代码压缩时可能$http可能会变，所以不推荐）
				//myfunc通过参数表声明这个函数依赖于"$http"服务
				var myfunc = function($http){
    				//do sth. with $http
				};
				injector.invoke(myfunc);//myfunc的定义将被转化为字符串进行参数名检查
			2、依赖数组注入
				//myfunc依赖于"$http"和"$compile"服务
				var myfunc = ["$http","$compile",function(p1,p2){
    				//do sth. with p1($http),p2($compile)
				}];
				injector.invoke(myfunc);
	
框架启动：
	angular.bootstrap(element, [modules], [config]);








angularJS开发指南笔记：

1、引导程序：
	a、自动初始化：
		angular在dom页面渲染完会通过ng-app指令找到应用程序，并顺序执行以下:
			1、载入和指令内容相关的模块；
			2、创建一个应用的“注入器（injector）”；
			3、以带ng-app指令的标签为根标签，创建$rootScope；
	b、手动初始化：
		用于在加载angular之前需要做一些处理的页面：
			angular.element(document).ready(function() {
	            angular.bootstrap(element, [modules], [config]);
			});

2、带angular的代码在客户端执行过程
	
	<!doctype html>
	<html ng-app>
	  <head>
	    <script src="http://code.angularjs.org/angular-1.1.0.min.js"></script>
	  </head>
	  <body>
	    <p ng-init=" name='World' ">Hello {{name}}!</p>
	  </body>
	</html>

	1、浏览器载入HTML，然后把它解析成DOM。
	2、浏览器载入angular.js脚本。
	3、AngularJS等到DOMContentLoaded事件触发。
	4、AngularJS寻找ng-app指令，这个指令指示了应用的边界。
	5、使用ng-app中指定的模块来配置注入器($injector)。
	6、注入器($injector)是用来创建“编译服务($compile service)”和“根作用域($rootScope)”的。
	7、编译服务($compile service)是用来编译DOM并把它链接到根作用域($rootScope)的。
	8、ng-init指令将“World”赋给作用域里的name这个变量。
	9、通过{{name}}的替换，整个表达式变成了“Hello World”。

3、angular和浏览器事件回路交互：
	1、浏览器的事件循环等待事件的触发。所谓事件包括用户的交互操作、定时事件、或者网络事件(服务器的响应)。
	2、事件触发后，回调会被执行。此时会进入Javascript上下文。通常回调可以用来修改DOM结构。
	3、一旦回调执行完毕，浏览器就会离开Javascript上下文，并且根据DOM的修改重新渲染视图。
		
	大部分情况下（如在控制器，服务中），$apply都已经被用来处理当前事件的相应指令执行过了。只有当你使用自定义的事件回调或者是使用第三方类库的回调时，才需要自己执行$apply。

4、指令



	
	


大漠穷球笔记：
指令
	1、指令：
		指令执行过程：加载阶段（加载angular.js，找到ng-app指令，确认边界）->编译阶段（遍历DOM，找到所有指令；根据指令代码中的tempale、replace、transclude转换DOM结构；如果存在compile函数则执行）->链接阶段（对每一条指令运行link函数；链接阶段执行，一般用来操作DOM、绑定事件监听器；）

	
			
		

	module的方法：
		run(function(){})：注册器加载完所有模块后执行一次
		