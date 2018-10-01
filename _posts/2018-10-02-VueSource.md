---
layout: post
title: 读书笔记—Vue.js权威指南（十一）
tags: [前端技术]
---
#### 遇见Vue.js

* 关键词：Vue.js -- 读书笔记

### 十八、Vue.js 源码

#### util

![image]({{ site.baseurl }}/assets/img/blog/2018-10-02-VueSource/1.png)

1.env

![image]({{ site.baseurl }}/assets/img/blog/2018-10-02-VueSource/2.png)

系统判断  | 说明
--------- | ---------
inBrowser  | 浏览器环境
isIE9  | IE9
isAndroid  | 安卓
isIos  | iOS
isWechat  | 微信

属性支持  | 说明
--------- | ---------
hasProto  | __proto__属性

过渡属性  | 说明
--------- | ---------
transitionProp  |
transitionEndEvent  | 在CSS过渡完成后触发
animationProp  | 在不需要触发任何事件的情况下(区别transition)可以显式地随着时间改变元素的css属性值
animationEndEvent  | 在CSS动画完成后触发

nextTick：

异步执行，在Vue.js内部，Vue.js会使用MutationObserver来实现队列的异步处理，如果不支持会回退到setTimeout(fn,0)。当Vue.nextTick的回调函数执行时，DOM已经是更新后的状态了。

set:

创建set简单对象，挂载属性set,add,clear,has方法。

2.dom

![image]({{ site.baseurl }}/assets/img/blog/2018-10-02-VueSource/3.png)

dom操作  | 说明
--------- | ---------
query(el)  | 查找dom元素，使用document.querySelector方法返回文档中匹配指定CSS选择器的元素集合中的第一个元素
inDoc(node)  | 是否在文档中，运用了ownerDocument(文档).documentElement(根节点)属性
before(el, target)  | 在target节点前插入el元素
after(el, target)  | 在target节点后插入el元素
prepend(el, target)  | 在target节点最前面插入el元素
extractContent(el, asFragment)  | 将元素内容提取到一个div元素或文本碎片中
remove(el)  | 删除el元素
replace(target, el)  | el元素替换target
trimNode(node)  | 清除node节点内首尾空文本或注释节点，使用isTrimmable判断是否为空文本或注释节点
isTemplate(el)  | 是否为template模板元素
isFragment(node)  | 是否为轻量级的Document对象，能够容纳文档的某个部分
getOuterHTML(el)  | 获取元素outerHTML，如果不支持则获取innerHTML，用div元素包裹

属性操作  | 说明
--------- | ---------
getAttr(node, _arrt)  | 在node元素上获取并移除_attr属性
getBindAttr(node, name)  | 获取：name或v-bind:name的属性值
hasBindAttr(node, name)  | node元素上是否有绑定的name属性

class操作  | 说明
--------- | ---------
setClass(el, cls)  | 设置class名称
getBindAttr(node, name)  | 获取：name或v-bind:name的属性值
hasBindAttr(node, name)  | node元素上是否有绑定的name属性

















