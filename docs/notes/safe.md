---
date: 2021-10-10
title: 安全预防
tags:
  - Web攻击
  - CSRF
describe: 安全预防
---

# 安全预防

常见的Web攻击

- SQL注入
- XSS
- CSRF
- 网络攻击

## SQL 注入

使用sequelize可以防止，不用刻意去解决。
PS: 只要不裸写sql语句，常见的数据库工具，都可以解决sql注入的问题

## XSS攻击 (前端来做)

通过vue和React可以防止。H5也可以通过vue ssr防止。

 - vue中要输出原生html需要用 v-html
 - React中要使用 dangerouslySetInnerHTML

如果想要单独处理，可使用 https://github.com/component/escape-html 或者xss <br />
前两天我就发现 segmentfault.com 暴露了一个类似的bug。

## CSRF跨站请求伪造

现有的逻辑，可以很好的预防CSRF攻击

 - 提交数据都使用post请求
 - jwt有token检查

如果想要做单独处理，可参考 https://github.com/expressjs.csurf

## 附：优化请求

使用 helemt 优化请求头

网络安全问题，你可以通过代码来处理，但http等协议标准，以及浏览器，他们也在大力合作来推动网络安全，制定一些规范。<br />
这些安全相关的细节，跟我们日常开发的业务相关性不大，我们也没必要去关注这些细节。但你要知道如何去使用它。

参考 [附-http请求头](https://www.baidu.com)

## 总结

 - SQL注入
 - XSS攻击
 - CSRF 跨站请求伪造
 - helmet 优化请求头