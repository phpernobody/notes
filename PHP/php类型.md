## 类型：

1. boolean
	- 会被当做false的情况
		- 布尔值`FALSE`本身
		- 整型值`0`
		- 浮点型`0.0`
		- 空字符串，以及字符串`"0"`
		- 不包括任何元素的数组
		- 不包括任何成员变量的对象(进PHP4.0适用)
		- 特殊类型`NULL`
		- 从空标记生成的`SimpleXML对象`

2. integer
	- 不同表现形式

			$a = 1234; // 十进制数
			$a = -123; // 负数
			$a = 0123; // 八进制数 (等于十进制 83)
			$a = 0x1A; // 十六进制数 (等于十进制 26)
	- 相关常量
		- 字长：PHP_INT_SIZE；最大值：PHP_INT_MAX
	- 强转：intval()或(int) number，浮点型强转整形会向下取整
	- 溢出：如果给定的一个数超出了 integer 的范围，将会被解释为 float；同样如果执行的运算结果超出了 integer 范围，也会返回 float

3. float
	- PHP 中的 string 的实现方式是一个由字节组成的数组再加上一个整数指明缓冲区长度
	- 浮点数是不精确的，永远不要拿浮点数来做比较；
	- 数学运算产生常量NAN代表着任何不同值，用来与任何值比较都是false，应该用is_nan()函数来判断
	- 字符串会被按照该脚本文件相同的编码方式来编码；当激活了 Zend Multibyte，此时脚本可以是以任何方式编码
	- 表达方式
		- ''(单引号)：不像双引号和 heredoc 语法结构，在单引号字符串中的变量和特殊字符的转义序列将不会被替换
		- ""(双引号)：如果字符串是包围在双引号（"）中， PHP 将对一些特殊的字符进行解析
		- <<<(Herdoc结构)：

				$str = <<<"EOD"
				Example of string
				spanning multiple lines
				using heredoc syntax.
				EOD;
			结束时所引用的标识符必须在该行的第一列
		- <<<(Nowdoc结构)：同Herdoc结构类似，但是字符串中不进行解析操作

				$str = <<<'EOD'
				Example of string
				spanning multiple lines
				using heredoc syntax.
				EOD;	
	- 变量解析
		- 当 PHP 解析器遇到一个美元符号（$）时，它会和其它很多解析器一样，去组合尽量多的标识以形成一个合法的变量名。可以用花括号来明确变量名的界线
		- 由于 { 无法被转义，只有 $ 紧挨着 { 时才会被识别。可以用 {\$ 来表达 {$
		- 函数、方法、静态类变量和类常量只有在 PHP 5 以后才可在 {$} 中使用。然而，只有在该字符串被定义的命名空间中才可以将其值作为变量名来访问。只单一使用花括号 ({}) 无法处理从函数或方法的返回值或者类常量以及类静态变量的值
	- 字符串连接：`.`
	- 强转字符串：`strval`或`(string)`，数组会被转成Array，对象会被转成Object，资源会被转成resource，这些要通过print_r()或var_dump()来列出这些类型的内容
	- 字符串序列化：通过serialize()

4. array
	- 定义：

			array(
				key => value,
				...
			)
		php5.4起可以用`[]`代替`()`
	- 遍历：foreach($arr as $item)
	- 比较：array_diff()
	- 去除数组元素：unset(key)

5. object	
	- 如果将一个对象转换成对象，它将不会有任何变化。如果其它任何类型的值被转换成对象，将会创建一个内置类 stdClass 的实例，例子：

			<?php
			$obj = (object) 'ciao';
			echo $obj->scalar;  // outputs 'ciao'
			?>

6. NULL
	- 产生：
		- 被赋值为 NULL
		- 尚未被赋值
		- 被 unset()
	- 强转：`(unset)`

## 变量
>PHP 中的变量用一个美元符号后面跟变量名来表示。变量名是区分大小写的。,一个有效的变量名由字母或者下划线开头，后面跟上任意数量的字母，数字，或者下划线

#### 普通赋值
>变量默认总是传值赋值。那也就是说，当将一个表达式的值赋予一个变量时，整个原始表达式的值被赋值到目标变量。这意味着，例如，当一个变量的值赋予另外一个变量时，改变其中一个变量的值，将不会影响到另外一个变量；

#### 引用赋值
>新的变量简单的引用（换言之，“成为其别名” 或者 “指向”）了原始变量。改动新的变量将影响到原始变量;使用引用赋值，简单地将一个 & 符号加到将要赋值的变量前（源变量），例子

	<?php
		$foo = 'Bob';              // 将 'Bob' 赋给 $foo
		$bar = &$foo;              // 通过 $bar 引用 $foo
		$bar = "My name is $bar";  // 修改 $bar 变量
		echo $bar;
		echo $foo;                 // $foo 的值也被修改
	?>

#### 变量范围
>与C和js不同，php中的变量不能被函数直接使用，要通过global关键字关联

###### 预定义变量
- 超全局变量
	- $GLOBALS
	- $_SERVER
	- $_GET
	- $_POST
	- $_FILES
	- $_COOKIE
	- $_SESSION
	- $_REQUEST
	- $_ENV

###### global 关键字
例子1：

	<?php
		$a = 1;
		$b = 2;
		
		function Sum()
		{
		    global $a, $b;
		
		    $b = $a + $b;
		}
		
		Sum();
		echo $b;
	?>

例子2：
>$GLOBALS 是一个关联数组，每一个变量为一个元素，键名对应变量名，值对应变量的内容。$GLOBALS 之所以在全局范围内存在，是因为 $GLOBALS 是一个超全局变量。

	<?php
		$a = 1;
		$b = 2;
		
		function Sum()
		{
		    $GLOBALS['b'] = $GLOBALS['a'] + $GLOBALS['b'];
		}
		
		Sum();
		echo $b;
	?>

###### 静态变量
>静态变量仅在局部函数域中存在，但当程序执行离开此作用域时，其值并不丢失.

例子：
	<?php
		function test()
		{
		    static $a = 0;
		    echo $a;
		    $a++;
		}
	?>

###### 可变变量
>一个可变变量可以获取一个普通变量的值作为这个可变变量的变量名。

例子：
	<?php
		$$a = 'world';
	?>

tips：

- 要将可变变量用于数组，必须解决一个模棱两可的问题。这就是当写下 $$a[1] 时，解析器需要知道是想要 $a[1] 作为一个变量呢，还是想要 $$a 作为一个变量并取出该变量中索引为 [1] 的值。解决此问题的语法是，对第一种情况用 ${$a[1]}，对第二种情况用 ${$a}[1]
- 类的属性也可以通过可变属性名来访问。可变属性名将在该调用所处的范围内被解析。例如，对于 $foo->$bar 表达式，则会在本地范围来解析 $bar 并且其值将被用于 $foo 的属性名。对于 $bar 是数组单元时也是一样

#### HTTP Cookies
>bool setcookie ( string $name [, string $value = "" [, int $expire = 0 [, string $path = "" [, string $domain = "" [, bool $secure = false [, bool $httponly = false ]]]]]] )
>[传送门](http://php.net/manual/en/function.setcookie.php)

例子：

	<?php
	  setcookie("MyCookie[foo]", 'Testing 1', time()+3600);
	  setcookie("MyCookie[bar]", 'Testing 2', time()+3600);
	?>

## 常量
>常量是一个简单值的标识符（名字）,在脚本执行期间该值不能改变.

#### 语法
>使用define()或const来定义常量

例子1:

	<?php
		define("CONSTANT", "Hello world.");
		echo CONSTANT; // outputs "Hello world."
		echo Constant; // 输出 "Constant" 并发出一个提示级别错误信息
	?>

例子2：

	<?php
		// 以下代码在 PHP 5.3.0 后可以正常工作
		const CONSTANT = 'Hello World';
		
		echo CONSTANT;
	?>

**tips**：常量和（全局）变量在不同的名字空间中。这意味着例如 TRUE 和 $TRUE 是不同的。

#### 魔术常量
>PHP 向它运行的任何脚本提供了大量的预定义常量。不过很多常量都是由不同的扩展库定义的，只有在加载了这些扩展库时才会出现，或者动态加载后，或者在编译时已经包括进去了。

- `__LINE__`：文件中的当前行号
- `__FILE__`：文件的完整路径和文件名。如果用在被包含文件中，则返回被包含的文件名。自 PHP 4.0.2 起，__FILE__ 总是包含一个绝对路径（如果是符号连接，则是解析后的绝对路径），而在此之前的版本有时会包含一个相对路径
- `__DIR__`：文件所在的目录。如果用在被包括文件中，则返回被包括的文件所在的目录。它等价于 dirname(__FILE__)。除非是根目录，否则目录中名不包括末尾的斜杠。（PHP 5.3.0中新增）
- `__FUNCTION__`：函数名称（PHP 4.3.0 新加）。自 PHP 5 起本常量返回该函数被定义时的名字（区分大小写）。在 PHP 4 中该值总是小写字母的
- `__CLASS__`：类的名称（PHP 4.3.0 新加）。自 PHP 5 起本常量返回该类被定义时的名字（区分大小写）。在 PHP 4 中该值总是小写字母的。类名包括其被声明的作用区域（例如 Foo\Bar）。注意自 PHP 5.4 起 __CLASS__ 对 trait 也起作用。当用在 trait 方法中时，__CLASS__ 是调用 trait 方法的类的名字
- `__TRAIT__`：Trait 的名字（PHP 5.4.0 新加）。自 PHP 5.4 起此常量返回 trait 被定义时的名字（区分大小写）。Trait 名包括其被声明的作用区域（例如 Foo\Bar）
- `__METHOD__`：类的方法名（PHP 5.0.0 新加）。返回该方法被定义时的名字（区分大小写）
- `__NAMESPACE__`：当前命名空间的名称（区分大小写）。此常量是在编译时定义的（PHP 5.3.0 新增）


## 函数

#### 函数的定义:

函数无需在调用之前被定义，除非是下面两个例子中函数是有条件被定义时，当一个函数是有条件被定义时，必须在调用函数之前定义。

PHP可以调用函数中的函数，例如：

	<?php
		function foo()
		{
		  function bar()
		  {
		    echo "I don't exist until foo() is called.\n";
		  }
		}
		
		/* 现在还不能调用bar()函数，因为它还不存在 */
		
		foo();
		
		/* 现在可以调用bar()函数了，因为foo()函数
		   的执行使得bar()函数变为已定义的函数 */
		
		bar();
	
	?>

#### 传参
>默认情况下，函数参数通过值传递（因而即使在函数内部改变参数的值，它并不会改变函数外部的值）。如果希望允许函数修改它的参数值，必须通过引用传递参数;如果想要函数的一个参数总是通过引用传递，可以在函数定义中该参数的前面加上符号 &：

###### 引用传参
例子：

	<?php
		function add_some_extra(&$string)
		{
		    $string .= 'and something extra.';
		}
		$str = 'This is a string, ';
		add_some_extra($str);
		echo $str;    // outputs 'This is a string, and something extra.'
	?>

###### 参数默认值
>函数可以定义 C++ 风格的标量参数默认值,默认值必须是常量表达式，不能是诸如变量，类成员，或者函数调用等。

<?php
	function makeyogurt($flavour, $type = "acidophilus")
	{
	    return "Making a bowl of $type $flavour.\n";
	}
	
	echo makeyogurt("raspberry");   // works as expected
?>

#### 返回值
>值通过使用可选的返回语句返回。可以返回包括数组和对象的任意类型。返回语句会立即中止函数的运行，并且将控制权交回调用该函数的代码行

#### 可变函数
>PHP 支持可变函数的概念。这意味着如果一个变量名后有圆括号，PHP 将寻找与变量的值同名的函数，并且尝试执行它。可变函数可以用来实现包括回调函数，函数表在内的一些用途。

	<?php
		function foo() {
		    echo "In foo()<br />\n";
		}
		
		function bar($arg = '') {
		    echo "In bar(); argument was '$arg'.<br />\n";
		}
		
		// 使用 echo 的包装函数
		function echoit($string)
		{
		    echo $string;
		}
		
		$func = 'foo';
		$func();        // This calls foo()
		
		$func = 'bar';
		$func('test');  // This calls bar()
		
		$func = 'echoit';
		$func('test');  // This calls echoit()
	?>

#### 匿名函数
>匿名函数（Anonymous functions），也叫闭包函数（closures），允许 临时创建一个没有指定名称的函数。最经常用作回调函数（callback）参数的值。

## 类和对象

#### 类

###### 定义
>个类的定义都以关键字 class 开头，后面跟着类名，后面跟着一对花括号，里面包含有类的属性与方法的定义。

例子：
	
	<?php
		class SimpleClass
		{
		    // property declaration
		    public $var = 'a default value';
		
		    // method declaration
		    public function displayVar() {
		        echo $this->var;
		    }
		}
	?>

###### 创建类的实例
>要创建一个类的实例，必须使用 new 关键字.

例子：

	<?php
	  class SimpleClass
	  {
	    public function dis() {
	      echo 'SimpleClass';
	    }
	  }
	
	  $SimpleClass = new SimpleClass();
	  $SimpleClass->dis();
	 ?>


###### 获取类名
>可以通过`::class`获取类名

例子： 

	ClassName::class;












