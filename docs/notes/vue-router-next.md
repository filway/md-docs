---
date: 2021-10-10
title: vue-router-next源码解析
tags:
  - 源码解析
  - vue-router
describe: vue-router-next源码解析
---

# vue-router-next 源码解析
源码仓库：https://github.com/vuejs/vue-router-next

## vue-router 常见问题
- history 和 hash 模式的区别是什么？（涉及 vue-router 路由模式和前端发布原理）
- Vue dev 模式下为什么不需要配置 history fallback？（涉及 webpack-dev-server 配置）
- 我们并没有定义 router-link 和 router-view，为什么代码里能直接使用？（涉及 vue-router 初始化流程和 Vue 插件）
- 浏览器中如何实现URL变化但页面不刷新？（涉及 vue-router history 模式核心实现原理）
- vue-router 如何实现路由匹配？（涉及 vue-router Matcher 实现原理）
- router-view 如何实现组件动态渲染？（涉及 Vue 动态组件）
## vue-router 路由模式
[官网地址](https://next.router.vuejs.org/zh/guide/essentials/history-mode.html)

hash和history模式，区别：
  - 语法结构不同
  - 部署方式不同
  - SEO

## vue-router 源码执行流程分析
### vue-router 初始化流程  