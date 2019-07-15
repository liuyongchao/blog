# seafile安装部署

## 启动

```
docker run -t -i \
  -p 10001:10001 \
  -p 12001:12001 \
  -p 80:8000 \
  -p 8080:8080 \
  -p 8082:8082 \
  -v /var/seafile:/opt/seafile \
  jenserat/seafile -- /bin/bash
```

```
docker run -d \
  --name seafile \
  -p 10001:10001 \
  -p 12001:12001 \
  -p 80:8000 \
  -p 8080:8080 \
  -p 8082:8082 \
  -v /var/seafile1:/opt/seafile \
  -e autostart=true \
  jenserat/seafile
```

