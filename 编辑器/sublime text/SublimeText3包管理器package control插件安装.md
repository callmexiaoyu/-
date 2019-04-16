# SublimeText3包管理器package control插件安装
***
### Package Control解压
 将Package Control.zip解压，生成Package Control文件夹，内含插件等数据，将该文件夹放在
Sublime Text Build 3176 x64\Data\Packages文件夹下

### There are no packages available for install
浏览器访问
1. https://packagecontrol.io/channel_v3.json
一个json网页，另存为channel_v3.json文件
2. 打开该文件，全文查找 `schema_version`
```json
  "schema_version": "3.0.0",
```
将其改为
```json
  "schema_version": "2.0",
```
并保存 `注意是2.0`
3. 打开Sublime Text3，打开Perfences > Packages Settings > Package Control > Setting-Users文件，一个json文件，添加
```json
"channels":
	[
		"C:\\Users\\Administrator\\Desktop\\Sublime Text3\\channel_v3.json"
	],
```
如下所示
```json
{
	"bootstrapped": true,
	"channels":
	[
		"C:\\Users\\Administrator\\Desktop\\Sublime Text3\\channel_v3.json"
	],
	"in_process_packages":
	[
	],
	"installed_packages":
	[
		"AutoFileName",
		"Color Highlight",
		"ColorPicker",
		"Emmet",
		"JavaScript Completions",
		"jQuery",
		"JsFormat",
		"Node Completions",
		"Package Control",
		"Pretty JSON",
		"SublimeLinter",
		"SublimeLinter-csslint",
		"SublimeLinter-jshint"
	]
}
```
并保存
`其中channels的值为channel_v3.json文件路径`
Package Control成功安装
Ctrl+shift+p快捷键调出Sublime Text3的包安装器
菜单Perferences > Package Control输入Install Package则检索需要的插件