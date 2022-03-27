---
date: 2022-03-27
title: 前端工具链
tags:
  - 前端架构师
  - week9
describe: 前端工具介绍
---

## 大话前端工具链

前端的飞速发展：带来了更复杂的项目。
项目的常见需求 - 依赖管理，预编译，构建打包，压缩合并等。
随着项目越来越复杂 - 诞生了**前端工程化**
随着工程化的产生 - 产生了对应的**前端工具链**



### 静态类型语言

动态语言的弊端

- **typescript**
- flow



### 代码风格检查 Linter

多人协作的弊端，风格各异，维护和扩展的困难

- eslint



### 包管理器

- npm
- yarn - 兼容 npm registry



### 转译器 Traspiler

非 JS 或 不同版本的 JS 翻译成 符合平台要求的等价代码

- Babel



### 开发服务器

- live reload
- HMR



### 打包工具 Bundler

将源代码转换成符合生产环境的代码

- Webpack - Loader， Plugin，大而全的功能
- Rollup - 专注于打包 输出多种格式
- Parcel - 零配置



### 任务管理工具 Task Runner

自动执行项目所需的重复任务

- CSS 预处理
- 优化图片
- 合并 压缩 Javascript
- 文件处理（拷贝，删除）
- 监听文件变化
- Gulp - 流式管道写法组合多个任务
- Webpack - 通过插件的方式
- npm scripts 或者 Shell 脚本



### 脚手架 Scaffolding tools

**将工具链聚合在一个工具内** 简单，快速，零配置

- Vue - Vue CLI，Vite， Imooc CLI
- React - create-react-app
- Angular - Angular CLI