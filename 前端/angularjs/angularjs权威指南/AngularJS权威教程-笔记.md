##自己理解anjs的实现逻辑：
> anjs就是一个应用，而且是由模块组成，其中应用本身也是一个大的模块；模块又分为指令和控制器，而anjs在每个模块上会绑定一个作用域，指令、控制器、作用域也就组成了anjs最主要的应用模型，用MVC的概念来套，M就是指令，用来封装要展示的内容（包括模板和一些交互方法），C就是控制器，用来定义一些事件，控制模板的展示内容，V可以说是作用域，在作用域内能够展示指令。

##数据绑定：

1. anjs采用创建实时模板来代替视图，而不是将数据合并进模板后更新DOM；
2. 只有被具有ng-app属性的DOM元素包含的元素才会受anjs影响；
3. anjs会记录数据模型所包含的数据在任何特定时间点的值，而不是原始值；当anjs认为某个值可能发生变化时，它会运行自己的脏检查事件（$digest()）来检测数据的变化，从而保持数据的一致性；
4. ng-model指令将数据模型对象（$scope）中的对应属性绑定到文本输入域中，从而可以更新和监听该属性的变化；
5. 由于js自身特点以及其在传递值和引用时的不同处理方式，通常认为在视图中通过对象的属性而非对象本身来进行引用绑定是anjs中的最佳实践；

##模块：

1. 模块是定义应用的最主要方式，一个应用可以包含多个模块，每一个模块都包含了定义具体功能的代码（白话：anjs定义了一个应用，应用由多个模块组成，每个模块实现特定的功能）；
2. 声明：angular.moudule('模块名',[被注入到模块中的对象列表，用‘，’隔开]);
3. 引用：angular.module('应用名')；

##作用域：

1. 概念：作用域（$scope）定义了模型的作用范围，其结构是与DOM类似的树形结构，内部绑定了模型中相关的数据，详细的$scope对象见下方笔记<anjs内置对象>；
2. 作用域对象($scope)生命周期：
	- 创建：创建controller或directive（例如ng-repeat）时，anjs会用$injector创建一个新的作用域，并在这个controller或directive运行时将作用域传递进去；
	- 链接：当anjs开始运行时，所有$scope对象都会附加或链接到视图中，这些作用域将会注册当anjs应用上下文中发生变化时需要运行的函数(这些函数被称为$watch函数)；
	- 更新：当事件循环运行时，每个$scope执行自己的脏值检测，如果检测到任意变化，$scope对象就会触发指定的回调函数；
	- 销毁：当一个$scope在视图中不再需要时，这个作用域将会清理和销毁自己，可以用$destory()手动销毁（尽管并不需要）；
	
##控制器：

>控制器可以将与一个独立视图相关的业务逻辑封装在一个独立的容器中。

##表达式：

1. anjs会对函数或表达式进行运算，表达式用{{ expression }}显示；
2. 表达式特性：
	- 所有表达式在其所属的作用域内部执行，并有访问本地$scope的权限；
	- 如果表达式发生错误，并不会抛出异常；
	- 表达式不支持流程控制（例如if/else）；
	- 表达式内可以接受过滤器和过滤器链；
	
##过滤器：

1. 在表达式中通过'|'来使用过滤器，格式：{{ 表达式 | 过滤器 }}；
2. 在js中使用$filter(过滤器)来过滤，例：$scope.name=$filter(过滤器)(过滤的内容原文)；
3. 内置过滤器：
	- currency：将数值格式化为货币格式；
	- data:'需要的格式'：将日期格式华为需要的格式，格式可选：medium/short/fullDate/longDate/mediumDate/shortDate/mediumTime/shortTime/yyyy/yy/y/MMMM/MMM/MM/M/dd/d/EEEE/EEE/HH/H/hh/h/mm/m/ss/s/sss/a/Z等；
	- fiter：从指定数组中选择一个子集，并生成一个新数组返回；
	- json：将一个JSON或js对象转换成字符串；
	- limitTo：根据传入的参数生成一个新的数组或字符串；
	- lowercase：将字符串转为小写；
	- number：将数字格式haunted成文本；
	- orderBy：用表达式对指定数组进行排序；
	- uppercase：将字符串转换成大写；

4. 自定义过滤器：

    angular.module('myfilter',[])
    .filter('过滤器名',function(){
    	return function(输入的字符串变量){
    		return 过滤后的内容；
    	}
    })

5. 表单验证：
	- 必填项：标签上填入required，例：
		<input type="text" required />
	- 最小长度：标签上使用指令ng-minleng="最小长度值"，例：
		<input type="text" ng-minlength="5" />
	- 最大长度：标签上使用指令ng-maxleng="最大长度值"，例：
		<input type="text" ng-maxlength="5" />
	- 模式匹配：标签上使用指令ng-pattern="/匹配规则/"，例：
		<input type="text" ng-pattern="[a-zA-Z]" />
	- 电子邮件：标签类型设置为email，例：
		<input type="email" name="email" ng-model="user.email" />；
	- 数字：标签类型设置为number，例：
		<input type="number" name="age" ng-model="user.age" />
	- URL：标签类型设置为url，例：
		<input type="url" name="homepage" ng-model="user.facebook_url" />
	- 自定义验证：通过指令；
	
6. js中可以访问DOM中表单的属性：

	- 所有属性：formName.inputFieldName.property
	- 未修改的表单：formName.inputFieldName.$pristine；
	- 修改过的表单：formName.inputFieldName.$dirty；
	- 合法的表单：formName.inputFieldName.$valid;
	- 不合法的表单：formName.inputFieldName.$invalid；
	- 错误：formName.inputfieldName.$error；
	
7. $parsers：

>表单验证数组，$parsers中的函数以流水线的形式逐个调用，例：
		
	angular.module('myApp')
	.directive('oneToTen', function() {
		return {
			require: '?ngModel',
			link: function(scope, ele, attrs, ngModel) {
				if (!ngModel) return;
				ngModel.$parsers.unshift(
					function(viewValue) {
						var i = parseInt(viewValue);
						if (i >= 0 && i < 10) {
							ngModel.$setValidity('oneToTen', true);
							return viewValue;
						} else {
							ngModel.$setValidity('oneToTen', false);
							return undefined;
						}
				});
			}
		};
	});

8. $formatters：
>当绑定的ngModel值发生了变化，并经过$parsers数组中解析器的处理后，这个值会被传递给$formatters流水线。同$parsers数组可以修改表单的合法性状态类似， $formatters中的函数也可以修改并格式化这些值，例：

	angular.module('myApp')
		.directive('oneToTen', function() {
			return {
				require: '?ngModel',
				link: function(scope, ele, attrs, ngModel) {
					if (!ngModel) return;
					ngModel.$formatters.unshift(function(v) {
					return $filter('number')(v);
				});
			}
		};
	});
			
##内置指令：
1. 布尔属性（基本与HTML中的对应属性相同）：
	- ng-disabled：只要属性中出现该属性，就会禁用该输入字段；
	- ng-readonly：只要属性中出现该属性，该输入框变成只读状态；
	- ng-checked：true或false，与html中的checked属性相同；
	- ng-selected：true或false，与html中的selected属性相同；
2. 类布尔属性：
	- ng-href：会等到插值生效后再执行点击链接行为；
	- ng-src：在表达式生效前不会加载图像；
3. 会自动创建子作用域的指令：
	- ng-app：anjs读取到该指令时，会自动创建$rootScope，该作用域是作用域链的起点，类似根目录一样的存在；
	- ng-contoller：在DOM元素上放置控制器，所有该DOM元素内的子元素都可以由该控制器控制，在创建控制器时，系统会自动创建一个继承于父级作用域的子作用域$scope；
	- ng-include：可以加载、编译并包含外部HTML片段到当前的应用中，创建ng-include时AngularJS会自动创建一个子作用域；
	- ng-switch：这个指令和ng-switch-when及on="propertyName"一起使用，同js中switch用法类似，详细应用百度；
	- ng-view：ng-view是由ngRoute模块提供的一个特殊指令，用来设置将被路由管理和放置在HTML中的视图的位置；
	- ng-if：可以在里面传入一个表达式，如果表达式为true则该DOM元素包含的内容显示，否则不显示；
	- ng-repeat：ng-repeat用来遍历一个集合或为集合中的每个元素生成一个模板实例，用法与forin类似，模板作用域中有一些独立的属性：
		- $index：遍历的进度（0...length-1）；
		- $first：当元素是遍历的第一个时值为true；
		- $middle：当元素处于第一个和最后元素之间时值为true；
		- $last：当元素是遍历的最后一个时值为true；
		- $even：当$index值是偶数时值为true；
		- $odd：当$index值是奇数时值为true；
	- ng-init：ng-init指令用来在指令被调用时设置内部作用域的初始状态；
	- {{ }}：{{ }}语法是AngularJS内置的模板语法，它会在内部$scope和视图之间创建绑定，它是ng-bind的简略形式；
	- ng-bind：ng-bind会在内部$scope和视图之间创建绑定；
	- ng-bind-template：同ng-bind指令类似， ng-bind-template用来在视图中绑定多个表达式；
	- ng-cloak：ng-cloak指令会将内部元素隐藏，直到路由调用对应的页面时才显示出来；
	- ng-model：ng-model指令用来将input、 select、 text area或自定义表单控件同包含它们的作用域中的属性进行绑定；
	- ng-show/ng-hide：ng-show和ng-hide根据所给表达式的值来显示或隐藏HTML元素；
	- ng-change：这个指令会在表单输入发生变化时计算给定表达式的值；
	- ng-form：同HTML中form表单同样功能，但是ng-form中支持嵌套；
	- ng-click：ng-click用来指定一个元素被点击时调用的方法或表达式；
	- ng-select：ng-select用来将数据同HTML的<select>元素进行绑定；
	- ng-submit：ng-submit用来将表达式同onsubmit事件进行绑定；
	- ng-class：使用ng-class 动态设置元素的类，方法是绑定一个代表所有需要添加的类的表达式；
	- ng-attr-(suffix)：AngularJS编译DOM时会查找花括号{{ some expression }}内的表达式。这些表达式会被自动注册到$watch服务中并更新到$digest循环中，成为它的一部分；

####指令详解：
>定义：可以把它简单的理解成在特定DOM元素上运行的函数，指令可以扩展这个元素的功能；

######指令注册格式：

	angular.module('myApp', [])
	.directive('myDirective', function() {
		return {
			restrict: String,
			priority: Number,
			terminal: Boolean,
			template: String or Template Function:
			function(tElement, tAttrs) (...},
			templateUrl: String,
			replace: Boolean or String,
			scope: Boolean or Object,
			transclude: Boolean,
			controller: String or
				function(scope, element, attrs, transclude, otherInjectables) { ... },
			controllerAs: String,
			require: String,
			link: 
				function(scope, iElement, iAttrs) { ... },
			compile: // 返回一个对象或连接函数，如下所示：
				function(tElement, tAttrs, transclude) {
					return {
						pre: function(scope, iElement, iAttrs, controller) { ... },
						post: function(scope, iElement, iAttrs, controller) { ... }
					}
				// 或者return function postLink(...) { ... }
				}
		};
	});

######指令参数详解：
- require：require参数可以被设置为字符串或数组，字符串代表另外一个指令的名字。 require会将控制器注入到其值所指定的指令中，并作为当前指令的链接函数的第四个参数，require参数的值可以用下面的前缀进行修饰，这会改变查找控制器时的行为：
	- ?：如果在当前指令中没有找到所需要的控制器，会将null作为传给link函数的第四个参数；
	- ^：如果添加了^前缀，指令会在上游的指令链中查找require参数所指定的控制器；
	- ?^：将前面两个选项的行为组合起来，我们可选择地加载需要的指令并在父指令链中进行查找；
	- 没有前缀：如果没有前缀，指令将会在自身所提供的控制器中进行查找，如果没有找到任何控制器（或具有指定名字的指令）就抛出一个错误；
- restrict：restrict是一个可选的参数，它告诉AngularJS这个指令在DOM中可以何种形式被声明，包括E(元素)、A(属性)、C(类名)、M(注释)，可以多种形式共用；
- priority：优先级参数可以被设置为一个数值，范围为0(默认)~1000，优先级高的指令会被先执行；
- terminal：这个参数用来告诉AngularJS停止运行当前元素上比本指令优先级低的指令；
- template：template参数是可选的，必须被设置为HTML文本或函数，函数可以接受参数(tElement和tAttrs)；
- replace：replace是一个可选参数，如果设置了这个参数，值必须为true，设置该属性后模板会替代当前元素，否则模板会当成当前元素的子元素插入；
- transclude：transclude是一个可选的参数。如果设置了，其值必须为true，它的默认值是false，该参数用来定义对子元素的处理是替换还是共存；
- scope：可以被设置为true或一个对象，当scope设置为true时，会从父作用域继承并创建一个新的作用域对象，如果直接设置一个对象，会创建一个隔离的作用域，与父作用域完全隔离，可以通过绑定策略与外层作用域进行数据交互，绑定策略包括：
	- @ (or @attr)：指令内部作用域可以使用外部作用域的变量；
	- = (or =attr)：通过=可以将本地作用域上的属性同父级作用域上的属性进行双向的数据绑定；
	- & (or &attr)：通过&符号可以对父级作用域进行绑定，以便在其中运行函数，意味着对这个值进行设置时会生成一个指向父级作用域的包装函数；
- controllerAs：controllerAs参数用来设置控制器的别名，可以以此为名来发布控制器，并且作用域可以访问controllerAs；
- controller：controller参数可以是一个字符串或一个函数，当设置为字符串时，会以字符串的值为名字，来查找注册在应用中的控制器的构造函数，如果是函数，则构建一个内联控制器，可传入以下参数：
	- $scope：与指令元素相关联的当前作用域；
	- $element：当前指令对应的元素；
	- $attrs：由当前元素的属性组成的对象；
	- $transclude：嵌入链接函数会与对应的嵌入作用域进行预绑定；

##AngularJS模块加载：
1. 配置：
- 概念：在模块的加载阶段， AngularJS会在提供者注册和配置的过程中对模块进行配置，在整个AngularJS的工作流中，这个阶段是唯一能够在应用启动前进行修改的部分，配置是按照代码顺序执行的，配置函数内只能注入提供者和常量；
- 格式：

	angular.module('myApp', [])
		.config(function($provide) {
		});

2. 运行块：
- 概念：和配置块不同，运行块在注入器创建之后被执行，它是所有AngularJS应用中第一个被执行的方法；
- 格式：

	angular.module('myApp', [])
		.run(function($rootScope, AuthService) {
			//运行块内容
		});

##多重视图和路由：

1. 概念：可以简单理解为通过URL实现了对视图的管理，普通URL是刷新整个页面，anjs路由只是更新其中一部分的视图；
2. 需要先创建一个布局模板，通过ng-view与一个DOM元素绑定即可；
3. 格式：

	angular.module('myApp', [])
		.config(['$routeProvider', function($routeProvider) {
			$routeProvider
			.when('/', {
				templateUrl: 'views/home.html',
				controller: 'HomeController'
			})
			//路径2
			.when('/login:paraname', {
				templateUrl: 'views/login.html',
				controller: 'LoginController',
				resolve: {
					'data': ['$http', function($http) {
						return $http.get('/api').then(
							function success(resp) { return response.data; },
							function error(reason) { return false; }
						);
					}];
				}
			})
			.otherwise({
				redirectTo: '/'
			});
		}]);

4. 参数解析：
- controller：如果配置对象中设置了controller属性，那么这个指定的控制器会与路由所创建的新作用域关联在一起；
- template/templateUrl：AngularJS会将配置对象中的HTML模板渲染到对应的具有ng-view指令的DOM元素中；
- resolve：如果设置了resolve属性， AngularJS会将列表中的元素都注入到控制器中,如果这些依赖是promise对象，它们在控制器加载以及- $routeChangeSuccess被触发之前，会被resolve并设置成一个值,在上面的例子中， resolve会发送一个$http请求，并将data的值替换为返回结果的值,列表中的键data会被注入到控制器中，所以在控制器中可以使用它;
- redirectTo：如果redirectTo属性的值是一个字符串，那么路径会被替换成这个值，并根据这个目标路径触发路由变化，如果redirectTo属性的值是一个函数，那么路径会被替换成函数的返回值，并根据这个目标路径触发路由变化；
- reloadOnSearch：如果reloadOnSearch选项被设置为true（默认），当$location.search()发生变化时会重新加载路由；
- $routeParams：如果我们在路由参数的前面加上:， AngularJS就会把它解析出来并传递给$routeParams，如上面例子中的路径2中的:paraname，只要在控制器中传入$routeParams就可以访问参数paraname；

5. $location服务：AngularJS提供了一个服务用以解析地址栏中的URL，并让你可以访问应用当前路径所对应的路由，$location服务没有刷新整个页面的能力，如果需要刷新整个页面，需要使用$window.location对象的href，$location服务包括以下内容：
- path()：path()用来获取页面当前的路径；
- replace()：如果你希望跳转后用户不能点击后退按钮（对于登录之后的跳转这种发生在某个跳转之后的再次跳转很有用）， AngularJS提供了replace()方法来实现这个功能；
- absUrl()：absUrl()方法用来获取编码后的完整URL；
- hash()：hash()方法用来获取URL中的hash片段；
- host()：host()方法用来获取URL中的主机；
- port()：	port()方法用来获取URL中的端口号；
- protocol()：protocol()方法用来获取URL中的协议；
- search()：我们可以向这个方法中传入新的查询参数，来修改URL中的查询串部分，search方法可以接受两个参数（新的查询参数，替代当前URL的参数）；
- url()：url()方法用来获取当前页面的URL，如果调用url()方法时传了参数，会设置并修改当前的URL，这会同时修改URL中的路径、查询串和hash，并返回$location；

6. 路由模式：
- 简介：不同的路由模式在浏览器的地址栏中会以不同的URL格式呈现。 $location服务默认会使用标签模式来进行路由；
- 模式：标签模式和HTML5模式（百度）；
- 路由事件：$route服务在路由过程中的每个阶段都会触发不同的事件，可以为这些不同的路由事件设置监听器并做出响应，事件包括：
	- $routeChangeStart：AngularJS在路由变化之前会广播$routeChangeStart事件，在这一步中，路由服务会开始加载路由变化所需要的所有依赖，并且模板和resolve键中的promise也会被resolve，参数（将要导航到的下一个URL，路由变化前的URL）；
	- $routeChangeSuccess：AngularJS会在路由的依赖被加载后广播$routeChangeSuccess事件，参数（原始的AngularJS evt对象，用户当前所处的路由，上一个路由（如果当前是第一个路由，则为undefined）；
	- $routeChangeError：AngularJS会在任何一个promise被拒绝或者失败时广播$routeChangeError事件，参数（当前路由的信息，上一个路由的信息，被拒绝的promise的错误信息）；
	- $routeUpdate：AngularJS在reloadOnSearch属性被设置为false的情况下，重新使用某个控制器的实例时，会广播$routeUpdate事件；

##依赖注入：
1. 概念：依赖注入是一种设计模式，它可以去除对依赖关系的硬编码，从而可以在运行时改变甚至移除依赖关系；
2. 注入方式：
- 推断式注入声明：injector.invoke(function($http, greeter) {});
- 显示式注入：（未搞懂）
- 行内注入声明：
	
	angular.module('myApp')
	.controller('MyController', ['$scope', 'greeter', function($scope, greeter) {
	}]);

3. $injecter API：
- annotate()：annotate()方法的返回值是一个由服务名称组成的数组，这些服务会在实例化时被注入到目标函数中，annotate()方法可以帮助$injector判断哪些服务会在函数被调用时注入进去，参数（函数或数组）；
- get()：get()方法返回一个服务的实例，可以接受一个参数（获取实例的名称）；
- has()：has()方法返回一个布尔值，在$injector能够从自己的注册列表中找到对应的服务时返回true，否则返回false，参数（想在注入器的注册列表中查询的服务名称）；
- instantiate()：instantiate()方法可以创建某个JavaScript类型的实例。它会通过new操作符调用构造函数，并将所有参数都传递给构造函数，参数（构造函数，对象）；
- invoke()：invoke()方法会调用方法并从$injector中添加方法参数，参数（调用的函数u，设置调用方法的this参数，这个可选参数提供另一种方式在函数被调用时传递参数名给该函数）；
		
服务：
1. 概念：服务提供了一种能在应用的整个生命周期内保持数据的方法，它能够在控制器之间进行通信，并且能保证数据的一致性；
2. 创建服务：

	- factory（）：
	
		angular.module('myApp.services', [])
		.factory('服务名', function() {
			var serviceInstance = {};
			// 我们的第一个服务
			return serviceInstance;
		});
		函数内可以传入参数，参数即该服务依赖的内
	
	- service()：使用service()可以注册一个支持构造函数的服务，它允许我们为服务对象注册一个构造函数，例：
	
		var Person = function($http) {
				this.getName = function() {
					return $http({ method: 'GET', url: '/api/user'});
				};
			};
			angular.service('personService', Person);
	
	- provider()：所有服务工厂都是由$provide服务创建的，$provide服务负责在运行时初始化这些提供者，所有创建服务的方法都构建在provider方法之上，provider()方法负责在$providerCache中注册服务，下面两种方法的作用完全一样，并且会创建同一个服务：
	
		angular.module('myApp')
		.factory('myService', function() {
			return {
				'username': 'auser'
			};
		})
		// 这与上面工厂的用法等价
		.provider('myService', {
			$get: function() {
				return {
					'username': 'auser'
				};
			}
		});
	
	- constant()：可以将一个已经存在的变量值注册为服务，并将其注入到应用的其他部分当中；	
	- value()：如果服务的$get方法返回的是一个常量，那就没要必要定义一个包含复杂功能的完整服务，可以通过value()函数方便地注册服务；
	- decorator()：$provide服务提供了在服务实例创建时对其进行拦截的功能，可以对服务进行扩展，或者用另外的内容完全代替它，$delegate是可以进行装饰的最原始的服务，为了装饰其他服务，需要将其注入进装饰器；






tips：
1. JavaScript对象要么是值复制要么是引用复制。字符串、数字和布尔型变量是值复制。数组、对象和函数则是引用复制。因此在父子作用域中，通过对象来进行数据绑定可以解决子作用域中的值无法传到父作用域中的问题；
2. 指令用驼峰法命名，在模板中调用时用'-'来区分单词；