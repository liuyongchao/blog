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
注意：window模式下，用cmd命令行运行，否则无法进行选择（在当前文件夹下`shift + 右键`，选择‘在此处打开命令窗口’）
```
* 3.此处有两个选择,通过`↑↓`选择Manually select features=>`enter`键
```bash
Vue CLI v3.0.0-rc.10
? Please pick a preset: (Use arrow keys)
 default (babel, eslint)  默认配置
>Manually select features手动配置
```
* 4.通过`↑↓`选择所需要内容，按`spacebar`空格键选定，然后按`enter`键，进入下一步针对每一项详细设置
```bash
Vue CLI v3.0.0-rc.10
? Please pick a preset: Manually select features
? Check the features needed for your project:
 (*) Babel
 ( ) TypeScript
 ( ) Progressive Web App (PWA) Support
 (*) Router
 (*) Vuex
 (*) CSS Pre-processors
>(*) Linter / Formatter
 ( ) Unit Testing
 ( ) E2E Testing
 ```
* 5.检查项目所需的功能，路由是否使用历史模式，选择N
```bash
Vue CLI v3.0.0-rc.10
? Please pick a preset: Manually select features
? Check the features needed for your project: Babel, Router, Vuex, CSS Pre-processors, Linter
? Use history mode for router? (Requires proper server setup for index fallback in production) (Y/n)
```
* 6.选择一个CSS预处理器,选择 SCSS/SASS
```bash
? Pick a CSS pre-processor (PostCSS, Autoprefixer and CSS Modules are supported by default): (Use arrow keys)
> SCSS/SASS
  LESS
  Stylus
```
* 7.选择linter / formatter配置,选择ESLint + Prettier
```bash
? Pick a linter / formatter config:
  ESLint with error prevention only
  ESLint + Airbnb config
  ESLint + Standard config
> ESLint + Prettier
```
* 8.选择其他lint功能:(按`<space>`选择，`<a>`切换全部，`<i>`反转选择）
```bash
? Pick additional lint features: (Press <space> to select, <a> to toggle all, <i> to invert selection)
>(*) Lint on save保存检查
 ( ) Lint and fix on commit检查修复提交
```
* 9.为Babel，PostCSS，ESLint等配置保存各自到单文件，还是统一保存到package.json,这里选择In dedicated config files
```bash
? Where do you prefer placing config for Babel, PostCSS, ESLint, etc.? (Use arrow keys)
> In dedicated config files
  In package.json
```
* 10.是否将其保存为未来项目的预设，这里选择N
```bash
? Where do you prefer placing config for Babel, PostCSS, ESLint, etc.? In dedicated config files
? Save this as a preset for future projects? (y/N)
```
* 11.切换到vue-rjs目录，并运行执行
```bash
cd vue-rjs
npm run serve
```




