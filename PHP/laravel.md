## 项目搭建



#### 安装laravel
1. 通过laravel安装工具安装
	- `composer global require "laravel/installer"`
	- `laravel new`

2. 通过Composer Create-Project

`composer create-project laravel/laravel your-project-name --prefer-dist "5.1.*"`


## 服务容器和服务提供者

#### 参考
[龙哥的redmine](http://192.168.45.4:7081/redmine/issues/7104)
[ 简单理解laravel框架中的服务容器，服务提供者以及怎样调用服务](http://blog.csdn.net/xiake2014/article/details/52935364)

#### 服务容器
程序中任何可以用到的东西都称为服务，如:类、实例、文件路径等，服务容器就是用来装这些服务

#### 服务绑定
服务容器生成后，需要向其中装服务，也就是所谓的绑定，简单理解就是一个服务和一个关键字绑定

绑定分为：

1. 回调函数服务绑定
- 普通绑定

	$this->app->bind('HelpSpot\API', function ($app) {
	    return new HelpSpot\API($app->make('HttpClient'));
	});

- 单例绑定

	$this->app->singleton('HelpSpot\API', function ($app) {
	    return new HelpSpot\API($app->make('HttpClient'));
	});
- 情境绑定（该方法可以多个类实现同一个接口）

	$this->app->when('Namespace\To\Your\Class\A')
	          ->needs('Namespace\To\Your\Interface\Pay')
	          ->give('Namespace\To\Your\Class\Weixin');
	          
	$this->app->when('Namespace\To\Your\Class\B')
	          ->needs('Namespace\To\Your\Interface\Pay')
	          ->give('Namespace\To\Your\Class\Ali');

2. 实例对象服务绑定

	$api = new HelpSpot\API(new HttpClient);
	$this->app->instance('HelpSpot\Api', $api);



#### 服务解析

服务绑定后，可以随时通过服务解析来获取

服务解析分为两个步骤：
1. 获取服务容器对象，通常为$app属性，也可以通过Facades中的App外观或app()全局函数来获取
2. 通过服务容器实现对服务的解析

实际例子（实际中$this所代表的对象好像与案例的不一致，所以没试验成功）

1. $general = app(App\ServiceTest\GeneralService::class);
2. $general = $this->app[App\ServiceTest\GeneralService::class];
3. $general = $this->app->make(App\ServiceTest\GeneralService::class);
4. $general = \App::make(App\ServiceTest\GeneralService::class);
5. 在引用的方法中注入`App\ServiceTest\GeneralService $general`
6. 通过情景绑定





#### 服务提供者
提供业务服务给其他模块调用，服务提供者需要在`config/app.php`中providers里面注册后才能调用

#### 个人理解
服务容器就是一个写了各种方法的类，服务提供者就是类的管理员，哪些服务可以调用需要服务提供者这边去设置，服务需要在初始化时被注册(config/app.php)才能被使用，而其他控制器在调用的时候直接通过注入服务容器就可以调用到里面的方法




## Route
#### 基本几个路由方法
- Route::get($uri, $callback);
- Route::post($uri, $callback);
- Route::put($uri, $callback);
- Route::patch($uri, $callback);
- Route::delete($uri, $callback);
- Route::options($uri, $callback);
- Route::match([$method1,$method2],[$routeName] , $callback);// 同一个路由响应不同请求方法
- Route::any($uri, $callback); // 同一个路由响应所有的请求方法

###### 传参
>`?`可以指定参数是否必须，`$name`可以给定默认值

    Route::get('user/{name?}', function ($name = null) {
        return $name;
    });


###### 单独设置参数验证条件

	Route::get('user/{id}/{name}', function ($id, $name) {
	    //
	})->where(['id' => '[0-9]+', 'name' => '[a-z]+']);

###### 为所有的http请求设置过滤条件
	// 文件路径 providers/RouteServiceProvider
	public function boot(Router $router)
	{
	    $router->pattern('id', '[0-9]+');
	
	    parent::boot($router);
	}

#### 命名路由

	// 方式1
	Route::get('user/profile', 'UserController@showProfile')->name('profile');
	
	// 方式2
	Route::get('user/profile', ['as' => 'profile', function () {
	    //
	}]);

	// 方式3
	Route::get('user/profile', [
	    'as' => 'profile', 'uses' => 'UserController@showProfile'
	]);

###### 组群与命名路由

	Route::group(['as' => 'admin::'], function () {
	    Route::get('dashboard', ['as' => 'dashboard', function () {
	        // Route named "admin::dashboard"
	    }]);
	});

###### 命名路由的使用

	// Generating URLs...
	$url = route('profile');
	
	// Generating Redirects...
	return redirect()->route('profile');

#### 路由组群

###### 中间件

	Route::group(['middleware' => 'auth'], function () {
	    Route::get('/', function ()    {
	        // Uses Auth Middleware
	    });
	
	    Route::get('user/profile', function () {
	        // Uses Auth Middleware
	    });
	});

###### 命名空间
>为一组路由设一个命名空间，通过命名空间可为控制器(controller)分组

	Route::group(['namespace' => 'Admin'], function()
	{
	    // Controllers Within The "App\Http\Controllers\Admin" Namespace
	
	    Route::group(['namespace' => 'User'], function() {
	        // Controllers Within The "App\Http\Controllers\Admin\User" Namespace
	    });
	});

#### 子域名路由

	Route::group(['domain' => '{account}.myapp.com'], function () {
	    Route::get('user/{id}', function ($account, $id) {
	        //
	    });
	});

#### 前缀路由
>以下的路由，访问时要在users前面加上admin/

	Route::group(['prefix' => 'admin'], function () {
	    Route::get('users', function ()    {
	        // Matches The "/admin/users" URL
	    });
	});


## 控制器
>控制器路径为：App/Htpp/Controllers/，一般控制器会先建一个命名空间，命名空间和路径一致

#### 通过路由与控制器关联

	// 方法1
	Route::any('user', ['uses' => 'UserController@method']);
	// 方法2
	Route::any('user', 'UserController@method');
	// 方法3，取别名
	Route::any('user', ['uses' => 'UserController@method', 'as' => 'user']);


## 数据库

#### DB facade(原始查找)

###### 连接数据库

- 配置文件：config/database.php
- 对应的环境变量：.env

###### 实现CURD
>需要引入：use Illuminate\Support\Facades\DB
- 搜索：`DB::select('select * from table_name');`
- 插入：`DB::insert('insert into table(colum1,colum2) values(?,?)', [val1, val2]);`
- 更新：`DB::update('update table set column1=? where column2=?', [val1, val2]);`
- 删除：`DB::delete('delete from table where column=?',[val]);`


#### 查询构造器

###### 插入
	
	// 插入数据，默认返回bool
	$bool = DB::table('student')->insert(['name'=>'xiaoming', 'age'=>18]);
	// 插入数据，返回id
	$id = DB::table('student')->insertGetId(['name'=>'xiaoming', 'age'=>18]);
	// 插入多条数据
	$id = DB::table('student')->insertGetId([
		'name1'=>'xiaoming', 'age'=>18,
		'name2'=>'daming', 'age'=>19
		]);

###### 更新
	// 更新数据
	DB::table('student')->where(['id', 12])->update(['age', 30]);
	// 自增，num表示自增的数，自减同理，只是increment改为decrement
	DB::table('student')->where('id', 2)->increment('age', num);
	// 自增且修改其他字段
	DB::table('student')->where('id', 3)->increment('age', num, ['name'=>'xiaohong']);

###### 删除
	// 删除id=3的数据
	DB::table('student')->where('id', 2)->delete();
	// 删除id>3的数据
	DB::table('student')->where('id', '>', 2)->delete();
	// 删除整个数据表
	DB::table('student')->truncate();

###### 查询

	// 获取全部数据
	DB::table('student')->get();
	// 获取第一条数据
	DB::table('student')->first();
	// 排序
	DB::table('student')->orderBy('id','desc')->get();
	// 条件查询
	DB::table('student')->where('id','>=',2)->get();
	// 多条件查询
	DB::table('student')->whereRaw('id > ? and age > ?', [21, 12])->get();
	// 返回指定字段,lists可以指定下标，例子中的id就是name的下标
	DB::table('student')->pluck('name');
	DB::table('student')->lists('name', 'id');
	// 返回指定的查询字段
	DB::table('student')->select('id', 'name', 'age')->get();
	// 分段查询，`return false;`可以停止查询
	DB::table('student')->chunk(2, function($students) {
		//
	});

###### 聚合函数

- count()
- max()
- sum()
- min()
- avg()

#### Eloquent ORM

###### 建立模型

// 模型表
namespace App

use Illuinate\Database\Eloquent\Model;

class Student extends Model
{
	// 指定表名
	protected $table = 'student';
	// 指定主键
	protected $primaryKey = 'id';
	// 自动维护时间戳
	public $timestamps = true;
	// 指定允许批量赋值的字段
	protected $fillable = ['name', 'age'];
	// 指定不允许批量赋值的字段
	protected $guarded = [];

	// 时间格式化
	protected function getDateFormat()
	{	
		return time();
	}
	// 不格式化
	protected function asDateTime($val)
	{
		return $val;
	}
}

###### 查询数据 
	// 控制器中
	
	// 获取模型所有数据
	Student::all();
	Student::get();
	// 获取指定id的数据
	Student::find(1001);
	// 如果查询不到数据抛出错误
	Student::findOrFail(1001);
	// 条件查询，其他的查询参考上面的，也支持chunk和聚合函数等
	Student::where('id', 1001)->get();


###### 新增数据
	// 控制器中
	
	// 使用模型新增数据
	$student = new Student();
	$student->name = 'sean';
	$student->age = 18;
	$student->save();
	
	// 使用create()新增数据
	Student::create(['name'=>'xiaoming', 'age'=>12]);
	// 以属性查找数据，如果没有就创建新的数据
	Student::firstOrCreate(['name'=>'xiaoming']);
	// 以属性查找数据，如果没有就创建新的实例，通过save()保存
	Student::firstOrNew(['name'=>'xiaoming']);


#### 事务
> Eloquent并没有封装事务，所以也是用DB提供的方法实现

```
    DB::beginTransaction();
    try {
		// 多条要执行的数据库语句
        DB::commit();
    } catch (Exception $e){
       DB::rollback();
       throw $e;
    }

```

## Eloquent 模型约定
#### 模型建立
模板：

```
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    /**
     * 与模型关联的数据表
     * @var string
     */
    protected $table = 'user';

    /**
     * created_at和updated_at字段是否自动维护
     * true：自动维护；false：手动维护；默认true
     * @var bool
     */
    public $timestamps = true;

    /**
     * 模型的日期字段保存格式。
     * @var string
     */
    protected $dateFormat = 'U';

    /**
     * 此模型的连接名称，连接配置在config/database.php
     * @var string
     */
    protected $connection = 'mysql';
    
}
```

#### 模型调用
```
// 引入
use App\Models\User;

// 模型操作
User::all()
```

#### 模型关联

######　一对一




## 实践

####　获取xml格式
[参考](https://ninghao.net/blog/1442)





