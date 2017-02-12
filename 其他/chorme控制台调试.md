##chorme控制台调试

1. console.log()
	- 最常用的输出方式，可以打印出各种数据类型
	
2. console.info()
	- 信息输出，格式与1相同，显示样式不同
	
3. console.debug()
	- 信息输出，格式与1相同，显示样式不同

4. console.warn()
	- 信息输出，格式与1相同，显示样式不同

5. console.error()
	- 信息输出，格式与1相同，显示样式不同

6. console.table()
	- 二维以内的数据会被处理成表单格式

7. console.time()和console.timeEnd()
	- 获得程序执行的时间，参数需要一致

8. console.assert()
	- 当参数中的表达式为false时，程序会中断，输出错误信息

9. console.count()
	- 输出循环的次数

			function func() {
			  console.count('label');
			}
			
			for(let i = 0; i < 3; i++) {
			  func();
			}
			
			resoult:
			
			label: 1
			label: 2
			label: 3


10. console.group()、console.groupEnd()、console.groupCollapsed()
	- 输出具有层级关系的数据

			console.group('1');
			console.log('1-1');
			console.log('1-2');
			console.log('1-3');
			console.groupEnd();
			console.group('2');
			console.log('2-1');
			console.log('2-2');
			console.log('2-3');
			console.groupEnd();


11. console.dir()
	- 在firfox中用会把数据的层级显示出来
	- 在chorme中打印dom元素，dir()会以json的形式输出

