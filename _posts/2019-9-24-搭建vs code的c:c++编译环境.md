---
layout: post
title:  "搭建vs code的c/c++编译环境"
date:   2019-09-24
---
搭建vs code的c/c++编译环境
===
---------
**卸载vs所有的插件**
<br>
  
`sudo rm -rf $HOME/Library/Application\ Support/Code` 
`sudo rm -rf $HOME/.vscode`  
<br>

---------
**配置编译文件tasks.json**  

```
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
        {
            "label": "g++",
            "type": "shell",
            "command": "g++",
            "args": [
                "-g",
                "${file}",
                "-o",
                "${fileBasenameNoExtension}.out"
            ],
            "problemMatcher": {  //正则匹配，删除无用符号
                "owner": "cpp",
                "fileLocation": ["relative", "${workspaceRoot}"],
                "pattern": {
                    "regexp": "^(.*):(\\d+):(\\d+):\\s+(warning|error):\\s+(.*)$",
                    "file": 1,
                    "line": 2,
                    "column": 3,
                    "severity": 4,
                    "message": 5
                }
            }
        }
    ]
}
```

---------
**添加“launch.json”文件配置**

```
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(lldb) Launch",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/${fileBasenameNoExtension}.out", // 将要进行调试的程序的路径
            "args": [], // 程序调试时传递给程序的命令行参数，一般设为空即可
            "stopAtEntry": false, // 设为true时程序将暂停在程序入口处，一般设置为false
            "cwd": "${workspaceFolder}", // 调试程序时的工作目录，一般为${workspaceRoot}即代码所在目录
            "environment": [],
            "externalConsole": true, // 调试时是否显示控制台窗口，一般设置为true显示控制台
            "MIMode": "lldb",
            "preLaunchTask": "g++", // 调试会话开始前执行的任务，这里为认为名字
            "setupCommands": [
                {
                    "description": "Enable pretty -printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ]
        }
    ]
}
```
---------
快捷键“F5”编译运行。<br>
手动编译快捷键“⇧⌘B”。

---------
>https://blog.csdn.net/deaidai/article/details/82955010