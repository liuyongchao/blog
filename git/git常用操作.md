



# git clone

```
#执行git clone SSL certificate problem 的解决办法
git config --global http.sslVerify false
```



# git取消已经add的文件

```bash
git rm -r --cached .   #删除追踪状态
git add . 
git commit -m "fixed untracked files"
git clean -f   #删除 untracked files
git clean -fd  #连 untracked 的目录也一起删掉 
git clean -xfd #连 gitignore 的untrack 文件/目录也一起删掉 （慎用，一般这个是用来删掉编译出来的 .o之类的文件用的） 
# 在用上述 git clean 前，墙裂建议加上 -n 参数来先看看会删掉哪些文件，防止重要文件被误删
git clean -nxfd
git clean -nf
git clean -nfd
#放弃本地所有新增、删除和修改
git checkout . && git clean -df
```

# 切换分支

```bash
[naxxm@19:06:05]~$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev
  remotes/origin/master
#检出远程分支并切换
[naxxm@19:06:05]~$ git checkout -t origin/dev
#创建本地分支
git checkout -b origin/dev
```

## 删除本地和远程分支

```bash
#第一种方法
#删除本地分支
git branch -d <branch name>
#删除远程分支
git push origin  :<branch name>
#第二种方法
git push origin -d <branch name>
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

# git pull 指定分支

```bash
#git pull = git fetch(默认master当前分支更新到最新) + git merge
git pull origin feature/generated-pages:feature/four-step
```

