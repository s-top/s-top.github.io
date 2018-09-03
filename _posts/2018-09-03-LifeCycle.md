---
layout: post
title: 读书笔记—Vue.js权威指南（四）
tags: [前端技术]
---
#### 遇见Vue.js

* 关键词：Vue.js -- 读书笔记

### 九、生命周期

在Vue.js中，在实例化Vue之前，它们以HTML的文本形式保存在文本编辑器中。当实例化后将经历创建、编译和销毁三个主要阶段。

![image]({{ site.baseurl }}/assets/img/blog/2018-09-03-LifeCycle/index.png)

![image]({{ site.baseurl }}/assets/img/blog/2018-09-03-LifeCycle/1.png)

可以看到在vue一整个的生命周期中会有很多钩子函数提供给我们在vue生命周期不同的时刻进行操作, 那么先列出所有的钩子函数:

> beforeCreate-created-beforeMount-mounted-beforeUpdate-updated-beforeDestroy-destroyed

#### 生命周期钩子：

<br>

生命周期  | 图解生命周期  | 生命周期说明
--------- | --------- | ---------
beforeCreate之前  |  ![image]({{ site.baseurl }}/assets/img/blog/2018-09-03-LifeCycle/0.png) |
--------- | --------- | ---------
beforeCreate -> created  |  ![image]({{ site.baseurl }}/assets/img/blog/2018-09-03-LifeCycle/3.png) | 在这个生命周期之间，进行初始化事件，进行数据的观测，可以看到在created的时候数据已经和data属性进行绑定（放在data中的属性当值发生改变的同时，视图也会改变）
--------- | --------- | ---------
created -> beforeMount  |  ![image]({{ site.baseurl }}/assets/img/blog/2018-09-03-LifeCycle/4.png) | 首先会判断对象是否有el选项。如果有的话就继续向下编译，如果没有el选项，则停止编译，也就意味着停止了生命周期，直到在该vue实例上调用vm.$mount(el)
--------- | --------- | ---------
beforeMount -> mounted  |  ![image]({{ site.baseurl }}/assets/img/blog/2018-09-03-LifeCycle/5.png) | 可以看到此时是给vue实例对象添加$el成员，并且替换掉挂载的DOM元素。因为在之前console中打印的结果可以看到beforeMount之前el上还是undefined
--------- | --------- | ---------
mounted  |  ![image]({{ site.baseurl }}/assets/img/blog/2018-09-03-LifeCycle/6.png) | 在mounted之前h1中还是通过{{message}}进行占位的，因为此时还有挂载到页面上，还是JavaScript中的虚拟DOM形式存在的。在mounted之后可以看到h1中的内容发生了变化
--------- | --------- | ---------
beforeUpdate -> updated  |  ![image]({{ site.baseurl }}/assets/img/blog/2018-09-03-LifeCycle/7.png) | 当vue发现data中的数据发生了改变，会触发对应组件的重新渲染，先后调用beforeUpdate和updated钩子函数
--------- | --------- | ---------
beforeDestroy -> destroyed |  ![image]({{ site.baseurl }}/assets/img/blog/2018-09-03-LifeCycle/8.png) | beforeDestroy钩子函数在实例销毁之前调用。在这一步，实例仍然完全可用。destroyed钩子函数在Vue 实例销毁后调用。调用后，Vue实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁
--------- | --------- | ---------

<br>

#### 相关总结

> create 和 mounted 相关：

beforecreated : el 和 data 并未初始化
created : 完成了 data 数据的初始化，el没有
beforeMount : 完成了 el 和 data 初始化
mounted : 完成挂载
另外，在标红处，我们能发现el还是 {{message}}，这里就是应用的 Virtual DOM（虚拟Dom）技术，先把坑占住了。到后面mounted挂载的时候再把值渲染进去。

> destroy 相关:

销毁完成后，我们再重新改变message的值，vue不再对此动作进行响应了。但是原先生成的dom元素还存在，可以这么理解，执行了destroy操作，后续就不再受vue控制了。

beforecreate : 比如说：可以在这加个loading事件
created : 在这结束loading，还做一些初始化，实现函数自执行
mounted : 在这发起后端请求，拿回数据，配合路由钩子做一些事情
beforeDestroy : 你确认删除XX吗？
destroyed : 当前组件已被删除，清空相关内容
















