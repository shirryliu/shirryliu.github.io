---
layout: post
title:  "搭建更适于acm的vs code"
date:   2019-10-12 22:33
---
今天我好寡(不是

---------

被硬生生卡在freopen上。<br>
在launch.json文件中修改args参数。<br>
添加重定向：
```javascript
"args": [
    "<",
    "${workspaceFolder}/data/data.in",
    ">",
    "${workspaceFolder}/data/data.out"
]
```

则我的launch.json文件目前为：
```javascript
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "type": "chrome",
            "request": "launch",
            "name": "Launch Chrome",
            //"url": "http://localhost:8080",
            "webRoot": "${workspaceFolder}",
            "file": "${file}"
        },
        {
            "type": "node",
            "request": "launch",
            "name": "Launch Program",
            "program": "${workspaceFolder}/${fileBasenameNoExtension}.js"
        },
        {
            "name": "(lldb) Launch",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/${fileBasenameNoExtension}.out", // 将要进行调试的程序的路径
            "args": [
                "<",
                "${workspaceFolder}/${fileBasenameNoExtension}.in",
                ">",
                "${workspaceFolder}/${fileBasenameNoExtension}.ans"
            ], // 程序调试时传递给程序的命令行参数，一般设为空即可
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

task.json为：
```javascript
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
<br>
<https://cliuyang.cn/2019/07/12/VScode-C-Algorithmic-Competition/index.html>