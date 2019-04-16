# nodejs版本控制器nvm安装
***
[TOC]
***
## nvm安装
1. 下载window版免安装板 **nvm-noinstall.zip**
https://github.com/coreybutler/nvm-windows/releases
2. 执行install.cmd
C:\developer\nvm，在该目录下解压缩，打开install.cmd
输入C:\nvm回车，创建一个nodejs快捷键方式，用于切换nodejs版本
3. 弹出setting.txt修改，将
	```
root: 
path: 
arch: 64 
proxy: none
	```
修改为
	```
root: C:\developer\nvm 
path: C:\developer\nodejs 
arch: 64 
proxy: none
node_mirror: https://npm.taobao.org/mirrors/node/
npm_mirror: https://npm.taobao.org/mirrors/npm/
	```
并保存
`root`：为各个版本nodejs安装目录，也就是存放在nvm文件目录下
`path`：在该路径创建一个nodejs文件夹的快捷方式，同时为全局安装npm所在路径，该路径是一个nodejs文件夹快捷方式，切换nodejs版本时，指向实际nodejs所在路径
`全局安装的npm安装在当前使用的nodejs，切换nodejs版本时，npm全局安装的命令失效`
arch系统64位
4. shift+右键调出cmd，输入nvm v弹出版本号表示nvm安装成功，`同时确保生成nodejs文件夹的快捷方式`
5. nvm的cmd常用命令
`nvm list` 已经安装的各个nodejs版本列表
`nvm v` nvm版本号
`nvm install nodejs版本号` 安装nodejs版本，会在nvm目录下下载各个版本的nodejs版本
`nvm use nodejs版本号` 切换nodejs版本号
***
##nvm和nodejs全局变量配置
1. nodejs已安装过的情况下，卸载nodejs，并清除nodejs全局变量设置，方法如下：
计算机右击属性-左边栏高级系统配置-高级-环境变量
系统变量删除NVM_HOME和NVM_SYMLINK，同时在系统变量的Path变量，寻找值和nodejs以及nvm有关的地址值，各个值之间用英文分号;结尾，注意不能有空格
2. 设置用户变量
新建用户变量
`NVM_HOME`
值为
`C:\developer\nvm`
也就是nvm文件目录下nvm.exe所在目录
新建用户变量
`NVM_SYMLINK`
其值为
`C:\developer\nodejs`
通过npm下载的模块存放路径，一个nvm生成的nodejs文件夹快捷方式，用于切换nodejs版本，指向node.exe
新建用户变量
`Path`
其值为
`%NVM_HOME%;%NVM_SYMLINK%;`
注意分号;不能缺少，以及不能出现空格
3. 任意地址调出cmd，输入
`echo %path%`
查看所有环境变量地址 
***
## cnpm和npm
cnpm定时同步npm，npm在国内的镜像
`npm install -g cnpm --registry=https://registry.npm.taobao.org`
通过淘宝镜像地址全局安装cnpm包管理器，和npm一样配置全局变量，操作同上
`npm config set registry https://registry.npm.taobao.org`
设置npm模块下载镜像路径为cnpm的地址，用于提高包下载速度

