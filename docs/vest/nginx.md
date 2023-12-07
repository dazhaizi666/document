---
sidebar_position: 2
---

# 二、Nginx配置

Nginx添加如下配置：
```bash
server {
    listen 80;
    server_name notice.*;

    location ^~ /notice {
        rewrite ^/notice(.*)$ /api/game/app/publicData$1 break;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_set_header X-NginX-Proxy true;
        proxy_pass http://localhost:8080;
    }

    location / {
        return 403;
    }
}

```