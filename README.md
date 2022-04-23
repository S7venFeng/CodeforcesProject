# CodeforcesProject
### 1. VSCODE

vscode通过.vscode里面的配置来设置编译/调试/运行环境，涉及到的都是.json文件，主要是launch.json(运行)和setting.json(code-runner插件配置)和task.json（编译配置）

这里使用code-runner插件的原因是VS自带的插件写题时必须加上system("pause")等中断函数让运行停止，不然F5运行调试一闪而过非常烦恼。而code-runner对多语言的支持也比较好。

#### tasks.json

```json
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
        {
            // 任务标签名
            "label": "compile",
            // 任务类型
            "type": "shell",
            // 编译器选择
            "command": "C:\\mingw64\\bin\\g++.exe",
            // 编译预执行的命令集
            "args": [
                "-g",
                "\"${file}\"",
                "-o",
                "\"${fileDirname}\\${fileBasenameNoExtension}\""
            ],
            // 任务输出配置
            "presentation": {
                "reveal": "always",
                "panel": "shared",
                "focus": false,
                "echo": true,
                "group": "build"
            },
            // 任务所属组名
            "group": "build",
            // 编译问题输出匹配配置
            "problemMatcher": {
                "owner": "cpp",
                "fileLocation": "absolute",
                "pattern": {
                    "regexp": "^(.*):(\\d+):(\\d+):\\s+(error):\\s+(.*)$",
                    "file": 1,
                    "line": 2,
                    "column": 3,
                    "severity": 4,
                    "message": 5
                }
            }
        },
        {
            "type": "shell",
            "label": "C/C++: g++.exe build active file",
            "command": "C:\\mingw64\\bin\\g++.exe",
            "args": [
                "-g",
                "${file}",
                "-o",
                "${fileDirname}\\${fileBasenameNoExtension}.exe"
            ],
            "options": {
                "cwd": "${workspaceFolder}"
            },
            "problemMatcher": [
                "$gcc"
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            }
        }
    ]
}
```



#### launch.json

```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(Windows) 启动",
            "type": "cppdbg",
            "request": "launch",
            "program": "${fileDirname}\\${fileBasenameNoExtension}.exe",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "MIMode":"gdb",
            "miDebuggerPath": "C:\\mingw64\\bin\\gdb.exe",
            // 不要在集成终端里运行程序
            "externalConsole": true,
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "environment": [],
            // 调试前先运行 compile（定义在上面的 tasks.json 里）
            "preLaunchTask": "compile"
        }
    ]
}
```



### settings.json

```json
{
    "code-runner.saveFileBeforeRun:": true,
    "code-runner.filterDirectorAsCwd:": true,
    "code-runner.runInTerminal": true,
    "code-runner.ignoreSelection": true,
    "code-runner.preserveFocus": false,
    "code-runner.executorMap": {
        "javascript": "node",
        "java": "cd $dir && javac $fileName && java $fileNameWithoutExt",
        "c": "cd $dir && gcc $fileName -o $fileNameWithoutExt && $dir$fileNameWithoutExt",
        "cpp": "cd $dir && C:\\mingw64\\bin\\g++.exe $fileName -o $fileNameWithoutExt -g -Wall -static && $dir$fileNameWithoutExt.exe",
        "objective-c": "cd $dir && gcc -framework Cocoa $fileName -o $fileNameWithoutExt && $dir$fileNameWithoutExt",
        "php": "php",
        "python": "python -u",
        "perl": "perl",
        "perl6": "perl6",
        "ruby": "ruby",
        "go": "go run",
        "lua": "lua",
        "groovy": "groovy",
        "powershell": "powershell -ExecutionPolicy ByPass -File",
        "bat": "cmd /c",
        "shellscript": "bash",
        "fsharp": "fsi",
        "csharp": "scriptcs",
        "vbscript": "cscript //Nologo",
        "typescript": "ts-node",
        "coffeescript": "coffee",
        "scala": "scala",
        "swift": "swift",
        "julia": "julia",
        "crystal": "crystal",
        "ocaml": "ocaml",
        "r": "Rscript",
        "applescript": "osascript",
        "clojure": "lein exec",
        "haxe": "haxe --cwd $dirWithoutTrailingSlash --run $fileNameWithoutExt",
        "rust": "cd $dir && rustc $fileName && $dir$fileNameWithoutExt",
        "racket": "racket",
        "scheme": "csi -script",
        "ahk": "autohotkey",
        "autoit": "autoit3",
        "dart": "dart",
        "pascal": "cd $dir && fpc $fileName && $dir$fileNameWithoutExt",
        "d": "cd $dir && dmd $fileName && $dir$fileNameWithoutExt",
        "haskell": "runhaskell",
        "nim": "nim compile --verbosity:0 --hints:off --run",
        "lisp": "sbcl --script",
        "kit": "kitc --run",
        "v": "v run",
        "sass": "sass --style expanded",
        "scss": "scss --style expanded",
        "less": "cd $dir && lessc $fileName $fileNameWithoutExt.css",
        "FortranFreeForm": "cd $dir && gfortran $fileName -o $fileNameWithoutExt && $dir$fileNameWithoutExt",
        "fortran-modern": "cd $dir && gfortran $fileName -o $fileNameWithoutExt && $dir$fileNameWithoutExt",
        "fortran_fixed-form": "cd $dir && gfortran $fileName -o $fileNameWithoutExt && $dir$fileNameWithoutExt",
        "fortran": "cd $dir && gfortran $fileName -o $fileNameWithoutExt && $dir$fileNameWithoutExt"
    },
    "code-runner.defaultLanguage": "C++",
}
```

项目的代码架构如下：

