类型：

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