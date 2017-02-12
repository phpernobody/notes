##第一章
1. 异常简介
	java中所有异常都继承自Throwable，其包括：
	
	- Error：指系统出错，一般是虚拟机出错或线程死锁之类的错误；
	- Exception
		- RuntimeException(非检查异常)
			- NullPointerException(空指针异常)
			- ArrayIndexOutOfBoundsException(数组下标越界异常)
			- ClassCastException(类型转换异常)
			- ArithmeticExceptior(算术异常)
		- 检查异常，需要自己捕获处理异常
			- IOException(文件异常)
			- SQLException(SQL异常)
		
2. 使用try..catch..finally实现异常处理
	- 如果出现异常，程序会执行catch里面的代码；
	- catch可以接连使用，能够捕获多种错误；
	- catch里面一般会输出错误或记录log；
	- catch要先子类后父类；
	- finally用于最终执行的代码；
	- e.printStackTrace()方法可以打印出异常信息；

3. 异常抛出以及自定义异常抛出
	- throw：表明具体要抛出的异常这个动作，写在方法体里面；
	- throws：声明将要抛出的异常类型
		
			public void 方法名(参数列表) throws 异常列表 {
				//调用会抛出异常的方法或者:
				throw new Exception();
			}
	- 自定义异常
		
			class 自定义异常类 extends 异常类型{
			}

4. JAVA中的异常链

5. 异常处理总结
	- 采用逻辑去合理规避同时辅助try-catch处理
	- 在多重catch后面加一个catch(Exception)来处理可能遗漏的异常
	- 对于不确定的代码也可以加上try-catch处理潜在的异常
	- 尽量去处理异常，切记只是简单的调用printStackTrack()去打印输出
	- 尽量添加finally语句块去释放占用的资源

##第二章
1. 字符串	
	- 创建方法
		- String str = "string";
		- String str = new String();
		- String str = new String("string");
	- 字符串的不变性：String对象创建后则不能被修改，我们常见的修改字符串其实是在堆内存中重新创建了字符串对象再把原来的变量名指向新字符串对象；使用new String()即使内容相同也会创建一个新字符串对象；

2. String类常用方法
<table>
<tr style="background: #999"><td>方法</td><td>说明</td></tr>
<tr><td>int length()</td><td>返回当前字符串的长度</td></tr>
<tr><td>int indexOf(int ch)</td><td>查找ch字符在该字符串中第一次出现的位置</td></tr>
<tr><td>intindexOf(String str)</td><td>查找str子字符串在该字符串中第一次出现的地方</td></tr>
<tr><td>int lastIndexOf(int ch)</td><td>查找ch字符在该字符串中最后一次出现的位置</td></tr>
<tr><td>int lastIndexOf(String str)</td><td>查找str子字符串在该字符串中最后一次出现的位置</td></tr>
<tr><td>String substring(int beginIndex)</td><td>获取从beginIndex位置开始到结束的子字符串</td></tr>
<tr><td>String substring(int beginIndex, int endIndex)</td><td>获取从beginIndex位置开始到endIndex位置的子字符串</td></tr>
<tr><td>String trim()</td><td>返回去除了前后空格的字符串</td></tr>
<tr><td>boolean equals(Object obj)</td><td>将该字符串与指定对象比较，返回true或false</td></tr>
<tr><td>String toLowerCase()</td><td>将字符串转换为小写</td></tr>
<tr><td>String toUpperCase()</td><td>将字符串转换为大写</td></tr>
<tr><td>char charAt(int index)</td><td>获取字符串中指定位置的字符</td></tr>
<tr><td>String[] split(String regex, int limit)</td><td>将字符串分割为子字符串，返回字符串数组</td></tr>
<tr><td>byte[] getBytes()</td><td>将该字符串转换为byte数组</td></tr>
</table>

>'==='和equals()的区别：前者判断内存地址是否相同，即判断是否同一个对象，后者比较内容是否一致

3. Java种的StringBulider类
>当频繁操作字符串时，就会额外产生很多临时变量。使用 StringBuilder 或 StringBuffer 就可以避免这个问题。至于 StringBuilder 和StringBuffer ，它们基本相似，不同之处，StringBuffer 是线程安全的，而 StringBuilder 则没有实现线程安全功能，所以性能略高。因此一般情况下，如果需要创建一个内容可变的字符串对象，应优先考虑使用 StringBuilder 类。

声明变量方法：
	- StringBuilder stb = new StringBuilder()
	- StringBuilder stb = new StringBulider("ste")

<table>
<tr style="background: #999"><td>方法</td><td>说明</td></tr>
<tr><td>StringBuilder append(参数)</td><td>追加内容到当前StringBuilder对象的末尾</td></tr>
<tr><td>StringBuilderinsert(位置,参数)</td><td>将内容插入到StringBuilder对象那个的指定位置</td></tr>
<tr><td>String toString()</td><td>将StringBuilder对象转换为String对象</td></tr>
<tr><td>int length()</td><td>获取字符串的长度</td></tr>
</table>

##第三章
1. Java中的包装类
>基本数据类型( int、float、double、boolean、char )是不具备对象的特性的，比如基本类型不能调用方法、功能简单。为了让基本数据类型也具备对象的特性， Java 为每个基本数据类型都提供了一个包装类，这样我们就可以像操作对象那样来操作基本数据类型。

基本类型和包装类之间的对应关系
<table>
<tr style="background: #999"><td>基本类型</td><td>对应的包装类</td></tr>
<tr><td>byte</td><td>Byte</td></tr>
<tr><td>short</td><td>Short</td></tr>
<tr><td>int</td><td>Integer</td></tr>
<tr><td>long</td><td>Long</td></tr>
<tr><td>float</td><td>Float</td></tr>
<tr><td>double</td><td>Double</td></tr>
<tr><td>char</td><td>Character</td></tr>
<tr><td>boolean</td><td>Boolean</td></tr>
</table>

包装类主要提供了两大类法：

- 将本类型和其他基本类型进行转换的方法
- 将字符串和本类型及包装类互相转换的方法


2. Integer包装类
	- 构造方法
		- Integer(int value)：创建一个Integer对象，表示指定的int值
		- Integer(String s)：创建一个Integer对象，表示String参数所指的int值
	- 常用方法
		<table>
		<tr style="backgound: #999"><td>返回值</td><td>方法名</td><td>解释</td></tr>
		<tr><td>byte</td><td>byteValue()</td><td>将该Integer转为byte类型</td></tr>
		<tr><td>double</td><td>doubleValue()</td><td>转为double类型</td></tr>
		<tr><td>float</td><td>floatValue()</td><td>转为float类型</td></tr>
		<tr><td>int</td><td>intValue()</td><td>转为int类型</td></tr>
		<tr><td>long</td><td>longValue()</td><td>转为long类型</td></tr>
		<tr><td>static int</td><td>parseInt(String s)</td><td>将字符串转换为int类型</td></tr>
		<tr><td>String</td><td>toString()</td><td>转为字符串类型</td></tr>
		<tr><td>static Integer</td><td>valueOf(String s)</td><td>将字符串转换为Integer类型</td></tr>
		</table>

3. Java中基本类和包装类之间的转换
	- 基本类型和包装类之间经常需要互相转换，在 JDK1.5 引入自动装箱和拆箱的机制，使得包装类和基本类型之间的转换更加轻松便利
	- 装箱：把基本类型转换成包装类，使其具有对象的性质，又可分为手动装箱和自动装箱，例子：
			
			int i = 10;// 定义一个int基本类型值
			Integer x = new Integer(i);//手动装箱
			Integer y = i;//自动装箱

	- 拆箱：和装箱相反，把包装类对象转换成基本类型的值，又可分为手动拆箱和自动拆箱，例子：

			Integer j = new Integer(8); //定义一个Integer包装类对象,值为8
			int m = j.intValue(); //手动拆箱int类型
			int n = j; //自动拆箱int类型

4. 基本类型和字符串之间的转换
	- 本类型转换为字符串的方法
		- 使用包装类toString()方法
		- 使用String类的valueOf()方法
		- 用一个空字符串加上基本类型，得到的就是基本类型数据对应的字符串
	- 将字符串转换成基本类型的方法
		- 调用包装类的 parseXxx 静态方法
		-  调用包装类的 valueOf() 方法转换为基本类型的包装类，会自动拆箱

5. Java中的Date类包
	- 使用 Date 类的默认无参构造方法创建出的对象就代表当前时间
		- Date d = new Date() //使用默认的构造方法创建Date对象
	- 使用format()方法将日期转换为指定格式的文本
	
			Date d = new Date();
			SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
			String today = sdf.format(d);
	- 使用parse()方法将文本转换为日期

			String day = "2014年02月14日 10:30:25";
			SimpleDateFormat df = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");
			Date date = df.parse(day);

6. Calendar 类的应用
>Date 类最主要的作用就是获得当前时间，同时这个类里面也具有设置时间以及一些其他的功能，但是由于本身设计的问题，这些方法却遭到众多批评，不建议使用，更推荐使用 Calendar 类进行时间和日期的处理。

>ava.util.Calendar 类是一个抽象类，可以通过调用 getInstance() 静态方法获取一个 Calendar 对象，此对象已由当前日期时间初始化，即默认代表当前时间，如 Calendar c = Calendar.getInstance();

- 例子：

		Calendar c = Calendar.getInstance(); //创建爱你Calendar对象
		int year = c.get(Calendar.YEAR); //获取年
		int month = c.get(Calendar.MONTH) + 1; //获取月份，0表示1月份
		int day = c.get(Calendar.DAY_OF_MONTH); //获取日期
		int hour = c.get(Calendar.HOUR_OF_DAY); //获取小时
		int minute = c.get(Calendar.MINUTE); //获取分钟
		int second = c.get(Calendar.SECOND); //获取秒
		Date date = c.getTime(); // 将Calendar对象转换为Date对象
		Long time = c.getTimeInMillis(); //获取当前毫秒数（时间戳）


7. 使用 Math 类操作数据
>Math 类位于 java.lang 包中，包含用于执行基本数学运算的方法， Math 类的所有方法都是静态方法，所以使用该类中的方法时，可以直接使用类名.方法名，如： Math.round();

常用的方法

<table>
<tr style="background: #999"><td>返回值</td><td>方法名</td><td>解释</td></tr>
<tr><td>long</td><td>round()</td><td>返回四舍五入后的整数</td></tr>
<tr><td>double</td><td>floor()</td><td>返回小于参数的最大整数</td></tr>
<tr><td>double</td><td>ceil()</td><td>返回大于参数的最小整数</td></tr>
<tr><td>double</td><td>random()</td><td>返回[0,1]之间的随机数浮点数</td></tr>
</table>
 
##第四章Java中的集合框架
1. 集合的概念：是一种工具类，就像是容器，储存任意数量的具有共同属性的对象

2. Java中的集合框架
	- Collection
		- List
			- ArrayList
		- Queue
			- LinkedList
		- Set
			- HashSet
	- Map(<KEY, value>形式存储的)
		- HashMap

3. Collection接口、子接口以及其实现类
	- List接口及其实现类--ArrayList
		- List是元素有序并且可以重复的集合，被称为序列
		- List可以精确的控制每个元素的插入位置，或删除某个位置元素
		- ArrayList--数组序列，是List的一个重要实现类
		- ArrayList底层是由数组实现的
	- Set接口及其实现类--HashSet
		- Set是元素无序并且不可以重复的集合，被称为集
		- HashSet--哈希集，是Set的一个重要实现类

4. Map和HashMap
	- Map接口
		- Map提供了一种映射关系，其中的元素时以键值对(key-value)的形式存储的，能够实现根据key快速查找value
		- Map中的键值对以Entry类型的对象实例形式存在
		- Key值不可重复，value值可以
		- 每个键最多只能映射到一个值
		- Map接口提供了分别返回key值集合、value值集合以及Entry(键值对)集合的方法
		- Map支持泛型，形式如：Map<K,V>
	- HashMap类
		- HashMap是Map的一个重要实现类，也是最常用的，基于哈希表实现
		- HashMap中的Entry对象是无序排列的
		- Key值和value值都可以为null，但是一个HashMap只能有一个key值为null的映射(key值不可重复)