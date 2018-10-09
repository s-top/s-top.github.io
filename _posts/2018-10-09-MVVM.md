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
addClass(el, cls)  | 添加className，其中classList属性返回元素的类名，作为DOMTokenList对象，该属性用于在元素中添加、移除及切换CSS类
removeClass(el, cls)  | 删除el元素的className，如果没有className，则移除元素的class属性

事件操作  | 说明
--------- | ---------
on(el, event, cb, useCapture)  | el绑定event事件监听（useCapture指定事件是否在捕获或冒泡阶段执行）
off(el, event, cb)  | 移除事件监听

其他  | 说明
--------- | ---------
createAnchor(content, persist)  | 创建一个“执行插入/删除锚点”。使用场景：片段代码、v-html、v-if、v-for、组件  | ![image]({{ site.baseurl }}/assets/img/blog/2018-10-02-VueSource/4.png)
findRef(node)  | 在node元素中找到v-ref绑定的值  |
mapNodeRange(node, end, op)  | 在范围兄弟节点内调用op函数  |
removeNodeRange(start, end, vm, frag, cb)  | 依次移除范围内的兄弟元素并移入frag元素中  |

3.lang

![image]({{ site.baseurl }}/assets/img/blog/2018-10-02-VueSource/5.png)

对象操作  | 说明
--------- | ---------
set(obj, key, val)  | 设置对象属性，添加新属性触发更新
del(obj, key)  | 删除对象属性，触发更新
hasOwn(obj, key)  | 判断是否为自有属性
extend(to, from)  | 扩展对象属性
isObject(obj)  | 是否为对象
isPlainObject(obj)  | 是否为对象字面量
def(obj, key, val)  | 将属性添加到对象，或修改现有属性的特性

名称转换  | 说明
--------- | ---------
classify(str)  | 将-、_、//的命名转换成驼峰命名方式
hyphenate(str)  | 将驼峰命名方式转换为-
camelize(str)  | 将-命名方式转换为驼峰式

数组操作  | 说明
--------- | ---------
indexOf(arr, obj)  | 返回obj在Array中的索引位置，没有则返回-1

类型转换  | 说明
--------- | ---------
_toString(value)  | 字符串转换
toNumber(value)  | 数字转换
toBoolean(value)  | 布尔转换
toArray(value)  | 数组转换

方法绑定  | 说明
--------- | ---------
bind(fn, ctx)  | ![image]({{ site.baseurl }}/assets/img/blog/2018-10-02-VueSource/6.png)
--------- | ---------
其他  | 说明
--------- | ---------
debounce(func, wait)  | 此方法只用于input输入后，wait毫秒后调用func方法
stripQuotes(str)  | 去除str中的前后引号
cancellable(fn)  | 可以撤销的异步回调函数
looseEqual(a, b)  | 判断a和b是否相等（非严格意义上的===）
isLiteral(exp)  | 检查表达式是否为字面量
isReserved(str)  | 检查str是否以$或_开头

4.components

![image]({{ site.baseurl }}/assets/img/blog/2018-10-02-VueSource/7.png)

属性  | 说明
--------- | ---------
commonTagRE  | 是否为普通元素
reservedTagRE  | 是否为自定义元素
checkComponentAttr(el, options)  | 检查el是否为组件，如果是则返回组件id

5.options

> mergeOptions，此方法的核心是strats对象。

![image]({{ site.baseurl }}/assets/img/blog/2018-10-02-VueSource/8.png)

这部分待细分。。。
















