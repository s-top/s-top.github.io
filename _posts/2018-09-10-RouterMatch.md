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

![image]({{ site.baseurl }}/assets/img/blog/2018-09-10-RouterMatch/1.png) | ![image]({{ site.baseurl }}/assets/img/blog/2018-09-10-RouterMatch/2.png)

```javascript
    ========index.html
    <div id="app"></div>
    <script src="build.js"></script>
    ========index.js
    var Vue = require('vue');
    var VueRouter = require('vue-router');
    var configRouter = require('./router-config');
    //router-config用来配置路由映射规则、重定向、别名、全局钩子函数等，并利用Webpack的代码分割功能和vue-router的组件动态resolve的功能提供组件的动态加载
    Vue.use(VueRouter);
    //创建router实例
    var router = new VueRouter({});
    //配置路由映射规则
    configRouter(router);
    //启动主组件，以此来启动整个路由应用
    var app = require("./app.vue");
    router.start(app,"#app");
    ========router-config.js
    module.exports = function configRouter(router){
        //定义路由映射规则
        router.map({
            '/home': {
                component: function(resolve){
                    require(['./home.vue'], resolve);
                }
            },
            '*': {
                component: function(resolve){
                    require(['./not-found.vue'], resolve);
                }
            }
        })
    }
    //路由别名配置
    router.alias({
        '/home/alias': '/home'
    })
    //路由重定向
    router.redirect({
        '/': '/home'
    })
    //全局前置钩子函数，在路由开始切换前执行，优先于路由组件钩子函数
    router.beforeEach(function(transition){
        //transition.to 表示将要跳转到的路由对象
        //transition.from 表示当前路由对象
        if(transition.to.path === "/forbidden"){
            router.app.authenticating = true;
            setTimeout(()=>{
                router.app.authenticating = false;
                //终止路由切换，应用保持
                transition.abort()
            }, 3000)
        }else{
            transition.next()
        }
    })
    ========app.vue定义了根组件，该组件定义了v-link导航部分以及根视图router-view
    <div>
        <a v-link = "{path: '/home'}">Home</a>
        <a v-link = "{path: '/not-found'}">Not-found</a>
    </div>
    //home.vue组件匹配path值为/home
    //在组件的配置对象中，除了常规Vue.js组件的属性外，还可以传入路由配置对象route属性，用来控制路由切换过程中的各个阶段
    //route是一个对象，它可以接受5个钩子函数属性分别用来控制切换时的5个阶段
    ========home.vue
    <style>
        module.exports = {
            //路由配置对象
            route: {
                //判断组件是否可重用
                //只有当前组件和要切换到的组件的根路径相同时，才会调用该钩子函数
                canResuse: function(){
                    return true;
                },
                //从其他路由切换到该组件时，会调用此钩子函数判断
                //可否切换到该组件，当该组件返回true或者被resolve时
                //才会认为可切换到此组件，否则将会取消路由切换，同时
                //停止执行之后的钩子函数
                canActivate: function(){
                    return true;
                },
                //被resolve后视图开始切换的同时被调用
                //注意，当组件可重用时，不会触发该钩子函数
                activate: function(){
                    var self = this;
                    this.timer = setTimeout(function(){
                        self.counter += 1;
                    }, 3000)
                },
                //activate被resolve后，调用该函数获取数据
                data: function(){
                    return {
                        counter: 0
                    };
                },
                //canDeactivate被resolve后调用该函数，可以在
                //函数中进行一些组件移除前的清理工作，如移除定时器等
                deactivate: function(){
                    clearTimeout(this.timer);
                }
            },
            data: function(){
                return{
                    title: "home",
                    counter: 0
                }
            }
        }
    </style>
```

> 注意：

```javascript
    require(['./asyncModule'], function(module){
      module.doSomething()
    })
    //Webpack在编译时遇到以上AMD语法，并不会直接加载asyncModule源码，而是在浏览器中执行到此处时，才会去服务端动态加载，加载完后会执行回调方法，回调接受的参数是请求的module

```

#### 常见问题解析

有时候我们希望在路由视图切换后，切换进来的视图保持在最后一次停留的位置。但是saveScrollPosition不生效（因为saveScrollPosition只在HTML5模式下生效，需要同时设置history值为true）。

监听不到查询参数，从#！/books/search?cat=1到#！/books/search?cat=2。因为此时只有查询参数发生了变化，vue-router认为组件是可重用的，所以只会执行data钩子函数。

判断当前路径和目的路径，有时候我们需要根据当前路径和将要跳转的路径来决定下一步的操作，这时可以在全局钩子函数beforeEach中进行判断

切换路由时修改页面标题（afterEach全局钩子函数）

canReuse配置无效（在可重用阶段，vue-router首先会判断两个路由有无相同的根组件，如果没有则任务不可复用，而不管canReuse的值是否为true）

![image]({{ site.baseurl }}/assets/img/blog/2018-09-10-RouterMatch/3.png)









