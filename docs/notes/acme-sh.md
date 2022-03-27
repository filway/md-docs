---
date: 2021-10-10
title: 免费泛域名证书申请
tags:
  - 证书
  - 域名
describe: 免费泛域名证书申请
---

## 安装 acme shell
```linux
curl  https://get.acme.sh | sh -s email=my@example.com
```

```linux
alias acme.sh=~/.acme.sh/acme.sh
```

## 生成证书

- dnsapi
  1. 阿里云

  ```linux
  export Ali_Key="sdfsdfsdfljlbjkljlkjsdfoiwje"
  export Ali_Secret="jlsdflanljkljlfdsaklkjflsa"
  ```

  ```linux
  acme.sh --issue --dns dns_ali -d example.com -d www.example.com
  ```
  Ali_Key 和 Ali_Secret 将保存在 ~/.acme.sh/account.conf 中，并在需要时重复使用。

## copy/安装 证书

前面证书生成以后, 接下来需要把证书 copy 到真正需要用它的地方.

注意, 默认生成的证书都放在安装目录下: ~/.acme.sh/, 请不要直接使用此目录下的文件, 例如: 不要直接让 nginx/apache 的配置文件使用这下面的文件. 这里面的文件都是内部使用, 而且目录结构可能会变化.

正确的使用方法是使用 --install-cert 命令,并指定目标位置, 然后证书文件会被copy到相应的位置, 例如:

### Nginx example:

```linux
acme.sh --install-cert -d example.com \
--key-file       /path/to/keyfile/in/nginx/key.pem  \
--fullchain-file /path/to/fullchain/nginx/cert.pem \
--reloadcmd     "service nginx force-reload"
```

Nginx 的配置 ssl_certificate 使用 /etc/nginx/ssl/fullchain.cer ，而非 /etc/nginx/ssl/domain.cer ，否则 SSL Labs 的测试会报 Chain issues Incomplete 错误。

## 更新证书
目前证书在 60 天以后会自动更新, 你无需任何操作. 今后有可能会缩短这个时间, 不过都是自动的, 你不用关心.

## 更新acme shell
目前由于 acme 协议和 letsencrypt CA 都在频繁的更新, 因此 acme.sh 也经常更新以保持同步.

升级 acme.sh 到最新版 :
```linux
acme.sh --upgrade
```

如果你不想手动升级, 可以开启自动升级:
```linux
acme.sh  --upgrade  --auto-upgrade
```

关闭自动更新
```linux
acme.sh --upgrade  --auto-upgrade  0
```

## 出错调试

```linux
acme.sh --issue ....   --debug
```
或
```linux
acme.sh  --issue  .....  --debug  2
```

