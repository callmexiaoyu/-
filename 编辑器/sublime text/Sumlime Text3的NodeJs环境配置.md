# Sumlime Text3的NodeJs环境配置
***
[TOC]
***
## 手动安装nodejs插件
https://github.com/tanepiper/SublimeText-Nodejs
下载压缩包，解压并重命名Nodejs，内含插件文件，将文件移动到Sublime Text Build 3176 x64\Data\Packages目录下
***
## 修改Nodejs.sublime-settings文件
Perferences-Package Settings-Nodejs-Settings-Uers
修改左边栏
```json
{
  "node_command": false,
  "npm_command": false,
}
```
修改为
```json
{
  "node_command": C:\developer\nodejs\node.exe,
  "npm_command": C:\developer\nodejs\npm.cmd,
}
```
`其中地址分别指向nodejs的node.exe和npm.cmd`
***
## 修改Nodejs.sublime-build文件

打开Sublime Text Build 3176 x64\Data\Packages\Nodejs目录下的Nodejs.sublime-build文件
原本如下
```json
{
  "cmd": ["node", "$file"],
  "file_regex": "^[ ]*File \"(...*?)\", line ([0-9]*)",
  "selector": "source.js",
  "shell": true,
  "encoding": "cp1252",
  "windows":
    {
        "shell_cmd": "taskkill /F /IM node.exe & node $file"
    },
    "linux":
    {
        "shell_cmd": "killall node; /usr/bin/env node $file"
    },
    "osx":
    {
        "shell_cmd": "killall node; /usr/bin/env node $file"
    }
}
```
修改为
```json
{
  "cmd": ["C:\\developer\\nodejs\\node.exe", "$file"],
  "file_regex": "^[ ]*File \"(...*?)\", line ([0-9]*)",
  "selector": "source.js",
  "shell": true,
  "encoding": "utf-8",
  "windows":
    {
        "cmd": ["C:\\developer\\nodejs\\node.exe", "$file"]
    },
    "linux":
    {
        "shell_cmd": "killall node; /usr/bin/env node $file"
    },
    "osx":
    {
        "shell_cmd": "killall node; /usr/bin/env node $file"
    }
}
```
并保存，Nodejs.sublime-build文件修改易出错
`注意修改为nodejs的node.exe所在的路径`

**使用方法**
`ctrl+b快捷键在Sublime Text3编写js脚本并用nodejs执行脚本`
`ctrl+shift+c快捷键取消运行脚本`

***
## Sublime Text3的nodejs语法提示插件
Node Completions
