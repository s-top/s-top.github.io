---
layout: post
title: 读书笔记—Vue.js权威指南（七）
tags: [前端技术]
---
#### 遇见Vue.js

* 关键词：Vue.js -- 读书笔记

### 十三、路由和视图

Vue.js本身并没有提供路由机制，但是官方以插件(vue-router)的形式提供了对路由的支持。vue-router支持嵌套路由、组件随性载入、视图切换动画、具名路劲等特性。

#### 视图部分

视图部分用来给用户提供导航以及导航结果展示区域。

v-link  | 说明
--------- | ---------
params(对象)  | 包含路由中的动态片段和全匹配片段的键值对
--------- | ---------
query(对象)  | 添加到路劲path后的键值对
--------- | ---------
replace(布尔值)  | 此次导航不会产生历史记录
--------- | ---------
uappend(布尔值)  | 如果此次导航的目的path为相对路径，则实际URL为当前path后拼接目的path
--------- | ---------
activeClass(字符串)  | 默认值为v-link-active，指带有v-link指令的a元素处于激活状态时的class名称
--------- | ---------

![image]({{ site.baseurl }}/assets/img/blog/2018-09-07-RouterViem/1.png)

#### router-view

视图部分用来展示匹配路由的模板内容，在vue-router中使用router-view来渲染匹配的组件。

router-view是一个Vue组件，它具有以下特性：

(1)通过props传递数据

(2)支持v-transition和transition-mode(使用transition指令在路由切换时提供过渡效果)

(3)支持v-ref，被渲染的组件会注册到父级组件的this.$对象中

(4)支持slot,router-view中的HTML内容会被插入到相应路由组件模板的slot中

#### 路由实例

实例化VueRouter时可以传入一个可选的vueRouterConfig路由选项对象来自定义路由器的行为。

(一)路由选项  | 创建路由器实例时，可以传入路由选项来自定义路由器行为
--------- | ---------
hashbang(布尔值)  | 默认值为true。表示匹配的路由在浏览器地址栏中以hash模式显示(当前浏览器地址：.com/path?query。点击home链接时，地址会显示为：.com/path?query#!/home)
--------- | ---------
history(布尔值)  | 默认值为false。当为true时，会以HTML5 history API进行导航 ![image]({{ site.baseurl }}/assets/img/blog/2018-09-07-RouterViem/2.png)
--------- | ---------
saveScrollPosition(布尔值)  | 该值为true时，在点击浏览器后退按钮时页面会定位到上一次该路由对应视图所在的位置
--------- | ---------
transitionOnLoad(布尔值)  | 该值为true时，在页面第一次加载时router-view会有路由切换动画，默认为直接渲染
--------- | ---------
suppressTransitionError(布尔值)  | 该值为true时，在组件路由切换钩子中产生的异常不会被抛出
--------- | ---------
linkActiveClass(字符串)  | 默认值为v-link-active，表示v-link所在元素处于激活状态时，vue-router加在该元素上的类名
--------- | ---------
root(字符串)  | 默认值为null，该值只在history值为true时生效。定义路由根路径，所有路径被匹配时，浏览器地址栏URL会显示为根路径 + 匹配路径
--------- | ---------

(二)路由实例属性  | router实例暴露了两个属性
--------- | ---------
app(根组件实例)  | vue-router应用的根vue实例，由调用router.start(App,'#app')时传入的组件构造器App创建得到
--------- | ---------
mode(字符串)  | 可能值有html5/hash/abstract
--------- | ---------

(三)路由器实例方法  | router实例暴露了很多方法，用来提供启动、路由映射、重定向、路由切换全局钩子等功能
--------- | ---------
start(App,el)  | 启动路由应用，参数(App可以是一个Vue组件构造器或者组件选项对象，el可以是一个CSS选择器或者DOM元素，用来挂载路由应用的根组件)
--------- | ---------
On(path,config)  | 添加顶级路由配置，参数(path要匹配的路径，config路由配置对象)
--------- | ---------
Map(routerMap)  | 批量定义路由映射规则，内部调用router.on方法实现。参数为对象，键为路径，值为路由配置对象。
--------- | ---------
go(path)  | 导航到指定path的路由
--------- | ---------
replace(path)  | 不会创建新的历史记录
--------- | ---------
redirect(redirectMap)  | 定义全局重定向规则。参数格式为\{fromPath: toPath\}，即当前访问的路径到实际路径的映射关系
--------- | ---------
Alias(aliasMap)  | 配置别名规则。和重定向不同，重定向的地址栏显示的是toPath，实际匹配地址也是toPath;而别名实际地址栏显示的是fromPath，而路由实际匹配的是toPath。
--------- | ---------
BeforeEach(hookFunction)  | 该方法用来全局注册前置钩子。它会在整个路由切换的最前端被调用，优先于各路由组件中的路由钩子执行，因此这里可以做一些全局的访问控制。当钩子被reject时，整个路由切换将取消
--------- | ---------






















