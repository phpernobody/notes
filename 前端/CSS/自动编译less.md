## 使用koala工具

## sublime下装插件
- 安装nodejs
- 安装less：`npm install less -g`
- sublime装less2css插件
	> ctrl+shift+p -> install Package -> less2css
- 配置less2css
	- Preferences -> Package Settings -> Less2Css -> Settings-Default
	
			{
			  "autoCompile": true,
			  "createCssSourceMaps": false,
			  "ignorePrefixedFiles": false,
			  "lessBaseDir": "./",
			  "lesscCommand": false,
			  "main_file": false,
			  "minify": true,
			  "minName": false,
			  "outputDir": "./",
			  "outputFile": "", //[example.css] if left blank uses same name of .less file
			  "showErrorWithWindow": true,
			  "autoprefix": false,
			  "disableVerbose":false,
			  "silent":false
			}
	- Preferences → Package Settings → Less2Css → Settings-User

			{
			  "minify":false
			}
- 执行编写好less文件后 按下ctrl+s 就会在当前目录下生成同名的css文件