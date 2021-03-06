# docker搭建FastDFS

> 资料参见：
>
> +  [使用docker搭建FastDFS文件系统](https://www.cnblogs.com/yanwanglol/p/9860202.html) 

+ 构建`tracker`服务

  ```shell
  docker run -d --network=host --name tracker -v /var/fdfs/tracker:/var/fdfs delron/fastdfs tracker
  ```

  + `-v`：本机`/var/fdfs/tracker`映射到容器中`/var/fdfs`

+ 构建`storage`服务

  ```shell
  docker run -d --network=host --name storage -e TRACKER_SERVER=ip:22122 -v /var/fdfs/storage:/var/fdfs -e GROUP_NAME=cxfwlm delron/fastdfs storage
  ```

  + `TRACKER_SERVER`：指定`tracker`服务的ip、端口，

    因为前面指定了`--network=host`，所以容易与宿主机共享ip（MAC中不是），所以这里的`ip`需替换为本机ip

  + `-v`：本机`/var/fdfs/storage`映射到容器中`/var/fdfs`

  + `GROUP_NAME`：指定fastdfs中使用的分组名称(可选项，不指定默认分组名称为`group1`)

  + 无法重启storage删除`/var/fdfs/storage/data`目录下的`fdfs_storaged.pid`

+ 进入`storage`容器

  > 如果构建`storage`服务时没有指定`GROUP_NAME`，可以不进行这步

  ```shell
  docker exec -it storage /bin/bash
  ```

  + 修改`/etc/fdfs/mod_fastdfs.conf`文件

    修改该文件中`group_name`为上面设置的分组名称

    ```shell
    group_name=cxfwlm
    ```

  + 配置nginx

    配置文件在`/usr/local/nginx/conf/nginx.conf`

    ```shell
    server {
            listen 8888;
            server_name     192.168.0.236;
            location /cxfwlm/M00/ {
                    ngx_fastdfs_module;
            }
        }
    ```

    重启nginx

    ```shell
    $ /usr/local/nginx/sbin/nginx -s reload
    ```

+ 测试

  + 上传文件

    将1个图片上传至本机的`/var/fdfs/storage`下，在容器中执行如下命令

    ```shell
    $ /usr/bin/fdfs_upload_file /etc/fdfs/client.conf /var/fdfs/cumt.png
    cxfwlm/M00/00/00/wKgA7Fx3n7SADmgwAAAvfqYsMyc295.jpg
    ```

  + 在浏览器中输入`192.168.0.236:8888/cxfwlm/M00/00/00/wKgA7Fx3n7SADmgwAAAvfqYsMyc295.jpg`查看图片