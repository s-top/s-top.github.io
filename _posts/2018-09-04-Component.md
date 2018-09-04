---
layout: post
title: 读书笔记—Vue.js权威指南（五）
tags: [前端技术]
---
#### 遇见Vue.js

* 关键词：Vue.js -- 读书笔记

### 十、开发组件

#### 基础组件

Web组件化的思维是非常重要的。我们习惯把script,view,style分开在独立的目录当中，保证三者不耦合在一起。

随着项目越来越大，这样的结果会让开发越来越痛苦，比如要增加或修改某个view时，就要在script和style里找到对应这个view的逻辑和样式进行修改。

我们开始尝试前端组件化，把view拆分成不同的组件（component），为单个组件编写对应的逻辑和样式:

![image]({{ site.baseurl }}/assets/img/blog/2018-09-04-Component/1.png)

在vue中，可以利用一个.vue文件实现组件化，更加直观。

```javascript
    <template></template>
    <script></script>
    <style></style>
    //还可以在*.vue文件中使用其他预处理器
    <style lang="jade"></style>
    //Webpack就可以自动将.vue文件编译成正常的JavaScript代码，我们只需要在Webpack中配置好vue-loader即可。
```

#### 基于第三方组件开发

Echarts.js 待更新。。。。

#### 常见问题

* camelCase & kebab-case

* 字面量语法 & 动态语法

* 组件选项问题

传入Vue构造器的多数选项也可以用在Vue.extent()中，不过有两个特例：data和el。

* 模板解析

Vue的模板是DOM模板，使用浏览器原生的解析器而不是自己实现一个。

相比字符串模板，DOM模板有一些好处，但是也有问题，它必须是有效的HTML片段。

常见的一些限制：

a不能包含其他的交互元素（如按钮、链接）

ul和ol只能直接包含li

select只能包含option和optgroup

table只能直接包含thead、tbody、tfoot、tr、caption、col、colgroup

tr只能直接包含th和td

* 动态组件与异步组件结合

* 如何解决数据层级结构太深的问题（vm.$set）

* 后端数据交互

```javascript
    new Vue({
        el: '#app',
        data: {
            todos: []
        },
        //如果配合vue-resourse，则：
        created: function () {
            this.$http
                    .get('')
                    .then((data) => {
                this.todos = data;
            });
        }
        //如果配合jQuery的AJAX，则：
        created: function () {
            $.get('')
            .done((data) => {
                 this.todos = data;
            });
        }
    });
```

### 十一、表单校验

在Vue.js应用中，我们可以使用vue-validator插件对表单进行校验。

因为vue-validator是Vue.js的一个插件，所以vue-validator需要使用Vue.use(PluginContructor)注册到Vue对象上，在vue-validator内部会检测window.Vue对象是否存在，如果存在则会自动调用Vue.use()方法；否则需要使用者手动调用Vue.use(VueValidator)来确保校验插件注册到Vue中。在Webpack等支出CommonJS规范的环境中，Vue对象并不会暴露到全局window对象中，而是会通过module.exports形式输出，因此需要使用者手动注册。

```javascript
    var Vue = require('vue');
    var VueValidator = require('vue-validator');
    //安装vue-validator
    Vue.use(VueValidator);

    //简单的小例子
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
    </head>
    <body>
        <div id="app">
            <validator name="myForm">
                <form novalidate>
                    Zip: <input type="text" v-validate:zip="['required']"><br />
                    <div>
                        <span v-if="$myForm.zip.required">Zip code is required.</span>
                        <br/>
                        {{$myForm | json}}
                        //输出如下：
                        { "valid": false, //字段校验是否通过
                          "invalid": true, //取反
                          "touched": false, //校验字段所在元素获得过焦点返回truke,否则返回false
                          "untouched": true, //touched取反
                          "modified": false, //当元素值与初始值是否相同
                          "dirty": false, //字段值改变过至少一次返回true，否则返回false
                          "pristine": true, //dirty取反
                          "zip": {
                             //zip字段的验证结果
                              "required": true,
                              "modified": false,
                              "pristine": true,
                              "dirty": false,
                              "untouched": true,
                              "touched": false,
                              "invalid": true,
                              "valid": false
                          }
                          //表单整体校验单独的属性:
                          errors: //如果整体校验没通过，则返回错误字段信息数组。否则返回undefined。
                        }
                    </div>
                </form>
            </validator>
        </div>
    <script src="https://unpkg.com/vue@1.0.26/dist/vue.min.js"></script>
    <script src="https://cdn.bootcss.com/vue-validator/2.1.3/vue-validator.js"></script>
    <script>
        new Vue({
            el:"#app"
        })
    </script>
    </body>
    </html>
```

#### 内置验证规则

![image]({{ site.baseurl }}/assets/img/blog/2018-09-04-Component/2.png)

#### 重置校验结果

$resetValidation()方法来动态重置校验结果

各校验状态对应的class

![image]({{ site.baseurl }}/assets/img/blog/2018-09-04-Component/3.png)

#### 自定义错误信息模板

validator-errors提供了Component和Partial模板两种定义方法：

```javascript
    //Component模板
    <div class="errors">
        <validator-errors :component="'custom-error'" :validation="$validation1"></validator-errors>
    </div>
    //Partial模板
    <div class="errors">
        <validator-errors partial="'custom-error'" :validation="$validation1"></validator-errors>
    </div>
    Vue.component('custom-error', {
        props: ['field', 'validator', 'message'],
        template: '<p class="error-{{field}}-{{validator}}">{{message}}</p>'
    });
    //动态设置错误信息
    //$setValidatorErrors
    methods: {
        onSubmit: function(){
            var self = this
            var resourse = this.$resource('/user/:id');
            resource.save({},{

            }, function(){

            }).error(function(){
                self.$setValidatorErrors([
                    { }
                ])
            })
        }
    }
```

#### 事件

在校验状态发生变化时，vue-validator会触发相应的事件，我们可以通过vue.js中的事件注册方法来注册相关事件。vue-validator会触发单个字段校验事件和整个表单校验事件。

> 单个字段校验事件

![image]({{ site.baseurl }}/assets/img/blog/2018-09-04-Component/4.png)

#### 后续更新










































