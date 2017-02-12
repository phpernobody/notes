[官方地址](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1474632113_xQVCl&token=&lang=zh_CN)

##目录结构
	-pages
    	-index
	 		-index.js
			-index.wxml
			-index.wxss
			-index.json
		-anothor
			-anothor.js
			-anothor.wxml
			-anothor.wxss
			-anothor.json
  		-logs
			-logs.js
			-logs.wxml
			-logs.wxss
			-logs.json
	-utils
		-util.js
	app.js
	app.json
	app.wxss

**一个app小程序由app主页面、utils和多个pages组成，每个page由js/wxml/wxss/json组成，官方为了省去多余的配置项，每个页面的js/wxml/wxss/json文件要设成同名**


##配置
####app.json

	{
	  "pages": [
	    "pages/index/index",
	    "pages/logs/index"
	  ],
	  "window": {
	    "navigationBarTitleText": "Demo"
	  },
	  "tabBar": {
	    "list": [{
	      "pagePath": "pages/index/index",
	      "text": "首页"
	    }, {
	      "pagePath": "pages/logs/logs",
	      "text": "日志"
	    }]
	  },
	  "networkTimeout": {
	    "request": 10000,
	    "downloadFile": 10000
	  },
	  "debug": true
	}

- pages				array	设置页面路径
- window			object	设置默认页面的窗口表现
- tabBar			object	设置底部tab的表现
- networkTimeout	object	设置网络超时时间
- debug				boolean	设置是否开启调试模式

####window

	{
	  "window":{
	    "navigationBarBackgroundColor": "#ffffff",
	    "navigationBarTextStyle": "black",
	    "navigationBarTitleText": "微信接口功能演示",
	    "backgroundColor": "#eeeeee",
	    "backgroundTextStyle": "light"
	  }
	}

- navigationBarBackgroundColor	HexColor(#000000)	导航栏背景颜色，如"#000000"
- navigationBarTextStyle		String(white)		导航栏标题颜色，仅支持 black/white
- navigationBarTitleText		String				导航栏标题文字内容
- backgroundColor				HexColor(#ffffff)	窗口的背景色
- backgroundTextStyle			String(dark)		下拉背景字体、loading 图的样式，仅支持 dark/light
- enablePullDownRefresh			Boolean(false)		是否开启下拉刷新，详见页面相关事件处理函数。 

####tarBar
- color			HexColor(必填)		tab上的文字默认颜色
- selectedColor	HexColor(必填)		tab上的文字选中时的颜色
- backgroundColor	HexColor(必填)		tab的背景色
- borderStyle		String(非必填black)	tabbar上边框的颜色，仅支持black/white
- list			Array(必填)			tab的列表，详见list属性说明，最少2个、最多5个tab
	- pagePath			String(必填)		页面路径，必须在 pages 中先定义
	- text				String(必填)		tab 上按钮文字
	- iconPath			String(必填)		图片路径，icon 大小限制为40kb
	- selectedIconPath	String(必填)		选中时的图片路径，icon 大小限制为40kb

####networkTimeout
- request		Number	wx.request的超时时间，单位毫秒
- connectSocket	Number	wx.connectSocket的超时时间，单位毫秒
- uploadFile	Number	wx.uploadFile的超时时间，单位毫秒
- downloadFile	Number	wx.downloadFile的超时时间，单位毫秒

**pages中的.json文件只能配置window，优先级比app.json高**

##逻辑层
###APP()
>app()用于注册一个小程序

	App({
	  onLaunch: function() { 
	    // Do something initial when launch.
	  },
	  onShow: function() {
	      // Do something when show.
	  },
	  onHide: function() {
	      // Do something when hide.
	  },
	  globalData: 'I am global data'
	})

<table>
<tr><td>属性</td><td>类型</td><td>描述</td><td>触发时机</td></tr>
<tr><td>onLaunch</td><td>Function</td><td>生命周期函数--监听小程序初始化</td><td>当小程序初始化完成时，会触发 onLaunch（全局只触发一次）</td></tr>
<tr><td>onShow</td><td>Function</td><td>生命周期函数--监听小程序显示</td><td>当小程序启动，或从后台进入前台显示，会触发 onShow</td></tr>
<tr><td>onHide</td><td>Function</td><td>生命周期函数--监听小程序隐藏</td><td>当小程序从前台进入后台，会触发 onHide</td></tr>
<tr><td>其他</td><td>any</td><td>开发者可以添加任意的函数或数据到 Object 参数中，用 this 可以访问</td><td></td></tr>
</table>

######App.prototype.getCurrentPage()
>getCurrentPage() 函数用户获取当前页面的实例。

######getApp()
>我们提供了全局的 getApp() 函数，可以获取到小程序实例。

	// other.js
	var appInstance = getApp()
	console.log(appInstance.globalData) // I am global data

**tips**：

App() 必须在 app.js 中注册，且不能注册多个。

不要在定义于 App() 内的函数中调用 getApp() ，使用 this 就可以拿到 app 实例。

不要在 onLaunch 的时候调用 getCurrentPage()，此时 page 还没有生成。

通过 getApp() 获取实例之后，不要私自调用生命周期函数。

###Page
>Page() 函数用来注册一个页面。接受一个 object 参数，其指定页面的初始数据、生命周期函数、事件处理函数等。

	//index.js
	Page({
	  data: {
	    text: "This is page data."
	  },
	  onLoad: function(options) {
	    // Do some initialize when page load.
	  },
	  onReady: function() {
	    // Do something when page ready.
	  },
	  onShow: function() {
	    // Do something when page show.
	  },
	  onHide: function() {
	    // Do something when page hide.
	  },
	  onUnload: function() {
	    // Do something when page close.
	  },
	  onPullDownRefresh: function() {
	    // Do something when pull down
	  },
	  // Event handler.
	  viewTap: function() {
	    this.setData({
	      text: 'Set some data for updating view.'
	    })
	  }
	})

<table>
<tr><td>属性</td><td>类型</td><td>描述</td></tr>
<tr><td>data</td><td>Object</td><td>页面的初始数据</td></tr>
<tr><td>onLoad</td><td>Function</td><td>生命周期函数--监听页面加载</td></tr>
<tr><td>onReady</td><td>Function</td><td>生命周期函数--监听页面初次渲染完成</td></tr>
<tr><td>onShow</td><td>Function</td><td>生命周期函数--监听页面显示</td></tr>
<tr><td>onHide</td><td>Function</td><td>生命周期函数--监听页面隐藏</td></tr>
<tr><td>onUnload</td><td>Function</td><td>生命周期函数--监听页面卸载</td></tr>
<tr><td>onPullDownRefreash</td><td>Function</td><td>页面相关事件处理函数--监听用户下拉动作</td></tr>
<tr><td>其他</td><td>any</td><td>开发者可以添加任意的函数或数据到 object 参数中，用 this 可以访问</td></tr>
</table>

######生命周期函数
- onLoad: 页面加载
	- 一个页面只会调用一次。
	- 参数可以获取wx.navigateTo和wx.redirectTo及<navigator/>中的 query。
- onShow: 页面显示
	- 每次打开页面都会调用一次。
- onReady: 页面初次渲染完成
	- 一个页面只会调用一次，代表页面已经准备妥当，可以和视图层进行交互。
	- 对界面的设置如wx.setNavigationBarTitle请在onReady之后设置。详见生命周期
- onHide: 页面隐藏
	- 当navigateTo或底部tab切换时调用。
- onUnload: 页面卸载
	- 当redirectTo或navigateBack的时候调用。
	
######页面相关事件处理函数
- onPullDownRefresh: 下拉刷新
	- 监听用户下拉刷新事件。
	- 需要在config的window选项中开启enablePullDownRefresh。
	- 当处理完数据刷新后，wx.stopPullDownRefresh可以停止当前页面的下拉刷新。
	
######事件处理函数
>除了初始化数据和生命周期函数，Page 中还可以定义一些特殊的函数：事件处理函数。在渲染层可以在组件中加入事件绑定，当达到触发事件时，就会执行 Page 中定义的事件处理函数。

	<view bindtap="viewTap"> click me </view>

	Page({
	  viewTap: function() {
	    console.log('view tap')
	  }
	})

- Page.prototype.setData()
>setData 函数用于将数据从逻辑层发送到视图层，同时改变对应的 this.data 的值。

**tips**

- 注意：
	- 直接修改 this.data 无效，无法改变页面的状态，还会造成数据不一致。
	- 单次设置的数据不能超过1024kB，请尽量避免一次设置过多的数据。

######setData() 参数格式
>接受一个对象，以 key，value 的形式表示将 this.data 中的 key 对应的值改变成 value。
其中 key 可以非常灵活，以数据路径的形式给出，如 array[2].message，a.b.c.d，并且不需要在 this.data 中预先定义。
	
	<!--index.wxml-->
	<view>{{text}}</view>
	<button bindtap="changeText"> Change normal data </button>
	<view>{{array[0].text}}</view>
	<button bindtap="changeItemInArray"> Change Array data </button>
	<view>{{obj.text}}</view>
	<button bindtap="changeItemInObject"> Change Object data </button>
	<view>{{newField.text}}</view>
	<button bindtap="addNewField"> Add new data </button>

	//index.js
	Page({
	  data: {
	    text: 'init data',
	    array: [{text: 'init data'}],
	    object: {
	      text: 'init data'
	    }
	  },
	  changeText: function() {
	    // this.data.text = 'changed data'  // bad, it can not work
	    this.setData({
	      text: 'changed data'
	    })
	  },
	  changeItemInArray: function() {
	    // you can use this way to modify a danamic data path
	    this.setData({
	      'array[0].text':'changed data'
	    })
	  },
	  changeItemInObject: function(){
	    this.setData({
	      'object.text': 'changed data'
	    });
	  },
	  addNewField: function() {
	    this.setData({
	      'newField.text': 'new data'
	    })
	  }
	})

######生命周期图
![链接](https://mp.weixin.qq.com/debug/wxadoc/dev/image/mina-lifecycle.png?t=1475218869876)


######页面的路由
>在小程序中所有页面的路由全部由框架进行管理，对于路由的触发方式以及页面生命周期函数如下：

<table>
<tr><td>路由方式</td><td>触发时机</td><td>路由后页面</td><td>路由前页面</td></tr>
<tr><td>初始化</td><td>小程序打开的第一个页面</td><td>onLoad，onShow</td><td></td></tr>
<tr><td>打开新页面</td><td>调用 API wx.navigateTo 或使用组件 navigator</td><td>onLoad，onShow</td><td>onHide</td></tr>
<tr><td>页面重定向</td><td>调用 API wx.redirectTo 或使用组件 navigator</td><td>onLoad，onShow</td><td>onUnload</td></tr>
<tr><td>页面返回</td><td>调用 API wx.navigateBack或用户按左上角返回按钮</td><td>onShow</td><td>onUnload</td></tr>
<tr><td>Tab切换</td><td>多 Tab 模式下用户切换 Tab</td><td>第一次打开 onLoad，onshow；否则 onShow</td><td>onHide</td></tr>
</table>


##文件作用域
>在 JavaScript 文件中声明的变量和函数只在该文件中有效；不同的文件中可以声明相同名字的变量和函数，不会互相影响。

通过全局函数 getApp() 可以获取全局的应用实例，如果需要全局的数据可以在 App() 中设置，如：

	// app.js
	App({
	  globalData: 1
	})
	// a.js
	// The localValue can only be used in file a.js.
	var localValue = 'a'
	// Get the app instance.
	var app = getApp()
	// Get the global data and change it.
	app.globalData++
	// b.js
	// You can redefine localValue in file b.js, without interference with the localValue in a.js.
	var localValue = 'b'
	// If a.js it run before b.js, now the globalData shoule be 2.
	console.log(getApp().globalData)

##模块化
>我们可以将一些公共的代码抽离成为一个单独的 js 文件，作为一个模块。模块只有通过 module.exports 才能对外暴露接口。

	// common.js
	function sayHello(name) {
	  console.log('Hello ' + name + '!')
	}
	module.exports = {
	  sayHello: sayHello
	}

>​在需要使用这些模块的文件中，使用 require(path) 将公共代码引入。

	var common = require('common.js')
	Page({
	  helloMINA: function() {
	    common.sayHello('MINA')
	  }
	})


##数据绑定
>数据绑定使用 Mustache 语法（双大括号）将变量包起来

######内容

	<view> {{ message }} </view>

	Page({
	  data: {
	    message: 'Hello MINA!'
	  }
	})

######组件属性(需要在双引号之内)

	<view id="item-{{id}}"> </view>

	Page({
	  data: {
	    id: 0
	  }
	})

######控制属性(需要在双引号之内)

	<view wx:if="{{condition}}"> </view>

	Page({
	  data: {
	    condition: true
	  }
	})

##运算
>可以在 {{}} 内进行简单的运算

######三元运算
	<view hidden="{{flag ? true : false}}"> Hidden </view>

######算数运算
	<view> {{a + b}} + {{c}} + d </view>

	Page({
	  data: {
	    a: 1,
	    b: 2,
	    c: 3
	  }
	})
	
	view中的内容为 3 + 3 + d。

######逻辑判断
	<view wx:if="{{length > 5}}"> </view>

######字符串运算
	<view>{{"hello" + name}}</view>

	Page({
	  data:{
	    name: 'MINA'
	  }
	})

##组合
>也可以在 Mustache 内直接进行组合，构成新的对象或者数组。

######数组
	<view wx:for="{{[zero, 1, 2, 3, 4]}}"> {{item}} </view>
	
	Page({
	  data: {
	    zero: 0
	  }
	})
	
	最终组合成数组[0, 1, 2, 3, 4]。

######对象
	<template is="objectCombine" data="{{for: a, bar: b}}"></template>

	Page({
	  data: {
	    a: 1,
	    b: 2
	  }
	})
	最终组合成的对象是 {for: 1, bar: 2}
	
	也可以用扩展运算符 ... 来将一个对象展开
	
	<template is="objectCombine" data="{{...obj1, ...obj2, e: 5}}"></template>
	Page({
	  data: {
	    obj1: {
	      a: 1,
	      b: 2
	    },
	    obj2: {
	      c: 3,
	      d: 4
	    }
	  }
	})

	最终组合成的对象是 {a: 1, b: 2, c: 3, d: 4, e: 5}。
	
	如果对象的 key 和 value 相同，也可以间接地表达。
	
	<template is="objectCombine" data="{{foo, bar}}"></template>
	Page({
	  data: {
	    foo: 'my-foo',
	    bar: 'my-bar'
	  }
	})

	最终组合成的对象是 {foo: 'my-foo', bar:'my-bar'}。


**tips**

	注意：上述方式可以随意组合，但是如有存在变量名相同的情况，后边的会覆盖前面，如：
	
	<template is="objectCombine" data="{{...obj1, ...obj2, a, c: 6}}"></template>
	Page({
	  data: {
	    obj1: {
	      a: 1,
	      b: 2
	    },
	    obj2: {
	      b: 3,
	      c: 4
	    },
	    a: 5
	  }
	})
	最终组合成的对象是 {a: 5, b: 3, c: 6}。

##条件渲染
####wx:if
>在框架中，我们用 wx:if="{{condition}}" 来判断是否需要渲染该代码块：

	<view wx:if="{{condition}}"> True </view>

>也可以用 wx:elif 和 wx:else 来添加一个 else 块：

	<view wx:if="{{length > 5}}"> 1 </view>
	<view wx:elif="{{length > 2}}"> 2 </view>
	<view wx:else> 3 </view>

####block wx:if
>因为 wx:if 是一个控制属性，需要将它添加到一个标签上。但是如果我们想一次性判断多个组件标签，我们可以使用一个 <block/> 标签将多个组件包装起来，并在上边使用 wx:if 控制属性。

    <block wx:if="{{true}}">
      <view> view1 </view>
      <view> view2 </view>
    </block>

**tip： <block/> 并不是一个组件，它仅仅是一个包装元素，不会在页面中做任何渲染，只接受控制属性。**

####wx:if vs hidden

因为 wx:if 之中的模板也可能包含数据绑定，所有当 wx:if 的条件值切换时，框架有一个局部渲染的过程，因为它会确保条件块在切换时销毁或重新渲染。

同时 wx:if 也是惰性的，如果在初始渲染条件为 false，框架什么也不做，在条件第一次变成真的时候才开始局部渲染。

相比之下，hidden 就简单的多，组件始终会被渲染，只是简单的控制显示与隐藏。

一般来说，wx:if 有更高的切换消耗而 hidden 有更高的初始渲染消耗。因此，如果需要频繁切换的情景下，用 hidden 更好，如果在运行时条件不大可能改变则 wx:if 较好。



##列表渲染
####wx:for
>在组件上使用wx:for控制属性绑定一个数组，即可使用数组中各项的数据重复渲染该组件。
>默认数组的当前项的下标变量名默认为index，数组当前项的变量名默认为item

	<view wx:for="{{items}}">
	  {{index}}: {{item.message}}
	</view>
	Page({
	  data: {
	    items: [{
	      message: 'foo',
	    }, {
	      message: 'bar'
	    }]
	  }
	})

>使用wx:for-item可以指定数组当前元素的变量名
>使用wx:for-index可以指定数组当前下标的变量名：

    <view wx:for="{{array}}" wx:for-index="idx" wx:for-item="itemName">
      {{idx}}: {{itemName.message}}
    </view>

>wx:for也可以嵌套，下边是一个九九乘法表

	<view wx:for="{{[1, 2, 3, 4, 5, 6, 7, 8, 9]}}" wx:for-item="i">
	  <view wx:for="{{[1, 2, 3, 4, 5, 6, 7, 8, 9]}}" wx:for-item="j">
	    <view wx:if="{{i <= j}}">
	      {{i}} * {{j}} = {{i * j}}
	    </view>
	  </view>
	</view>

####block wx:for
>类似block wx:if，也可以将wx:for用在<block/>标签上，以渲染一个包含多节点的结构块。例如：

    <block wx:for="{{[1, 2, 3]}}">
      <view> {{index}}: </view>
      <view> {{item}} </view>
    </block>



##模板
>WXML提供模板（template），可以在模板中定义代码片段，然后在不同的地方调用。

####定义模板
>使用name属性，作为模板的名字。然后在<template/>内定义代码片段，如：
	
	<!--
	  index: int
	  msg: string
	  time: string
	-->
	<template name="msgItem">
	  <view>
	    <text> {{index}}: {{msg}} </text>
	    <text> Time: {{time}} </text>
	  </view>
	</template>

####使用模板
>使用 is 属性，声明需要的使用的模板，然后将模板所需要的 data 传入，如：
	
	<template is="msgItem" data="{{...item}}"/>
	Page({
	  data: {
	    item: {
	      index: 0,
	      msg: 'this is a template',
	      time: '2016-09-15'
	    }
	  }
	})

>is 属性可以使用 Mustache 语法，来动态决定具体需要渲染哪个模板：
	
	<template name="odd">
	  <view> odd </view>
	</template>
	<template name="even">
	  <view> even </view>
	</template>
	
	<block wx:for="{{[1, 2, 3, 4, 5]}}">
	    <template is="{{item % 2 == 0 ? 'even' : 'odd'}}"/>
	</block>

####模板的作用域
>模板拥有自己的作用域，只能使用data传入的数据。


##事件
####什么是事件
- 事件是视图层到逻辑层的通讯方式。
- 事件可以将用户的行为反馈到逻辑层进行处理。
- 事件可以绑定在组件上，当达到触发事件，就会执行逻辑层中对应的事件处理函数。
- 事件对象可以携带额外信息，如id, dataset, touches。

####事件的使用方式
- 在组件中绑定一个事件处理函数。
	>如bindtap，当用户点击该组件的时候会在该页面对应的Page中找到相应的事件处理函数。
	<view id="tapTest" data-hi="MINA" bindtap="tapName"> Click me! </view>
- 在相应的Page定义中写上相应的事件处理函数，参数是event。

    Page({
      tapName: function(event) {
    	console.log(event)
      }
    })

- 可以看到log出来的信息大致如下：

	{
	"type": "tap",
	"timeStamp": 1252,
	"target": {
	  "id": "tapTest",
	  "offsetLeft": 0,
	  "offsetTop": 0,
	  "dataset": {
	   "hi": "MINA"
	  }
	},
	"currentTarget": {
	  "id": "tapTest",
	  "offsetLeft": 0,
	  "offsetTop": 0,
	  "dataset": {
	    "hi": "MINA"
	  }
	},
	"touches": [{
	  "pageX": 30,
	  "pageY": 12,
	  "clientX": 30,
	  "clientY": 12,
	  "screenX": 112,
	  "screenY": 151
	}],
	"detail": {
	  "x": 30,
	  "y": 12
	}
	}
	
####事件详解

######事件分类
>事件分为冒泡事件和非冒泡事件：

- 冒泡事件：当一个组件上的事件被触发后，该事件会向父节点传递。
- 非冒泡事件：当一个组件上的事件被触发后，该事件不会向父节点传递。

>WXML的冒泡事件列表：
<table>
<tr><td>类型</td><td>触发条件</td></tr>
<tr><td>touchstart</td><td>手指触摸</td></tr>
<tr><td>touchmove</td><td>手指触摸后移动</td></tr>
<tr><td>touchcancel</td><td>手指触摸动作被打断，如来电提醒，弹窗</td></tr>
<tr><td>touchend</td><td>手指触摸动作结束</td></tr>
<tr><td>tap</td><td></td>手指触摸后离开</tr>
<tr><td>longtap</td><td>手指触摸后，超过350ms再离开</td></tr>
</table>

**tip：除上表之外的其他组件自定义事件都是非冒泡事件，如form的submit事件，input的input事件，scroll-view的scroll事件，(详见各个组件)**

######事件绑定
>事件绑定的写法同组件的属性，以 key、value 的形式。

- key 以bind或catch开头，然后跟上事件的类型，如bindtap, catchtouchstart
- value 是一个字符串，需要在对应的 Page 中定义同名的函数。不然当触发事件的时候会报错。

bind事件绑定不会阻止冒泡事件向上冒泡，catch事件绑定可以阻止冒泡事件向上冒泡。

>如在下边这个例子中，点击 inner view 会先后触发handleTap3和handleTap2(因为tap事件会冒泡到 middle view，而 middle view 阻止了 tap 事件冒泡，不再向父节点传递)，点击 middle view 会触发handleTap2，点击 outter view 会触发handleTap1。
	
	<view id="outter" bindtap="handleTap1">
	  outer view
	  <view id="middle" catchtap="handleTap2">
	    middle view
	    <view id="inner" bindtap="handleTap3">
	      inner view
	    </view>
	  </view>
	</view>

######事件对象
>如无特殊说明，当组件触发事件时，逻辑层绑定该事件的处理函数会收到一个事件对象。
**事件对象的属性列表：**
<table>
<tr><td>属性</td><td>类型</td><td>说明</td></tr>
<tr><td>type</td><td>String</td><td>事件类型</td></tr>
<tr><td>timeStamp</td><td>Integer</td><td>事件生成时的时间戳</td></tr>
<tr><td>target</td><td>Object</td><td>触发事件的组件的一些属性值集合</td></tr>
<tr><td>currentTarget</td><td>Object</td><td>当前组件的一些属性值集合</td></tr>
<tr><td>touches</td><td>Array</td><td>触摸事件，触摸点信息的数组</td></tr>
<tr><td>detail</td><td>Object</td><td>额外的信息</td></tr>
</table>

- type
	>通用事件类型
- timeStamp
	>该页面打开到触发事件所经过的毫秒数。
- target
	>触发事件的源组件。
    <table>
    <tr><td>属性</td><td>说明</td></tr>
    <tr><td>id</td><td>事件源组件的id</td></tr>
    <tr><td>dataset</td><td>事件源组件上由data-开头的自定义属性组成的集合</td></tr>
    <tr><td>offsetLeft, offsetTop</td><td>事件源组件的坐标系统中偏移量</td></tr>
    </table>

- currentTarget
	>事件绑定的当前组件。
	<table>
    <tr><td>属性</td><td>说明</td></tr>
    <tr><td>id</td><td>当前组件的id</td></tr>
    <tr><td>dataset</td><td>当前组件上由data-开头的自定义属性组成的集合</td></tr>
    <tr><td>offsetLeft, offsetTop</td><td>当前组件的坐标系统中偏移量</td></tr>
    </table>
	**说明: target 和 currentTarget 可以参考上例中，点击 inner view 时，handleTap3收到的事件对象 target 和 currentTarget 都是 inner，而handleTap2收到的事件对象 target 就是 inner，currentTarget 就是 middle**
	- dataset
		>在组件中可以定义数据，这些数据将会通过事件传递给 SERVICE。 书写方式： 以data-开头，多个单词由连字符-链接，不能有大写(大写会自动转成小写)如data-element-type，最终在 event.target.dataset 中会将连字符转成驼峰elementType。
		
	    <view data-alpha-beta="1" data-alphaBeta="2" bindtap="bindViewTap"> DataSet Test </view>
	    Page({
	      bindViewTap:function(event){
	    	event.target.dataset.alphaBeta == 1 // - 会转为驼峰写法
	    	event.target.dataset.alphabeta == 2 // 大写会转为小写
	      }
	    })

- touches
	>touches是一个触摸点的数组，每个触摸点包括以下属性：
	<table>
    <tr><td>属性</td><td>说明</td></tr>
    <tr><td>pageX,pageY</td><td>距离文档左上角的距离，文档的左上角为原点 ，横向为X轴，纵向为Y轴</td></tr>
    <tr><td>clientX,clientY</td><td>距离页面可显示区域（屏幕除去导航条）左上角距离，横向为X轴，纵向为Y轴</td></tr>
    <tr><td>screenX,screenY</td><td>距离屏幕左上角的距离，屏幕左上角为原点，横向为X轴，纵向为Y轴</td></tr>
    </table>

- detail
>特殊事件所携带的数据，如表单组件的提交事件会携带用户的输入，媒体的错误事件会携带错误信息，详见组件定义中各个事件的定义。

##引用
>WXML 提供两种文件引用方式import和include。
####import
>import可以在该文件中使用目标文件定义的template，如：

在 item.wxml 中定义了一个叫item的template：

	<!-- item.wxml -->
	<template name="item">
	  <text>{{text}}</text>
	</template>

在 index.wxml 中引用了 item.wxml，就可以使用item模板：

	<import src="item.wxml"/>
	<template is="item" data="{{text: 'forbar'}}"/>

####import 的作用域
>import 有作用域的概念，即只会 import 目标文件中定义的 template，而不会 import 目标文件 import 的 template。

**如：C import B，B import A，在C中可以使用B定义的template，在B中可以使用A定义的template，但是C不能使用A定义的template。**

	<!-- A.wxml -->
	<template name="A">
	  <text> A template </text>
	</template>
	<!-- B.wxml -->
	<import src="a.wxml"/>
	<template name="B">
	  <text> B template </text>
	</template>
	<!-- C.wxml -->
	<import src="b.wxml"/>
	<template is="A"/>  <!-- Error! Can not use tempalte when not import A. -->
	<template is="B"/>

####include
>include可以将目标文件除了<template/>的整个代码引入，相当于是拷贝到include位置，如：
    
    <!-- index.wxml -->
    <include src="header.wxml"/>
    <view> body </view>
    <include src="footer.wxml"/>
    <!-- header.wxml -->
    <view> header </view>
    <!-- footer.wxml -->
    <view> footer </view>








<table>
<tr><td>属性</td><td>类型</td><td>描述</td><td>触发时机</td></tr>
<tr><td></td><td></td><td></td><td></td></tr>
</table>