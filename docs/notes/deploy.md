---
date: 2021-10-10
title: 自动部署
tags:
  - 发布上线
  - 自动部署
describe: 前端发布上线相关
---

## 前端发布上线相关

### release-it

1. 配置文件.release-it.js
```javascript
module.exports = {
    git: {
        tagName: 'v${version}',
        commitMessage: 'release: v${version}',
        requireCleanWorkingDir: false,
        requireBranch: 'master',
    },
    hooks: {
        'before:init': ['git pull origin master'],
    },
    npm: {
        publish: false,
    },
    prompt: {
        ghRelease: false,
        glRelease: false,
        publish: false,
    },
    plugins: {
        '@release-it/conventional-changelog': {
            preset: 'angular',
            infile: 'CHANGELOG.md',
        },
    },
  }
  ```
  2. npm脚本

  ```json
  {
    "prd": "cross-env NODE_ENV=production NODE_LOG_DIR=/tmp/admin-server ENABLE_NODE_LOG=YES  pm2 start bin/pm2-prd.config.js",
    "release": "release-it",
    "just-try-release": "release-it --dry-run"
  }
  ```

 ### pm2相关

 1. 配置
  - pm2AppConf.js
  ```javascript
  /**
 * @description pm2 app 配置信息
 */

const os = require('os')

const cpuCoreLength = os.cpus().length // CPU 几核

module.exports = {
    name: '项目名称',
    script: 'bin/www',
    // watch: true,
    ignore_watch: ['node_modules', '__test__', 'logs'],
    instances: cpuCoreLength,
    error_file: './logs/err.log',
    out_file: './logs/out.log',
    log_date_format: 'YYYY-MM-DD HH:mm:ss Z', // Z 表示使用当前时区的时间格式
    combine_logs: true, // 多个实例，合并日志
    max_memory_restart: '300M', // 内存占用超过 300M ，则重启
}
  ```
  - pm2-prd.config.js
  ```javascript
  /**
 * @description pm2 配置文件，线上环境
 * @author 双越
 */

const appConf = require('./pm2AppConf')

module.exports = {
    apps: [appConf],
}
  ```
  - www脚本
  ```javascript
  #!/usr/bin/env node

/**
 * Module dependencies.
 */

const app = require('../src/app')
const debug = require('debug')('demo:server')
const http = require('http')
const syncDb = require('../src/db/seq/utils/sync-alter')

/**
 * Get port from environment and store in Express.
 */

const port = normalizePort(process.env.PORT || '3000')
// app.set('port', port);

/**
 * Create HTTP server.
 */

const server = http.createServer(app.callback())

/**
 * Listen on provided port, on all network interfaces.
 */

// 先同步 mysql 数据表
syncDb().then(() => {
    // 再启动服务
    server.listen(port)
    server.on('error', onError)
    server.on('listening', onListening)
})

/**
 * Normalize a port into a number, string, or false.
 */

function normalizePort(val) {
    const port = parseInt(val, 10)

    if (isNaN(port)) {
        // named pipe
        return val
    }

    if (port >= 0) {
        // port number
        return port
    }

    return false
}

/**
 * Event listener for HTTP server "error" event.
 */

function onError(error) {
    if (error.syscall !== 'listen') {
        throw error
    }

    const bind = typeof port === 'string' ? `Pipe ${port}` : `Port ${port}`

    // handle specific listen errors with friendly messages
    switch (error.code) {
        case 'EACCES':
            console.error(`${bind} requires elevated privileges`)
            process.exit(1)
            break
        case 'EADDRINUSE':
            console.error(`${bind} is already in use`)
            process.exit(1)
            break
        default:
            throw error
    }
}

/**
 * Event listener for HTTP server "listening" event.
 */

function onListening() {
    const addr = server.address()
    const bind = typeof addr === 'string' ? `pipe ${addr}` : `port ${addr.port}`
    debug(`Listening on ${bind}`)
}

  ``` 

### github action
  
  - deploy-prd.yml
  ```yaml
  # This workflow will run tests using node and then publish a package to GitHub Packages when a release is created
# For more information see: https://help.github.com/actions/language-and-framework-guides/publishing-nodejs-packages
# github actions 中文文档 https://docs.github.com/cn/actions/getting-started-with-github-actions

name: deploy production

on:
    push:
        tags:
            - 'v*.*.*'

jobs:
    deploy:
        runs-on: ubuntu-latest

        steps:
            - uses: actions/checkout@v2
            - name: Use Node.js
              uses: actions/setup-node@v1
              with:
                  node-version: 12 # 尽量和线上机 node 版本保持一直
            - name: lint and test # 测试
              run: |
                  npm i
                  npm run lint
                  npm run test:remote
            - name: set ssh key # 临时设置 ssh key
              run: |
                  mkdir -p ~/.ssh/
                  echo "${{secrets.ZX_ID_RSA}}" > ~/.ssh/id_rsa # secret 在这里配置 https://github.com/filway/settings/secrets
                  chmod 600 ~/.ssh/id_rsa
                  ssh-keyscan "${{secrets.IPTOC}}" >> ~/.ssh/known_hosts
            - name: deploy # 部署
              run: ssh work@${{secrets.IPTOC}} "bash -s" < bin/deploy.sh ${{secrets.TOKEN}} ${{github.ref}}
            - name: delete ssh key # 删除 ssh key
              run: rm -rf ~/.ssh/id_rsa

  ```
  - deploy脚本
  ```shell
  #!/bin/bash

# 执行时需传入两个参数：参数1 - github 密码，参数2 - tag ，如 `sh bin/deploy.sh v1.0.1 xxx`
# shell 代码中使用 $1 $2 即可依次获取参数

teamPath="/home/work/xxx-team" # team 目录
repoPath="/home/work/xxx-team/repo" # 项目目录，要和 repo 同名！！！
repoGitUrl="https://$1@github.com/username/repo.git"

if [ ! -d "$teamPath" ]; then
    # 如果 team 目录不存在，则创建
    echo =================== mkdir "$teamPath" ===================
    mkdir "$teamPath"
fi
cd "$teamPath"

if [ ! -d "$repoPath" ]; then
    ## 如果 repo 目录不存在，则 git clone （私有项目，需要 github 用户名和密码）
    echo =================== git clone start ===================
    git clone "$repoGitUrl"
    git remote remove origin; # 删除 origin ，否则会暴露 github 密码
    echo =================== git clone end ===================
fi;
cd "$repoPath"

git checkout . # 撤销一切文件改动，否则可能导致 pull 失败

git remote add origin "$repoGitUrl"
git pull origin master # 下载最新 master 代码，tag 都是基于 master 分支提交的
git fetch --tags # 获取所有 tags 。否则，光执行 git pull origin master 获取不到新提交的 tag
git remote remove origin; # 删除 origin ，否则会暴露 github 密码

# 切换到 tag ，重要！！！
git checkout "$2"
echo =================== git checkout "$2" ===================

# 安装依赖
npm install --registry=https://registry.npm.taobao.org

## 运行/重启 服务
npm run prd

## 心跳检测
npm run heart-beat-check

echo =================== deploy success ===================

  ```






