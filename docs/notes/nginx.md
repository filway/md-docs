---
date: 2021-10-10
title: nginx配置
tags:
  - nginx
  - https
describe: nginx配置相关
---

## nginx配置示例

### 脚手架发布静态页面 history 模式

```yaml
#以下属性中，以ssl开头的属性表示与证书配置有关。
server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    #配置HTTPS的默认访问端口为443。
    #如果未在此处配置HTTPS的默认访问端口，可能会造成Nginx无法启动。
    #如果您使用Nginx 1.15.0及以上版本，请使用listen 443 ssl代替listen 443和ssl on。
    server_name h5maker.filway.cn; #需要将yourdomain.com替换成证书绑定的域名。
    index index.html index.htm;
    ssl_certificate cert/5800294_h5maker.filway.cn.pem;  #需要将cert-file-name.pem替换成已上传的证书文件的名称。
    ssl_certificate_key cert/5800294_h5maker.filway.cn.key; #需要将cert-file-name.key替换成已上传的证书密钥文件的名称。
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    #表示使用的加密套件的类型。
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2; #表示使用的TLS协议的类型。
    ssl_prefer_server_ciphers on;
    root /root/upload;
    autoindex on;
    add_header Cache-Control "no-cache, must-revalidate";
    index index.php index.html index.htm;

    location / {
      add_header Access-Control-Allow-Origin *;
    }

    location /test2 {
         try_files $uri $uri/ @router;
	 index index.html;
    }

    location @router {
      rewrite ^.*$ /test2/index.html last;
    }

    location ~ /\.ht {
        deny all;
    }

    location /.well-known/acme-challenge/ {
        root /var/www/letsencrypt/;
        log_not_found off;
    }

    error_log logs/h5maker_error.log;
    access_log logs/h5maker_access.log;
}

server {
    listen 80;
    server_name h5maker.filway.cn; #需要将yourdomain.com替换成证书绑定的域名。
    rewrite ^(.*)$ https://$host$1; #将所有HTTP请求通过rewrite指令重定向到HTTPS。
    location / {
        index index.html index.htm;
    }
}
```