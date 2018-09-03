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
--------- | ---------
beforeCreate之前  |  ![image]({{ site.baseurl }}/assets/img/blog/2018-09-03-LifeCycle/0.png) |
--------- | ---------
beforeCreate -> created  |  ![image]({{ site.baseurl }}/assets/img/blog/2018-09-03-LifeCycle/3.png) | 在这个生命周期之间，进行初始化事件，进行数据的观测，可以看到在created的时候数据已经和data属性进行绑定（放在data中的属性当值发生改变的同时，视图也会改变）
--------- | --------- | ---------
created -> beforeMount  | ![image]({{ site.baseurl }}/assets/img/blog/2018-09-03-LifeCycle/4.png) | 首先会判断对象是否有el选项。如果有的话就继续向下编译，如果没有el选项，则停止编译，也就意味着停止了生命周期，直到在该vue实例上调用vm.$mount(el)
--------- | ---------
beforeMount -> mounted |  | 可以看到此时是给vue实例对象添加$el成员，并且替换掉挂在的DOM元素。因为在之前console中打印的结果可以看到beforeMount之前el上还是undefined
--------- | ---------
mounted  |
--------- | ---------
beforeUpdate  |
--------- | ---------
updated  |
--------- | ---------
beforeDestroy  |
--------- | ---------
destroyed  |
--------- | ---------

<br>





















