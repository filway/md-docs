---
date: 2022-04-30
title: 解决Github Pages自定义域名失效的问题
tags:
  - github page
  - vitepress
  - vuepress
describe: vitepress或者vuepress搭建博客自动部署github pages, 配置自定义域名经常失效出现404的原因
---



## 原因

1. 在github page仓库的settings当中配置自定义域名的时候，会在对应分支的根目录下面自动生成CNAME文件, 内容为配置的自定义域名

2. 自动部署的时候，会将build好的dist目录下的文件全部提交到gh-pages(或者master)分支上面去作为pages的展示内容,  如果CNAME文件没有一起提交的话，就会将之前生成好的CNAME文件覆盖掉，从而导致自定义域名解析生效, 访问就会出现404



## 解决方法

在public目录下面手动新增CNAME文件, 内容为你的自定义域名。然后在推送部署即可