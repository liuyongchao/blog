```
//代码保存自动格式化
{
    "eslint.autoFixOnSave": true,
"eslint.validate": [
  "javascript",{
    "language": "vue",
    "autoFix": true
  },"html",
  "vue"
],
    "editor.quickSuggestions": {
        "strings": true
    },
//配置打开终端自动启动虚拟环境
    "python.pythonPath": "E:\\virtualenv\\rjs\\Scripts\\python.exe",
    "terminal.integrated.shellArgs.windows": ["/k", "E:\\virtualenv\\rjs\\Scripts\\activate"],
//配置终端    
    "terminal.integrated.shell.windows": "C:\\Windows\\System32\\cmd.exe"
}
```

```
// Place your key bindings in this file to overwrite the defaults
[
  {
    // 大写
    "key": "ctrl+u",
    "command": "editor.action.transformToUppercase",
    "when": "editorTextFocus"
},
{
    // 小写
    "key": "ctrl+l",
    "command": "editor.action.transformToLowercase",
    "when": "editorTextFocus"
}
]
```

