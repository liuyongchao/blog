# 1.安装node.js
```
1.下载解压
wget https://nodejs.org/dist/v8.12.0/node-v8.12.0-linux-x64.tar.xz
tar -zxvf node-v8.12.0-linux-x64.tar.xz

2.在/etc/profile中配置好path环境变量
PATH="*:node-v8.12.0-linux-x64/bin:"

3.验证
node -v
```
# 2.下载elasticsearch-head
```
git clone https://github.com/mobz/elasticsearch-head
```
# 3.安装elastichsearch-head插件
```
cd elastichsearch-head
npm install
```
# 4.启动elastichsearch-head
```
npm run start
```
# 5.验证elastichsearch-head
```
http://127.0.0.1:9100/
```
# 6.报错
```
1.跨域，elastichsearch.yml添加一下内容
#allow origin
http.cors.enabled: true
http.cors.allow-origin: "*"

2.elastichsearch-head不能放在plugins目录
```
