# Webpack 是什么

webpack是一款模块加载器兼打包工具，它能把各种资源，例如JS（含JSX）、coffee、样式（含less/sass）、图片等都作为模块来使用和处理。

我们可以直接使用 require(XXX) 的形式来引入各模块，即使它们可能需要经过编译（比如JSX和sass），但我们无须在上面花费太多心思，因为 webpack 有着各种健全的加载器（loader）在默默处理这些事情，这块我们后续会提到。

webpack的官网是 http://webpack.github.io/ ，文档地址是 http://webpack.github.io/docs/ 。
## webpack 的优势

1. webpack 是以 commonJS 的形式来书写脚本滴，但对 AMD/CMD 的支持也很全面，方便旧项目进行代码迁移。
2. 能被模块化的不仅仅是 JS 了。
3. 开发便捷，能替代部分 grunt/gulp 的工作，比如打包、压缩混淆、图片转base64等。
4. 扩展性强，插件机制完善，特别是支持 React 热插拔（见 react-hot-loader ）的功能让人眼前一亮。

## 基本使用 
### 安装和配置
#### 一. 安装

我们常规直接使用 npm 的形式来安装：（在此之前请记得先安装npm，就是安装node.js）

    npm install webpack -g

当然如果常规项目还是把依赖写入 package.json 包去更人性化：

    npm init$ npm install webpack --save-dev

#### 二. 配置
每个项目下都必须配置有一个 webpack.config.js ，它的作用如同常规的 gulpfile.js/Gruntfile.js ，就是一个配置项，告诉 webpack 它需要做什么。

我们看看下方的示例：

    var webpack = require('webpack');
    var commonsPlugin =  new webpack.optimize.CommonsChunkPlugin('common.js');
    module.exports = {
        //插件项
        plugins: [commonsPlugin],
        //页面入口文件配置
        entry: {
        index : './src/js/page/index.js'
        },
        //入口文件输出配置
        output: {
        path: 'dist/js/page',
        filename: '[name].js'
        },
        module: {
        //加载器配置
        loaders: [
            { test: /\.css$/, loader: 'style-loader!css-loader' },
            { test: /\.js$/, loader: 'jsx-loader?harmony' },
            { test: /\.scss$/, loader: 'style!css!sass?sourceMap'},
            { test: /\.(png|jpg)$/, loader: 'url-loader?limit=8192'}
        ]
        },
        //其它解决方案配置
        resolve: {
            root: 'E:/github/flux-example/src', //绝对路径
            extensions: ['', '.js', '.json', '.scss'],
            alias: {
            AppStore : 'js/stores/AppStores.js',
            ActionType : 'js/actions/ActionType.js',
            AppAction : 'js/actions/AppAction.js'
            }
        }
    };
1. plugins 是插件项，这里我们使用了一个 CommonsChunkPlugin的插件，它用于提取多个入口文件的公共脚本部分，然后生成一个 common.js 来方便多页面之间进行复用。

2. entry 是页面入口文件配置，output 是对应输出项配置 （即入口文件最终要生成什么名字的文件、存放到哪里） ，其语法大致为：    

	    {
	        entry: {
	            page1:"./page1",// 支持数组形式，将加载数组中的所有模块，但以最后一个模块作为输出
	            page2: ["./entry1", "./entry2"]
	        },
	        output: {
	            path: "dist/js/page",
	            filename: "[name].bundle.js"
	        }
	    }
    
该段代码最终会生成一个 page1.bundle.js 和 page2.bundle.js，并存放到 ./dist/js/page 文件夹下。

3. module.loaders 是最关键的一块配置。它告知 webpack 每一种文件都需要使用什么加载器来处理：


	    module: {
	        //加载器配置
	        loaders: [
	            //.css 文件使用 style-loader 和 css-loader 来处理
	            { test: /\.css$/, loader: 'style-loader!css-loader' },
	            //.js 文件使用 jsx-loader 来编译处理
	            { test: /\.js$/, loader: 'jsx-loader?harmony' },
	            //.scss 文件使用 style-loader、css-loader 和 sass-loader 来编译处理
	            { test: /\.scss$/, loader: 'style!css!sass?sourceMap'},
	            //图片文件使用 url-loader 来处理，小于8kb的直接转为base64
	            { test: /\.(png|jpg)$/, loader: 'url-loader?limit=8192'}
	        ]
	    }
    
如上，"-loader"其实是可以省略不写的，多个loader之间用“!”连接起来。

注意所有的加载器都需要通过 npm 来加载，并建议查阅它们对应的 readme 来看看如何使用。

拿最后一个 url-loader 来说，它会将样式中引用到的图片转为模块来处理，使用该加载器需要先进行安装：

    npm install url-loader -save-dev

配置信息的参数“?limit=8192”表示将所有小于8kb的图片都转为base64形式 （其实应该说超过8kb的才使用 url-loader 来映射到文件，否则转为data url形式） 。

4. 最后是 resolve 配置，这块很好理解，直接写注释了：


	    resolve: {
	        //查找module的话从这里开始查找
	        root: 'E:/github/flux-example/src', //绝对路径
	        //自动扩展文件后缀名，意味着我们require模块可以省略不写后缀名
	        extensions: ['', '.js', '.json', '.scss'],
	        //模块别名定义，方便后续直接引用别名，无须多写长长的地址
	        alias: {
	        AppStore : 'js/stores/AppStores.js',//后续直接 require('AppStore') 即可
	        ActionType : 'js/actions/ActionType.js',
	        AppAction : 'js/actions/AppAction.js'
	        }
	    }


### 运行 webpack

webpack 的执行也很简单，直接执行

    webpack --display-error-details

即可，后面的参数“--display-error-details”是推荐加上的，方便出错时能查阅更详尽的信息（比如 webpack 寻找模块的过程），从而更好定位到问题。

其他主要的参数有：

	    $ webpack --config XXX.js //使用另一份配置文件（比如webpack.config2.js）来打包
	    $ webpack --watch //监听变动并自动打包
	    $ webpack -p //压缩混淆脚本，这个非常非常重要！
	    $ webpack -d //生成map映射文件，告知哪些模块被最终打包到哪里了
    
其中的 -p 是很重要的参数，曾经一个未压缩的 700kb 的文件，压缩后直接降到 180kb （主要是样式这块一句就独占一行脚本，导致未压缩脚本变得很大） 。

## 模块引用

一. HTML
直接在页面引入 webpack 最终生成的页面脚本即可，不用再写什么 data-main 或seajs.use 了：

    <!DOCTYPE html>
    <html>
    <head lang="en">
    <meta charset="UTF-8">
    <title>demo</title>
    </head>
    <body>
    <script src="dist/js/page/common.js"></script>
    <script src="dist/js/page/index.js"></script>
    </body>
    </html>
    
可以看到我们连样式都不用引入，毕竟脚本执行时会动态生成\<style\>并标签打到head里。

二. JS
各脚本模块可以直接使用 commonJS 来书写，并可以直接引入未经编译的模块，比如 JSX、sass、coffee等（只要你在 webpack.config.js 里配置好了对应的加载器）。

我们再看看编译前的页面入口文件（index.js）：

    require('../../css/reset.scss'); //加载初始化样式
    require('../../css/allComponent.scss'); //加载组件样式
    var angular = require('angular');
    
## 其他
### CommonJS 与 AMD 支持

Webpack 对 CommonJS 的 AMD 的语法做了兼容, 方便迁移代码。不过实际上, 引用模块的规则是依据 CommonJS 来的

    require('lodash') // 从模块目录查找
    require('./file') // 按相对路径查找
    
AMD 语法中, 也要注意, 是按 CommonJS 的方案查找的

    define (require, exports. module) ->
      require('lodash') # commonjs 当中这样是查找模块的
      require('./file')

### 特殊模块的处理 shimming

在 AMD/CMD 中，我们需要对不符合规范的模块（比如一些直接返回全局变量的插件）进行 shim 处理，这时候我们需要使用 exports-loader 来帮忙：

    { test: require.resolve("./src/js/tool/swipe.js"), loader: "exports?swipe"}
    
之后在脚本中需要引用该模块的时候，这么简单地来使用就可以了：

    require('./tool/swipe.js');
swipe(); 

### CSS 及图片的引用

    require('./bootstrap.css');
    require('./myapp.less');
    var img = document.createElement('img');
    img.src = require('./glyph.png');
    
上边的是 JavaScript 代码, CSS 跟 LESS, 还有图片, 被直接引用了。实际上 CSS 被转化为 /<style/> 标签, 而图片可能被转化成 base64 格式的 dataUrl。 但是要主要在 webpack.config.js 文件写好对应的 loader:

    // webpack.config.js
    module.exports = {
      entry: './main.js',
      output: {
        path: './build', // This is where images AND js will go
        publicPath: 'http://mycdn.com/', // This is used to generate URLs to e.g. images
        filename: 'bundle.js'
      },
      module: {
        loaders: [
          { test: /\.less$/, loader: 'style-loader!css-loader!less-loader' }, // use ! to chain loaders
          { test: /\.css$/, loader: 'style-loader!css-loader' },
          {test: /\.(png|jpg)$/, loader: 'url-loader?limit=8192'} // inline base64 URLs for <=8k images, direct URLs for the rest
        ]
      }
    };

### url-loader

稍微啰嗦一下这个 loader, 这个 loader 实际上是对 file-loader 的封装https://github.com/webpack/url-loader
比如 CSS 文件当中有这样的引用:

    .demo {
      background-image: url('a.png');
    }
那么对应这样的 loader 配置就能把 a.png 抓出来, 并且按照文件大小, 或者转化为 base64, 或者单独作为文件:

    module: {
      loaders: [
        {test: /\.(png|jpg)$/, loader: 'url-loader?limit=8192'} // inline base64 URLs for <=8k images, direct URLs for the rest
      ]
    }
上边 ? 后边的 query 有两种写法, 可以看下文档: http://webpack.github.io/docs/using-loaders.html#query-parameters

### 独立打包样式文件
有时候可能希望项目的样式能不要被打包到脚本中，而是独立出来作为.css，然后在页面中以/<link/>标签引入。这时候我们需要 extract-text-webpack-plugin 来帮忙：

    var webpack = require('webpack');
    var commonsPlugin = new webpack.optimize.CommonsChunkPlugin('common.js');
    var ExtractTextPlugin = require("extract-text-webpack-plugin");
    module.exports = {
    plugins: [commonsPlugin, new ExtractTextPlugin("[name].css")],
    entry: {
    //...省略其它配置
    
最终 webpack 执行后会乖乖地把样式文件提取出来。

### 与 grunt/gulp 配合

以 gulp 为示例，我们可以这样混搭：

    gulp.task("webpack", function(callback) {
        // run webpack
        webpack({
        // configuration
        }, function(err, stats) {
        if(err) throw new gutil.PluginError("webpack", err);
        gutil.log("[webpack]", stats.toString({
        // output options
        }));
        callback();
        });
    });
当然我们只需要把配置写到 webpack({ ... }) 中去即可，无须再写 webpack.config.js 了。

更多参照信息请参阅： grunt配置 / gulp配置 。

### React 相关
1. 推荐使用 npm install react 的形式来安装并引用 React 模块，而不是直接使用编译后的 react.js，这样最终编译出来的 React 部分的脚本会减少 10-20 kb左右的大小。

2. react-hot-loader 是一款非常好用的 React 热插拔的加载插件，通过它可以实现修改-运行同步的效果，配合 webpack-dev-server 使用更佳！

