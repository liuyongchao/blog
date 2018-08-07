# vue + nginx + 后端工作原理
* 1.nginx配置文件
```bahs
server {
        listen       8080;#监听8080端口
        server_name  localhost;#监听的主机ip,此时为客户端(存有静态资源)（客户端与nginx为同一台机器）
        location / {
            root   D:\project\vue-rjs\dist;//静态资源路径
            index  index.html index.htm;
			# 此处用于处理 Vue、Angular、React 使用H5 的 History时 重写的问题
            if (!-e $request_filename) {
                rewrite ^(.*) /index.html last;
                break;
            }
        }
		# 代理服务端接口
        location /api/ {
            proxy_pass http://localhost:80/; #代理请求接口地址
        }
       }
```
* 2.vue中axios请求路径为：
```bash
return request({
    url: "/api/login",
    method: "post",
    data: {
      username,
      password,
      remember
    }
  });
}
```
* 3.客户端实际请求地址为http://localhost:8080/api/login,nginx反向代理后，后端实际收到的请求地址为：http://localhost:80/login

# redis处理session共享架构图
*|---客户端1　　　　　　　　　　|服务器1             
        
           
*|---客户端2　--- Nginx　---　|服务器2　---Redis
          
       
*|---客户端3　　　　　　　　　|服务器3
