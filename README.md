# Easy Development Environment - Nginx
容易搭建的开发环境 - Nginx Web 服务器
- Docker
- Certificate


## 使用说明
### 先创建一个 `dev` 网络，所有服务都加入同一网络下便于容器互通
```
docker network create dev
```

### 配置文件
复制 `.env.example` 命名为 `.env`
```
cp .env.example .env
```
根据需要修改 `.env` 里的配置

### 启动容器
```
docker compose up -d
```

### 访问服务
```
http://localhost
or
https://localhost
```

## Debug 模式

在 `docker-compose.yml` 文件的 `nginx` service 中添加以下内容
```
command: [nginx-debug, '-g', 'daemon off;']  
```

重新加载容器
```
docker compose up -d
```

`error_log` 日志级别设置成 `debug`  
*nginx.conf* 
```
error_log /var/log/nginx/nginx.error.log debug;
```

重载nginx
```
docker compose exec nginx nginx -s reload
```

查看debug日志
```
tail -f /var/log/nginx/nginx.error.log
```

## 自签名证书

如果想使用自签名证书, 可以使用[sslcert](https://github.com/runchangneo/ede-sslcert) 来生成   
将生成的证书复制到 *etc/ssl* 目录下，在 `server` 中配置证书  
```
ssl_certificate /ssl/localhost/ssl.crt;
ssl_certificate_key /ssl/localhost/ssl.key;
```

重载nginx

```
docker compose exec nginx nginx -s reload
```