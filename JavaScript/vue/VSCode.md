# 保存自动格式化符合ESLint规范
* 1.VSCode下载安装ESLint插件
* 2.下载安装Vetur
* 3.配置user.settings文件
```bash
{
  "eslint.autoFixOnSave": true,
  "eslint.validate": [
    "javascript",
    {
      "language": "vue",
      "autoFix": true
    },
    "html",
    "vue"
  ],
  "editor.quickSuggestions": {
    "strings": true
  }
}
````
# element-ui插件vscode-element-helper提示及妹纸查看官网API
# Debugger for Chrome调试插件
## 三种配置方式
* 1.launch本地开启http服务调试
* 2.attach远程调试，需要chrome连接远程服务器，所以在浏览器启动时加上--remote-debugging-port=9222;这个选项是要连接到远程调试服务器上的，如果不是chrome内核，就没法连接，还有被墙的话也可能会有影响;attach选项不能设置runtimeExecutable这个远程调用是chrome特有的，不允许自己设置浏览器，具体做法就是windows下：找到浏览器图标->右键单击->选择属性->在目标这一栏加上--remote-debugging-port=9222;Linux：在启动命令后面直接加上就ok
```bash
{
    "version": "0.2.0",
    "configurations": [{
            "type": "chrome",
            "request": "launch",
            "name": "Launch Chrome against localhost",
            "url": "http://localhost:8080",
            "webRoot": "${workspaceRoot}"
        },
        {
            "type": "chrome",
            "request": "attach",
            "name": "Attach to Chrome",
            "port": 9222,
            "webRoot": "${workspaceRoot}"
        },
        {
            "name": "Launch index.html (disable sourcemaps)",
            "type": "chrome",
            "request": "launch",
            "sourceMaps": false,
            "file": "${workspaceRoot}/jsTest/test1/test1.html"  #每次需要修改这里的文件地址
        }
    ]
}
```
