---
layout: post
title: 读书笔记—Vue.js权威指南（八）
tags: [前端技术]
---
#### 遇见Vue.js

* 关键词：Vue.js -- 读书笔记

### 十四、路由匹配

vue-router做路径匹配时支持动态片段、全匹配片段以及查询参数。对于解析过的路由，这些信息都可以通过路由上下文对象访问。在使用了vue-router的应用中，路由对象会被注入每个组件中，赋值为this.$route，并且当路由切换时，路由对象会被更新。

#### 动态片段

动态片段使用以冒号开头的路径片段定义，:username就是动态片段。

动态片段的信息可以从$route.params(键值对)中获得。

> 全匹配片段 *

#### 具名路径

在某些情况下，给一条路径加上一个名字能够让我们更方便地进行路径跳转。

```javascript
    route.map({
        '/user/:userId': {
            name: 'user',
            component: {...}
        }
    })
```

#### 路由对象

路由对象$route  | 说明
--------- | ---------
path  | 字符串，当前路由对象的路径
--------- | ---------
params  | 对象，包含路由中的动态片段和全匹配片段的键值对
--------- | ---------
query  | 对象，包含路由中查询参数的键值对
--------- | ---------
router  | 管理当前路由器的vue-router实例
--------- | ---------
matched  | 数组，包含当前匹配的路径中所包含的所有片段所对应的配置参数对象
--------- | ---------
name  | 字符串，当前路径的名字
--------- | ---------

除了内置属性，在用router.map配置路由映射规则时<定义的字段>(可以在全局钩子函数中进行身份验证)也会复制到最终的路由对象上。

#### transition对象

transition对象在这里不是动画对象，它提供了很多方法来控制路由切换的时机。

transition对象  | 说明
--------- | ---------
to  | 路由对象，将要切换到的路由对象
--------- | ---------
from  | 路由对象，当前路由的路由对象
--------- | ---------
next  | 函数，调用此函数处理切换过程的下一步
--------- | ---------
abort  | 函数，调用此函数来终止或者拒绝此次切换
--------- | ---------
redirect  | 函数，取消当前切换并重定向到另一个路由
--------- | ---------

#### 嵌套路由

嵌套路由和嵌套组件

\<router-view\>是一个顶级的外链，它会渲染一个和顶级路由匹配的组件。

```javascript
    <div id="app">
        <router-view></router-view>
    </div>
    route.map({
        '/foo': {
            component: Foo,
            subRouters:{
                '/':{
                    component: Default
                },
                '/bar':{
                    component: Bar
                }
                ....
            }
        }
    })
    //以上路由映射等价于通过此种方式调用，实际上调用router.mao时在vue-router内部会对每个键值对调用router.on方法来完成路由规则映射
    var Foo = Vue.extend({
        template:
            '<div class="foo">' + .....
            '<router-view></router-view>'  //嵌套的外链
    })
```

#### 动态加载路由组件

当我们使用Webpack或者Browserify时，在基于异步组件编写的Vue项目中也可以较为容易地实现惰性加载组件。

```javascript
    route.map({
        '/async': {
            component: function(resolve){
                //MyComponent通过RequireJS从服务端加载
                resolve(MyComponent)
            }
        }
    })
    //Webpack集成了代码分割功能，我们可以使用AMD风格的require来标识代码分割点
    require(['./MyComponent.vue'], function(MyComponent)){
        //执行MyComponent组件加载完成后的逻辑
    }
    //和路由配合使用
    route.map({
        '/async': {
            component: function(resolve){
                require(['./MyComponent.vue'], resolve){
            }
        }
    })
```

#### Webpack模块化开发

当项目变得非常复杂时，有必要采用模块化方式来开发。

通常我们会用Webpack的代码分割以及vue组件的resolve来实现只有当用户访问某一个路由时，才去服务端获取相应的组件代码。这样做可以加快页面渲染速度，同时可以节省流量。

> 目录结构

![image]({{ site.baseurl }}/assets/img/blog/2018-09-10-RouterMatch/1.png)

![image]({{ site.baseurl }}/assets/img/blog/2018-09-10-RouterMatch/2.png)















