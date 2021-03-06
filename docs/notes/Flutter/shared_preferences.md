---
date: 2022-04-13
title: 基于shared_preferences本地存储操作
tags:
  - Flutter
describe:
---

## 基于shared_preferences本地存储操作

- `shared_preferences` 是什么？
- 如何使用`shared_preferences`？
- `shared_preferences`有那些常用的API？
- 基于`shared_preferences`实现计数器Demo

数据存储是开发APP必不可少的一部分，比如页面缓存，从网络上获取数据的本地持久化等，那么在Flutter中如何进行数据存储呢？

> Flutter官方推荐我们用sharedpreferences进行数据存储，类似于RN中的`AsyncStorage`。

## 什么是shared_preferences?

[shared_preferences](https://pub.dartlang.org/packages/shared_preferences#-installing-tab-)是Flutter社区开发的一个本地数据存取插件：

- 简单的，异步的，持久化的key-value存储系统；
- 在Android上它是基于`SharedPreferences`的；
- 在iOS上它是基于`NSUserDefaults`的；

## 如何使用shared_preferences？

首先在`pubspec.yaml`文件中添加：

```
dependencies:
  shared_preferences: ^0.5.1+
```

记得运行安装哦：`flutter packages get`

在需要用到的文件中导入：

```
import 'package:shared_preferences/shared_preferences.dart';
```

> 存储数据

```
final prefs = await SharedPreferences.getInstance();

// set value
prefs.setInt('counter', counter);
```

> 读取数据

```
final prefs = await SharedPreferences.getInstance();

// Try reading data from the counter key. If it does not exist, return 0.
final counter = prefs.getInt('counter') ?? 0;}
```

> 删除数据

```
final prefs = await SharedPreferences.getInstance();
prefs.remove('counter');
```

## shared_preferences有那些常用的API？

### 存储相关

![shared_preferences](https://oss.filway.cn/filway-blog/shared_preferences_set.png)

> 如上图`shared_preferences`支持int, double, bool, string 与 stringList类型的数据存储；

### 读取相关

![shared_preferences](https://oss.filway.cn/filway-blog/shared_preferences_get.png)

> 上图`shared_preferences`中所提供的读取相关的API；

## 基于shared_preferences实现计数器Demo

![shared_preferences](https://oss.filway.cn/filway-blog/shared_preferences_demo.gif)

```
import 'package:flutter/material.dart';
import 'package:shared_preferences/shared_preferences.dart';

void main() {
  runApp(MaterialApp(
    home: Scaffold(
      appBar: AppBar(
        title: Text('shared_preferences'),
      ),
      body: _CounterWidget(),
    ),
  ));
}

class _CounterWidget extends StatefulWidget {
  @override
  _CounterState createState() => _CounterState();
}

class _CounterState extends State<_CounterWidget> {
  String countString = '';
  String localCount = '';

  @override
  Widget build(BuildContext context) {
    return Center(
      child: Column(
        children: <Widget>[
          RaisedButton(
              onPressed: _incrementCounter, child: Text('Increment Counter')),
          RaisedButton(onPressed: _getCounter, child: Text('Get Counter')),
          Text(
            countString,
            style: TextStyle(fontSize: 20),
          ),
          Text(
            'result：' + localCount,
            style: TextStyle(fontSize: 20),
          ),
        ],
      ),
    );
  }

  _incrementCounter() async {
    SharedPreferences prefs = await SharedPreferences.getInstance();
    setState(() {
      countString = countString + " 1";
    });
    int counter = (prefs.getInt('counter') ?? 0) + 1;
    await prefs.setInt('counter', counter);
  }

  _getCounter() async {
    SharedPreferences prefs = await SharedPreferences.getInstance();
    setState(() {
      localCount = prefs.getInt('counter').toString();
    });
  }
}
```

## 参考

- [Storing key-value data on disk](https://flutter.dev/docs/cookbook/persistence/key-value)