---
date: 2022-03-28
title: 项目整体搭建
tags:
  - 前端架构师
  - week9
describe: 使用ESLint添加代码规范, 深入ESLint配置文件, 使用Prettier自动格式化, 项目结构规范, 了解Gi t Flow标准, SPA路由的基本原理, 状态管理工具，Vuex安装和基础使用
---



## 使用ESLint添加代码规范

**规范的代码格式可以让整个工作的效率在一定程度上提升到最高**



### 没有规范可能出现的问题

- 代码难以读懂
- 代码提交的时候会有很多格式问题的修改，造成不必要的时间消耗。



### ESLint 是什么？

> 是一个开源的 JavaScript 的 linting 工具，使用 espree 将 JavaScript 代码解析成抽象语法树 (AST)，然后通过AST 来分析我们代码。



### 命令行工具

```bash
npx eslint --version
npx eslint --ext ts,vue src/**
```



### 编辑器插件

[ESLint 插件](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) <br />
[Vetur 插件](https://marketplace.visualstudio.com/items?itemName=octref.vetur)



## 深入ESLint配置文件

### Rules

[ESLint 可用的 Rules](https://eslint.org/docs/rules/)

一个 rule 有三个等级 0， 代表关闭，1代表 warning，输出警告，但是不是错误，2 代表错误，会直接抛出错误。这三个数字也可以使用单词来代表，分别是 off warn 和 error。

```javascript
rules: {
	'semi': 2
}


代码块123
```

一个 rule 在一些情况下还有配置的选项，比如分号，可以选择加的情况，就是刚才我们测试的情况，还可以选择不加的情况，这是另外一个分支。
[关于分号的Rule 详情](https://eslint.org/docs/rules/semi)

```javascript
// 第一个还是rule 的等级，第二项是选项名称
rules: {
	'semi': ['error', 'never'],
}


代码块1234
```



### Extends

一系列的规则作为一组。大家可以把这一组拿来用到自己的项目中，那么这个就是 extends 字段，是一个数组，里面是几个项目，其实在 extends 中的每一个字段，都是一组规则。

[Vue3 essential 规则组](https://eslint.vuejs.org/rules/#priority-a-essential-error-prevention-for-vue-js-3-x)



### 自动修正错误

在 vscode 中自动 lint 错误的做法

- 方法一： cmd+shift+p 呼出快捷操作菜单。然后运行 ESLint：Fix all auto-fixable Problems
- 方法二：在 settings.json 编辑器配置文件中写入：

```json
"editor.codeActionsOnSave": {
  "source.fixAll.eslint": true
},


代码块123
```

**注意的是不是每一个错误都是有修复规则的，有一些是无法修复的**



##  使用Prettier自动格式化

**Prettier** 顾名思义，将代码变漂亮！



### ESLint 的功能

- 代码质量问题
- 代码风格问题



### Prettier 的理念

- **An opinionated code formatter**，格式很重要，但是很重要，让我来帮你搞定！
- Fewer Options：Prettier 还给予了一部分配置项，可以通过 .prettierrc 文件修改。



### Prettier 安装

**CLI命令行安装使用**
[文档地址](https://prettier.io/docs/en/install.html)

**编辑器插件安装使用**
[插件地址](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
**编辑器快捷键**
[配置详情](https://github.com/prettier/prettier-vscode)

```
1 CMD + Shift + P -> Format Document
2 Shift + Option + F


代码块12
```

修改默认的格式化工具到 Prettier，在 settings.json 中添加：

```javascript
{
	// 所有文件
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  // 特定类型文件
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}



代码块123456789
```

保存后自动修改：

```javascript
// Set the default
"editor.formatOnSave": false,
// Enable per-language
"[javascript]": {
    "editor.formatOnSave": true
}


代码块123456
```



### ESLint prettier 的工作原理

[文档地址](https://prettier.io/docs/en/integrating-with-linters.html)

- 禁用所有和Prettier 冲突的 ESLint 的代码格式规则
- 将所有 Prettier 的规则和修改导入 ESLint 中，在 ESLint 统一的显示这些错误。

**支持 Vue ESLint 的 Prettier 插件**
[文档地址](https://www.npmjs.com/package/eslint-plugin-prettier-vue)



### 支持 Prettier 工具的代码库分支

https://git.imooc.com/class-110/lego/src/feature/prettier



## 项目结构规范

**代码结构** 针对单个文件的书写格式
**项目结构** 针对这些文件应该以怎样的标准进行存放和管理。

[React 项目文件结构](https://reactjs.org/docs/faq-structure.html)

- 按照按功能或路由组织，也就是所说的 feature
- **按照文件类型**



### 注意事项

- 避免多层嵌套
- 不要过度思考



### 项目结构举例

```bash
/assets
	image.png
  logo.png	
/components
	ColorPicker.vue (使用 Pascal 命名方式)
  Dropdown.vue
  ...
/views
	Home.vue （使用 Pascal 命名方式）
    ...
/router
	index.ts
	...
/store
	index.ts
	editor.ts
	user.ts
	...
/hooks
	useURLLoader.ts （ 以use开头，使用驼峰命名方式 ）
    ...
/plugins
	hotKeys.ts （使用驼峰命名方式）
	...
/test
	ColorPicker.spec.ts （使用Pascal命名方式，和组件名称相同，以 spec.ts 结尾）
App.vue
main.ts
...
```



## Git 标准操作流程：Git Flow

所有的这些规范都是针对特定的**多人**设定的，意在让多人协作的过程更顺畅，更简单，减少不必要的冲突和时间的浪费。



### Git Flow

![图片描述](https://oss.filway.cn/filway-blog/601650f7098c267006850425.jpg)

**预设两个分支**

- master 只能用来包括产品代码。你不能直接工作在这个 master 分支上
- develop 是你进行任何新的开发的基础分支。

**这两个分支被称之为长期分支**

![图片描述](https://oss.filway.cn/filway-blog/601651210912858f07000454.jpg)

- 功能开发 feature
  - 整合回到 develop
  - 等待更全面的测试
  - 等待和 develop 一起进行发布
- 管理 release
  - 新功能已经添加，bug已经修复
  - 代码已经被测试
  - release 分支使用版本号命名的
- bug 修复 hotfix
  - 针对 master 分支

**优点**： 清晰可控

**缺点**：相对复杂，不适合持续发布。



### Github Flow

[Github Flow 官方文档](https://guides.github.com/introduction/flow/index.html)

![图片描述](https://oss.filway.cn/filway-blog/60165133090f7cb716000402.jpg)

- 根据需求，从 master 拉出分支
- 激烈的开发阶段，提交 commit
- 开发完毕，发起 PR（pull request）
- 代码评审（很重要！）
- 部署，并且测试
- 没问题， merge 到 master！

**Github flow 的最大优点就是简单，对于"持续发布"的产品，可以说是最合适的流程。**



### 两大规则

- branch 命名
  - feature 开头代表功能开发
  - hotfix 开头代码bug修复
- commit 信息，必须 **言之有物**，杜绝 **update，fix bug** 这类废话



## 安装第三方组件库



### 两大解决方案

- **[ant-deisgn-vue](https://2x.antdv.com/docs/vue/getting-started-cn/)**
- [element-plus](https://element-plus.gitee.io/)



### 安装方式

- npm 安装 配合 webpack 等打包工具使用

```bash
npm install ant-design-vue@next --save
```

- 直接引入 js 和 css (不推荐)

```html
<!-- 引入样式 -->
<link rel="stylesheet" href="https://unpkg.com/element-plus/lib/theme-chalk/index.css">
<!-- 引入组件库 -->
<script src="https://unpkg.com/element-plus/lib/index.full.js"></script>
```



### 开始使用

```typescript
// main.ts
import Antd from 'ant-design-vue'
import 'ant-design-vue/dist/antd.css'
app.use(Antd)
```



### 关于全局 API

Vue2 的时代，都是全局API 和 配置

```javascript
Vue.component('button-counter', {})
Vue.directive('focus', {})
```

Vue 2 没有“app”的概念，我们定义的应用只是通过 new Vue() 创建的根 Vue 实例。从同一个 Vue 构造函数创建的每个根实例共享相同的全局配置。

- 在测试期间，全局配置很容易意外地污染其他测试用例。
- 全局配置使得在同一页面上的多个“app”之间共享同一个 Vue 副本非常困难。

```javascript
Vue.mixin({
  /* ... */
})
const app1 = new Vue({ el: '#app-1' })
const app2 = new Vue({ el: '#app-2' })
```

**Vue3 的做法: createApp**

```javascript
import { createApp } from 'vue'
const app = createApp({})
```

任何全局改变 Vue 行为的 API 现在都会移动到**应用实例**上，现在就不会产生 vue2 的这些冲突了。



## SPA 应用路由的基本原理



### SPA 和普通网页应用的区别

**普通网页**

- 跳转到新页面，每次重新加载所有资源
- HTML 内容是后端直接渲染的

**SPA 应用**

- 不跳转，JS 拦截跳转，修改 URL
- JS 动态渲染 DOM 内容



### SPA 路由的实现方式

**History API**

[pushState 文档地址](https://developer.mozilla.org/zh-CN/docs/Web/API/History_API#添加和修改历史记录中的条目)

[codesandbox 演示地址](https://codesandbox.io/s/optimistic-cookies-wgxhc?file=/src/index.js)

**URL Hash**



### SPA优点

- 速度快
- 体验好
- 为前后端分离提供了实践场所



### Vue Router 4

[文档地址](https://next.router.vuejs.org/)



## 什么是状态管理工具

首先搞清楚，是否**真的**需要状态管理工具。

它随着 SPA 的出现，而浮出水面。客户端需要处理复杂的状态数据。

多个组件需要共享的一系列数据，称之为 **全局数据**。



### 解决方案

**方案一：单向数据流，从父组件传递到子组件**

![图片描述](https://oss.filway.cn/filway-blog/601651e20951431e07920678.jpg)

#### 缺点

- 多层传递非常繁琐
- 中间传递层有可能根本不需要这个数据
- 根组件压力太大 逻辑代码会非常繁杂



### 方案二 使用全局对象

![图片描述](https://oss.filway.cn/filway-blog/601651ed098d8b9408920602.jpg)

#### 缺点

- 数据非响应式
- 修改无法追踪
- 直接从组件获取数据是一种反模式



### 状态管理工具三杰

- [Vuex](https://next.vuex.vuejs.org/)
- [Redux](https://redux.js.org/)
- [Mobx](https://mobx.js.org/)

![图片描述](https://oss.filway.cn/filway-blog/601651f909f543c009380580.jpg)

#### 状态管理工具的特点

- store，神奇的全局数据结构：single source of truth
- 不能随意修改，调用特殊的方法来实现数据修改
- 变化可追溯，可预测（predictable）。



## Vuex 安装和基础



### Vue 的数据流

```javascript
const Counter = {
  // state
  data () {
    return {
      count: 0
    }
  },
  // view
  template: `
    <div>{{ count }}</div>
  `,
  // actions
  methods: {
    increment () {
      this.count++
    }
  }
}
```

![图片描述](https://oss.filway.cn/filway-blog/6016523f091766b206400433.jpg)



### 出现问题：多组件共享状态

- 根组件多层传递的困境
- 多组件同步数据的繁琐



### Vuex 的解决方案

![图片描述](https://oss.filway.cn/filway-blog/6016524a0904d0e707010551.jpg)
**Vuex 的特点**

- 核心就是一个 store
- Vuex 的状态是响应式的
- 不能直接改变 store 中 state 的值，需要显式的提交 Mutation。

**安装**

```bash
npm install vuex@next --save


代码块1
```

**初步使用**

```javascript
const store = createStore({
  state: {
    count: 0
  },
  mutations: {
    add (state) {
      state.count++
    }
  }
})
store.commit('add')
console.log(store.state.count)
```



## 本周总结



### 知识点

- 脚手架
  - Imooc CLI
  - Vue CLI
  - Vite
- 代码规范
  - ESLint 简介，使用和配置文件
  - Prettier 的定位和作用以及使用
  - 产出：代码规范文档：https://www.yuque.com/docs/share/b8d81a2b-2700-4e9c-8fcd-1d2ee52faf7c?#
  - 产出：文件结构和命名规范文档：https://www.yuque.com/docs/share/fb9f53ef-033a-448e-84b1-a046d8dd1794?# 《B 端项目文件结构规范》
- Git Flow
  - 标准 Git Flow
  - **Github Flow**
  - 产出 Git 操作规范文档：https://www.yuque.com/docs/share/a3ca5bc4-3f99-4b69-a485-dc029c8494da?# 《B 端 Git flow 规范》
  - Ant Design Vue
  - 安装，全局导入和使用
  - 多看文档，多用
- Vue Router
  - SPA router 的简单原理
  - 安装，全局引入
  - 添加路由，使用内置组件 router-view 和 router-link
  - 使用钩子函数 useRoute 和 useRouter 获取路由信息和跳转
- Vuex
  - 全局状态工具的定义
  - 安装，概念：** store，state，mutations**，使用 commit 提交 mutations
  - 使用钩子函数 useStore
  - 使用 module 将 store 拆分成多个模块。



### 要点

- **文档化** 标准
- **Typescript** 深入编码理念中



### 作业

- 根据我的介绍，写自己的**技术规范文档**，你可以根据我的简介和自己的偏好，写出适合自己项目的文档



## B 端项目编码规范

B 端项目使用 **ESLint** 进行编码规范的约束



### 必须采用的规则合集

在项目中目前已经使用的规范集有：**（这三个规则合集 建议必须开启）**

```typescript
  'extends': [
    'plugin:vue/vue3-essential',
    'eslint:recommended',
    '@vue/typescript/recommended'
  ],
```

这三个规则合集正好约束了项目对应的三种语言类型：Vue 文件编码，js 通用编码规范，typescript 编码规范。

### 规则详情



如果想了解规则详情：

请参看如下链接

vue3-essential 规则合集（关于 vue3 的一些规范合集）：https://eslint.vuejs.org/rules/

eslint recommended（eslint 推荐的 js 编码规范合集）： https://eslint.org/docs/rules/

vue/typescript/recommended（typescript 编码规范合集）： https://github.com/typescript-eslint/typescript-eslint/tree/master/packages/eslint-plugin



### 推荐插件



请使用 vscode 并安装 eslint， vetur 插件，在编辑器内获得显示错误的能力。

[ESLint 插件](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)

[Vetur 插件](https://marketplace.visualstudio.com/items?itemName=octref.vetur)



### 推荐其他规则



按照个人或者团队意愿，可以选择一些扩充的 js 规范作为补充。本项目并没有采用。备选方案如下：

Airbnb Javascript 编码规范（Github 103k星）

Airbnb ESLint 合集：https://www.npmjs.com/package/eslint-config-airbnb

规则详情：https://github.com/airbnb/javascript#types



### 关于 Prettier 的使用

可以使用 Prettier 对代码进行优化，但是本项目并没有采用，同学们可以依据自己的爱好进行选择。创建了一个分支，可以将 Prettier 和 ESLint 在一起进行良好的工作。同学们可以参考：

https://git.imooc.com/class-110/lego/src/feature/prettier





## B 端项目文件结构规范

**首先：没有任何强制的标准！**



### 参考



React 项目规范标准：[React 项目文件结构](https://reactjs.org/docs/faq-structure.html)



### 项目文件结构和命名规范

```bash
/assets （静态文件）
	image.png
  logo.png	
/components （所有组件）
	ColorPicker.vue (使用 Pascal 命名方式)
  Dropdown.vue
  ...
/views （所有路由）
	Home.vue （使用 Pascal 命名方式）
    ...
/router （路由配置文件）
	index.ts
	...
/store （全局 store 配置文件）
	index.ts
	editor.ts
	user.ts
	...
/hooks （vue钩子函数）
	useURLLoader.ts （ 以use开头，使用驼峰命名方式 ）
    ...
/plugins （插件）
	hotKeys.ts （使用驼峰命名方式）
	...
/test （测试文件）
	ColorPicker.spec.ts （使用Pascal命名方式，和组件名称相同，以 spec.ts 结尾）
App.vue
main.ts
...
```



### 注意事项



- 避免多层嵌套
- 不要过度思考





## B 端 Git flow 规范

### 规范



**采用 Github Flow** 

**Github Flow 文档地址：**https://guides.github.com/introduction/flow/index.html



**图片描述：**



![img](https://oss.filway.cn/filway-blog/1609063340633-6a4687b3-2d7b-4d47-8b9e-f43687fb7f27.png)

**文字过程描述：**

- 根据需求，从 master 拉出分支
- 激烈的开发阶段，提交 commit
- 开发完毕，发起 PR（pull request）
- 代码评审（很重要！）
- 部署，并且测试
- 没问题， merge 到 master！



### 分支命名 

分支命名规范属于**强制**采用的规范

- feature 开头代表功能开发 如 feature/add-vuex
- hotfix 开头代表代码 bug 修复 如 hotfix/fix-header



### Commit 信息

Commit 信息，杜绝 **update，fix bug** 这类废话，每次提交必须**言之有物**，至少要言简意赅的把一次 commit 完成的任务说清楚。