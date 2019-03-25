# git取消已经add的文件

```bash
git rm -r --cached .   #删除追踪状态
git add . 
git commit -m "fixed untracked files"
```

# 切换分支

```bash
[naxxm@19:06:05]~/project/uhtp-quickweb$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev
  remotes/origin/master

[naxxm@19:06:05]~/project/uhtp-quickweb$ git checkout -t origin/dev
```

# git注释规范

```
#格式
<type>(<scope>): <subject>

type
feat：新功能（feature）
fix：修补bug
docs：文档（documentation）
style： 格式（不影响代码运行的变动）
refactor：重构（即不是新增功能，也不是修改bug的代码变动）
test：增加测试
chore：构建过程或辅助工具的变动

scope
用于说明 commit 影响的范围，比如数据层、控制层、视图层等等，视项目不同而不同。

subject
是 commit 目的的简短描述，不超过50个字符。
1.以动词开头，使用第一人称现在时，比如change，而不是changed或changes
2.第一个字母小写
3.结尾不加句号（.）
```

