# vue-cli编译内存溢出卡死
```bash
"CALL_AND_RETRY_LAST Allocation failed - JavaScript heap out of memory"
```
* 解决方案->修改目录: my-project/node_modules/.bin  找到 vue-cli-service.cmd :添加--max_old_space_size=8192
```bash
@IF EXIST "%~dp0\node.exe" (
  "%~dp0\node.exe"  "%~dp0\..\_@vue_cli-service@3.0.0-rc.12@@vue\cli-service\bin\vue-cli-service.js" %*
) ELSE (
  @SETLOCAL
  @SET PATHEXT=%PATHEXT:;.JS;=;%
  node  --max_old_space_size=8192 "%~dp0\..\_@vue_cli-service@3.0.0-rc.12@@vue\cli-service\bin\vue-cli-service.js" %*
)
```
