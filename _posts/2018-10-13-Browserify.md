---
layout: post
title: 读书笔记—Vue.js权威指南（十六）
tags: [前端技术]
---
#### 遇见Vue.js

* 关键词：Vue.js -- 读书笔记

### 二十四、Browserify

#### 安装

npm i -g browserify 或者将其作为项目依赖在项目中进行安装 npm i browserify --save-dev

#### 基本使用

同Node.js支持的CommomJS规范一样，Browserify通过require来加载依赖文件。

打包命令：browserify index.js > bundle.js

#### 转换模块

有时候直接require源文件并不能达到我们的目的，我们需要对文件进行预处理后再输出。这时可以利用Browserify提供的转换模块机制，转换模块可以对源文件进行相应的处理后再输出到最终文件中。

> 安装转换模块

转换模块即遵循一定规则的npm模块，所有我们直接通过npm来安装。这里以envify转换模块为例：

npm install -g envify(全局) 和 npm install envify --save-dev(本地)

> 使用转换模块

1.配置方式

在package.json文件的Browserify字段指定转换模块，Browserify编译时会自动依次调用transform数组内指定的转换模块：

"browserify": { "transform": [ "envify" ]}

2.参数方式

还可以在命令行中进行指定:

browserify entry.js -t envify > bundle.js

我们也可以向转换模块传入参数，以下命令想envify转换模块传入NODE_ENV参数，值为development：

browserify index.js -t [ envify --NODE_ENV development ] > bundle.js

#### 相关转换模块介绍

Browserify生态中提供了很多转换模块供我们实现各类需求。

1.vueity，可以让我们将组件的模板、样式、JS逻辑写在同一个文件中。

npm install vueify --save-dev

如果使用npm3及以上版本，需要手动安装babel依赖：

npm install babel-core babel-preset-es2015 babel-runtime babel-plugin-transform-runtime --save-dev

> 使用该转换模块允许我们通过以下形式来写Vue组件，代码示例如下：

```javascript
    //app.vue
    <style>
        .red{
            color: #f00;
        }
    </style>

    <template>
        <h1 class="red">{{mgs}}}</h1>
    </template>

    <script>
        export default {
            data(){
                return {
                    msg: 'hello,world!'
                }
            }
        }
    </script>

    //==========================
    //也可以使用lang来指定预处理器
    //app.vue
    <style lang = "stylus">
        .red
        color #f00
    </style>

    <template lang = "jade">
        h1(class="red") {{mgs}}}
    </template>

    <script lang = "coffee">
        module.exports =
            data: ->
                msg: 'hello,world!'
    </script>

    //==========================
    //也可以使用src引入外部文件
    <style lang = "stylus" src = "style.styl"></style>
    //main.js 入口JS文件
    var Vue = require('vue')
    var App = require('./app.vue')

    new Vue({
        el: 'body',
        components: {
            app: App
        }
    })

    //入口HTML文件
    <body>
        <app></app>
        <script src = "build.js"></script>
    </body>
```

> 编译

browserify -t vueify -e src/main.js -o build/build.js

2.envify，转换模块用来替换代码中的有关node环境变量代码片段。这样我们就可以只在开发环境中输出调试信息，在生成环境中由于条件判断为false，调试信息不会输出。还可以结合uglifyify库在生产环境中将调试信息直接移除。

npm install envify browserify

```javascript
    //index.js
    if(process.env.NODE_ENV === "development"){
        console.log("developmet only");
    }
    //假设当前NODE_ENV环境变量值为production，运行转换模块后将会得到：
    //bundle.js
    if("production" === "development"){
        console.log("developmet only");
    }
```

> 编译

通过Browserify命令行使用envify转换模块：

browserify index.js -t envify > bundle.js

还可以在编译时传入自定义环境变量：

browserify index.js -t [ envify --NODE_ENV development ] > bundle.js

browserify index.js -t [ envify --NODE_ENV production ] > bundle.js

### 二十五、vue-loader

vue-loader是基于webpack的loader，在Vue组件化中起着决定性作用。

#### 如何配置

vue-loader的配置和webpack其他loader的配置类似，对.vue后缀增加处理。

> 配置如下：

```javascript
    module.exports = {
        entry: {
            app: './src/main.js'
        },
        module: {
            loaders: [
                {
                    test: /\.vue$/,
                    loader: 'vue'
                }
            ]
        }
    }
```

#### 包含内容

.vue文件的组成，一般包含：template标签、script标签、style标签

#### 特性介绍

> template标签

支持lang配置多种模板语法

只支持单个template标签

> script标签

默认支持Babel(使用label-loader)来编译ES 6语法糖

对于7.0及以上版本，使用Babel6;如果使用Babel5,则需要使用6.x版本

支持通过import方式载入其他.vue后缀的组件文件

只支持单个script标签

> style标签

支持lang配置多种预编译语法

支持scope属性，这样CSS只应用到当前组件的元素中

一个.vue文件中可以包含多个style标签

内置PostCSS和autoprefixer来自动添加浏览器前缀

> Hot Reload

.vue文件修改后，默认支持对应的页面自动刷新。

默认内置loader的配置如下：

```javascript
    var defaultLoaders = {
        html: 'vue-html-loader',
        css: 'vue-style-loader!css-loader',
        js: 'babel-loader?presets[]=es2015&plugins[]=transform-runtime&comments=false'
    }
    //我们可以看到：
    //JS默认使用babel-loader编译，加了一些参数
    //HTML默认使用vue-html-loader
    //CSS默认使用vue-style-loader和css-loader
```

#### 常见问题解析

1.如何自定义autoprefixer

```javascript
    //webpack.config.js
    module.exports = {
        //...
        vue: {
            autoprefixer: false
        }
    }
```

2.如何自定义PostCSS

```javascript
    //webpack.config.js
    module.exports = {
        //...
        vue: {
            postcss: {
            //...
            }
        }
    }
```

3.如何自定义style的预编译语言

```javascript
    //第一步使用lang配置
    <style lang = "sass"></style>
    //第二步安装依赖
    npm install sass-loader node-sass
```

4.如何自定义loaders

```javascript
    module.exports = {
        module: {
            loaders: [
                {
                    test: /\.vue$/,
                    loader: 'vue'
                }
            ]
        },
        vue: {
            loaders: {}
        }
    }
```

#### vue-loader文件结构

![image]({{ site.baseurl }}/assets/img/blog/2018-10-13-Browserify/1.png)

> 主入口文件loader.js

接受.vue文件的全部内容，然后把内容content、文件名fileName传递下去。

> 内容解析文件parser.js

首先通过调用hash函数把文件名和文件内容生成唯一的key，然后通过cache检测，如果命中cache，就直接返回；如果没有命中，则调用parse5的parseFragment方法对文件内容进行解析，遍历子节点内容(只处理template、style和script节点)，取出对应的特性属性(lang、src和scoped)，针对空节点的script和style也进行了优化。

> parser.js解析过程

![image]({{ site.baseurl }}/assets/img/blog/2018-10-13-Browserify/2.png)

#### 工具包介绍

![image]({{ site.baseurl }}/assets/img/blog/2018-10-13-Browserify/3.png)

![image]({{ site.baseurl }}/assets/img/blog/2018-10-13-Browserify/4.png)

### 二十六、PostCSS

PostCSS是一个用JavaScript插件来转换CSS的工具，插件可以lintCSS，支持变量，mixins、内联的图片等。

PostCSS可以将CSS转换为JavaScript能够处理的数据格式，基于JavaScript所写的插件可以完成上述各种操作。

PostCSS为这些插件提供了接口，方便其完成各自的功能，但是不会对CSS代码做任何修改。

PostCSS的插件可以对CSS进行任何操作，只要我们有需求，就可以写一个JavaScript插件来实现。

#### 安装

PostCSS针对不同的构建工具提供了不同的安装工具。

webpack中：npm install -g postcss-loader

也可以：npm install autoprefixer

#### 配置

PostCSS一般与Gulp、webpack等构建工具搭配使用。在vue-loader中使用PostCSS时，需要在webpack.config.js中进行配置。当需要使用PostCSS的插件时，在vue选项中想postcss设置选项传入一个数组。

```JavaScript
    //比如使用CSSNext插件的配置代码示例如下：
    //webpack.config.js
    module.exports = {
        //other configs...
        vue: {
            //use custom postcss plugins
            postcss: [require('postcss-cssnext')()],
            autoprefixer: false
        }
    }
    //除此之外，postcss设置选项还可以接受：

    //一个可以返回插件数组的函数
    postcss: function(){
        return [precss, autoprefixer];
    }

    //一个对象，该对象包含将被传给PostCSS处理器的设置选项
    postcss: {
        plugins: [...],
        options: {
            parse: sugarss
        }
    }
```

#### 命令

在命令行或者npm scripts中使用PostCSS需要额外安装postcss-cli，其安装方式如下：

nom install postcss-cli

> 语法如下：

postcss [options] [-o output-file|-d output-directory|-r] [input-file]

命令选项如下：

![image]({{ site.baseurl }}/assets/img/blog/2018-10-13-Browserify/5.png)

![image]({{ site.baseurl }}/assets/img/blog/2018-10-13-Browserify/6.png)

![image]({{ site.baseurl }}/assets/img/blog/2018-10-13-Browserify/7.png)

#### 插件

![image]({{ site.baseurl }}/assets/img/blog/2018-10-13-Browserify/8.png)

![image]({{ site.baseurl }}/assets/img/blog/2018-10-13-Browserify/9.png)























