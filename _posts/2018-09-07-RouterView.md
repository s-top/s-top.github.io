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

<br>

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

![image]({{ site.baseurl }}/assets/img/blog/2018-09-07-RouterView/1.png)

#### router-view

视图部分用来展示匹配路由的模板内容，在vue-router中使用router-view来渲染匹配的组件。

router-view是一个Vue组件，它具有以下特性：

(1)通过props传递数据

(2)支持v-transition和transition-mode(使用transition指令在路由切换时提供过渡效果)

(3)支持v-ref，被渲染的组件会注册到父级组件的this.$对象中

(4)支持slot,router-view中的HTML内容会被插入到相应路由组件模板的slot中

#### 路由实例

实例化VueRouter时可以传入一个可选的vueRouterConfig路由选项对象来自定义路由器的行为。

<br>

(一)路由选项  | 创建路由器实例时，可以传入路由选项来自定义路由器行为
--------- | ---------
hashbang(布尔值)  | 默认值为true。表示匹配的路由在浏览器地址栏中以hash模式显示(当前浏览器地址：.com/path?query。点击home链接时，地址会显示为：.com/path?query#!/home)
--------- | ---------
history(布尔值)  | 默认值为false。当为true时，会以HTML5 history API进行导航 ![image]({{ site.baseurl }}/assets/img/blog/2018-09-07-RouterView/6.png)
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

<br>

(二)路由实例属性  | router实例暴露了两个属性
--------- | ---------
app(根组件实例)  | vue-router应用的根vue实例，由调用router.start(App,'#app')时传入的组件构造器App创建得到
--------- | ---------
mode(字符串)  | 可能值有html5/hash/abstract
--------- | ---------

<br>

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
Alias(aliasMap)  | 配置别名规则。和重定向不同，重定向的地址栏显示的是toPath，实际匹配地址也是toPath;而别名实际地址栏显示的是fromPath，而路由实际匹配的是toPath
--------- | ---------
BeforeEach(hookFunction)  | 该方法用来全局注册前置钩子。它会在整个路由切换的最前端被调用，优先于各路由组件中的路由钩子执行，因此这里可以做一些全局的访问控制。当钩子被reject时，整个路由切换将取消。后一个钩子只有在前一个被resolve之后才会调用
--------- | ---------
router.afterEach(hookFunction)  | 该方法用来全局注册后置钩子函数，该钩子会在每次canDeactivate和canActivate猴子被resolve之后执行，并不能保证所有的activate钩子被resolve
--------- | ---------

<br>

#### 组件路由配置

在vue-router应用中，每一个路由对应一个组件，在路由组件中我们可以配置route字段来实现在路由切换的各个阶段对组件进行更好的控制。

<br>

(1)路由切换的各个阶段

vue-router将路由切换分为三个阶段。

在路由切换的各个阶段，vue-router都提供了相应的钩子函数。

canReuse,canActivate,activate,data,canDeavtivate,deactivate。

钩子函数介绍  | 说明
--------- | ---------
canReuse  | 该钩子函数用来判断组件是否可以重用。若可以重用，则不会创建新的组件实例，只会调用data钩子函数来更新数据(transition.to和transition.from)
--------- | ---------
canActivate  | 在验证阶段，当一个组件将要被切入时被调用(transition.abort()可以取消此次切换)
--------- | ---------
activate  | 在激活阶段，当组件被创建而且将要切换进入时被调用(transition.next(),此时再调用transition.abort()不可以取消此次切换)
--------- | ---------
data  | 在激活阶段，activate钩子函数被resolve时调用，用于加载和设置当前组件的数据(transition.next(data)会为组件的data相应属性赋值，原理就是调用component.$set())。切换进来的组件会得到一个名为$loadingRouteData的元属性，初始值为true。
--------- | ---------
canDeavtivate  | 在验证阶段，当一个组件将要被切出时被调用，用来验证该组件是否可以被卸载(transition.next(),此时再调用transition.abort()可以取消此次切换)
--------- | ---------
deactivate  | 在激活阶段，当一个组件将要被禁用和移除时被调用
--------- | ---------

<br>

data和activate钩子函数的不同之处在于：

data在每次路由变动时都会被调用，即使当前组件可以被重用，但是activate仅在组件是新创建时才会被调用。

假设有一个组件对应于路由/message/:id，当前用户所处的路径是/message/1。当用户浏览/message/2使，当前组件可以被重用，所以activate不会被调用。但是我们需要根据新的id参数去获取和更新数据，所以在大部分情况下，在data中获取数据比在activate中更加合理。

activate的作用是控制切换到新组件的时机(数据的获取和新组件的切入动画是并行进行的)。

从用户体验的角度来看一下两者的区别：

如果等到获取数据之后再显示新组件。。。相反，应立刻响应用户的操作，切换视图，展示新组件的“加载”状态(waitForData:true)。

举例：假定当前匹配的路径为a/b/c，当接下来要访问的路径是a/d/e时，我们需要从a/b/c路径对应的组件树切换到a/d/e对于的组件树。

![image]({{ site.baseurl }}/assets/img/blog/2018-09-07-RouterView/3.png)

> 在以上路由切换过程中，我们需要做以下工作：

可以重用组件A，因为重新渲染后，组件A依然保持不变。

需要停用并移除组件B和C。

启用并激活组件D和E。

在执行2和3之前，需要确保切换效果有效，也就是确保切换中涉及的所有组件都能按照所期望的那样被停用/激活。

<br>

步骤阶段  | 图解不同阶段  | 说明
--------- | ---------
可重用阶段  |   | 检测可重用性（通过canReuse选项）来判断的。默认情况下，所有组件都是可重用的
--------- | ---------
验证阶段  | ![image]({{ site.baseurl }}/assets/img/blog/2018-09-07-RouterView/4.png)  | 检测当前组件是否能够停用，以及新组件是否可以被激活（通过canDeactivate和canActivate钩子函数来判断）
--------- | ---------
激活阶段  | ![image]({{ site.baseurl }}/assets/img/blog/2018-09-07-RouterView/5.png)  | 一旦所有的验证钩子函数都被调用而且没有终止切换，切换就可以认定是合法的，路由器则开始禁用当前组件并启用新组件
--------- | ---------

<br>



















