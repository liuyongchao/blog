# seafile重新启动服务

```
docker run -t -i   -p 10001:10001   -p 12001:12001   -p 80:8000   -p 8080:8080   -p 8082:8082   -v /srv/seafile:/opt/seafile jenserat/seafile -- /bin/bash
```

```
docker run -itd -p 8000:80 -v /app/onlyoffice/DocumentServer/data:/var/www/onlyoffice/Data -v /app/onlyoffice/DocumentServer/logs:/var/log/onlyoffice --restart=always --name office onlyoffice/chinese
```

