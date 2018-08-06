# 一、npm更新配置
* 1.更新npm到最新版本并查看版本号
```bash
npm install -g npm 
npm -v
```
* 2.淘宝镜像使用方法
```bash
npm config set registry https://registry.npm.taobao.org 
cnpm config set registry https://registry.npm.taobao.org 
yarn config set registry https://registry.npm.taobao.org 
```
* 3.验证是否配置成功
```bash
npm config get registry
```
# 二、vue-cli全局安装及项目创建
* 1.npm 全局安装 vue-cli
```bash
npm i -g @vue/cli
```
* 2.创建项目
```bash
vue create my-project
```
* 3.此处有两个选择
```bash
default (babel, eslint)默认套餐，提供babel和eslint支持
Manually select features自己去选择需要的功能，提供更多的特性选择。比如如果想要支持 TypeScript ，就应该选择这一项。
可以使用上下方向键来切换选项。如果只需要 babel 和 eslint 支持，那么选择第一项，就完事了，静静等待 vue 初始化项目。
vue-cli 内置支持了8个功能特性，可以多选：使用方向键在特性选项之间切换，使用空格键选中当前特性，使用 a 键切换选择所有，使用 i 键翻转选项。
对于每一项的功能，此处做个简单描述：
TypeScript 支持使用 TypeScript 书写源码
Progressive Web App (PWA) Support PWA 支持。
Router 支持 vue-router 。
Vuex 支持 vuex 。
CSS Pre-processors 支持 CSS 预处理器。
Linter / Formatter 支持代码风格检查和格式化。
Unit Testing 支持单元测试。
E2E Testing 支持 E2E 测试。
我选择了 Router，Vuex，CSS Pre-processors，Linter / Formatter
```
