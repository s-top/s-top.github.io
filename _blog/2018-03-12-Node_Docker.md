---
layout: post
title: 读书笔记—Node.js进阶之路（二）
tags: [Node]
---
#### 使用Docker部署Node服务以及Node模块

* 关键词：Docker -- Node模块

### 一、Docker基础

Docker是一个开源的容器引擎。开发者可以将自己的应用以及依赖包打包为一个可移植的容器，然后发布到Linux机器上。它类似于一个轻量级的虚拟机。

Docker客户端和守护进程可以运行在同一个系统内，也可以使用Docker客户端去连接一个远程的守护进程，此时两者通过socket或RESTfulAPI进行通信。

默认情况下，Docker守护进程会生成一个socker文件来与本地的客户端通信，而不会监听任何端口。可以编辑文件/etc/default/docker，然后重启服务实现远程通信。

### 二、Node模块

#### 1.VM模块直接运行JavaScript文件

```javascript
    //部分代码
    const vm = require('vm');
    const path = require('path');
    function stripBOM(content){
        ......
    }
    var wrapper = stripBOM(fs.readFileSync(path.resolve(_dirname, '.', 'a.js'), 'utf8'))
    var compiledWrapper = vm.runInThisContext(wrapper,{
        ......
    });
    compiledWrapper();
```

#### 2.模块加载与缓存

#### 3.模块分类

Node.js的模块可以分为三类：

* V8自身支持的对象，例如data、Math等。



* Node作为JavaScript运行时环境，提供了丰富的API，实现这些API的C++函数和JavaScript代码，在Node初始化时被加载（原始模块）。



* 用户的JavaScript文件以及NPM安装的模块。



### 三、V8引擎























