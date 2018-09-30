---
layout: post
title: 读书笔记—Vue.js权威指南（十）
tags: [前端技术]
---
#### 遇见Vue.js

* 关键词：Vue.js -- 读书笔记

### 十六、Scrat + Vue.js

#### 前端工程化

从软件工程的角度来思考前端开发，用工程化的手段来解决前端开发中遇到的各种问题，提升团队的开发效率，这就是所谓的前端工程化。

第一反应是：库/框架选型 + 简单的构建化 + JavaScript/CSS模块化开发。

面临的问题：多人协作开发、组件模块复用、调试部署、版本管理控制、性能优化等。

所以，需要做好以下几件事：

事件  | 说明
--------- | ---------
开发规范  | 制定好开发、部署的目录规范、编码规范
--------- | ---------
模块化  | JavaScript:如AMD/CommonJS/UMD/ES 6 Module等 CSS:如less/sass/stylus等
--------- | ---------
组件化  | 把页面拆分成多个组件，每个组件依赖的CSS,JavaScript，模板，图片等资源放在一起(组件化开发框架：Polymer/React/Vue.js)
--------- | ---------
组件库  | bower/component等
--------- | ---------
性能优化  | 请求合并、资源压缩、CDN、bigpipe和bigrender等
--------- | ---------
项目部署  | 项目部署一般包括静态资源缓存、CDN、增量发布等问题
--------- | ---------
开发流程  | 本地化开发调试，视觉走查确认，前后端联调，测试，上线等环节
--------- | ---------
工程工具  | 构建与优化工具、咖啡啊-调试-部署等流程工具、组件获取和提交工具等
--------- | ---------

> 前端工程化8大要素

![image]({{ site.baseurl }}/assets/img/blog/2018-09-30-FrontEngineering/1.png)

#### Scrat

最大的特点就是：模块化开发和模块生态

![image]({{ site.baseurl }}/assets/img/blog/2018-09-30-FrontEngineering/2.png) | ![image]({{ site.baseurl }}/assets/img/blog/2018-09-30-FrontEngineering/3.png)

模块化资源：具有独立性的模块所对应的静态资源。

非模块化资源：一般为入口页面、模块化框架JavaScript、第三方JavaScript库、页面启动JavaScript等。

![image]({{ site.baseurl }}/assets/img/blog/2018-09-30-FrontEngineering/4.png)

![image]({{ site.baseurl }}/assets/img/blog/2018-09-30-FrontEngineering/5.png)

> scrat release编译后生成的调试目录结构如下：

![image]({{ site.baseurl }}/assets/img/blog/2018-09-30-FrontEngineering/6.png)









