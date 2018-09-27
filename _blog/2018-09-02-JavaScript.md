---
layout: post
title: 读书笔记—JavaScript总结
tags: [前端技术]
---
#### 遇见JavaScript

* 关键词：遇见JavaScript -- 读书笔记

### 钩子函数和回调函数的区别

钩子的概念源于Windows的消息处理机制，通过设置钩子，应用程序可以对所有的消息事件进行拦截，然后执行钩子函数，对消息进行想要的处理方式。

```JavaScript
    //钩子函数
    //钩子是在捕获消息的时候立即执行钩子函数
    let btn = document.getElementById("btn");
    btn.onclick = () => {
        console.log("i'm a hook");
    }
    //回调函数
    //后面那个函数是它的回调函数，因为消息捕获的过程我们并不能参与，而在捕获执行完毕的时候，回调函数才会执行，我们可以把对消息的处理写在回调函数里
    btn.addEventListener("click",() =>{
        console.log(this.onclick);//undefined
    });
```

根本上，他们都是为了捕获消息而生的，但是钩子函数在捕获消息的第一时间就会执行，而回调函数是在整个捕获过程结束时，最后一个被执行的。

回调函数其实就是调用者把回调函数的函数指针传递给调用函数，当调用函数执行完毕时，通过函数指针来调用回调函数。

### 严格模式

"use strict";

消除代码运行的一些不安全之处，保证代码运行的安全；

提高编译器效率，增加运行速度；

为未来新版本的Javascript做好铺垫。

"严格模式"体现了Javascript更合理、更安全、更严谨的发展方向，包括IE 10在内的主流浏览器，都已经支持它，许多大项目已经开始全面拥抱它。

另一方面，同样的代码，在"严格模式"中，可能会有不一样的运行结果；一些在"正常模式"下可以运行的语句，在"严格模式"下将不能运行。掌握这些内容，有助于更细致深入地理解Javascript，让你变成一个更好的程序员。

#### 严格模式的限制

不允许使用未声明的变量

不允许删除变量或对象

不允许删除函数

不允许变量重名

不允许使用八进制

不允许使用转义字符

不允许对只读属性赋值

不允许对一个使用getter方法读取的属性进行赋值

不允许删除一个不允许删除的属性

变量名不能使用 "eval" 字符串

变量名不能使用 "arguments" 字符串

不允许使用以下这种语句：with (Math){x = cos(2)};

由于一些安全原因，在作用域 eval() 创建的变量不能被调用

禁止this关键字指向全局对象

#### 保留关键字

为了向将来Javascript的新版本过渡，严格模式新增了一些保留关键字：

implements-interface-let-package-private-protected-public-static-yield

"use strict" 指令只允许出现在脚本或函数的开头

### 函数去抖(debounce)和函数节流(throttle)

函数节流是指一定时间内js方法只跑一次。比如人的眨眼睛，就是一定时间内眨一次。这是函数节流最形象的解释。

函数防抖是指频繁触发的情况下，只有足够的空闲时间，才执行代码一次。比如生活中的坐公交，就是一定时间内，如果有人陆续刷卡上车，司机就不会开车。只有别人没刷卡了，司机才开车。

```javascript
    // 函数节流
    // 函数节流的要点是，声明一个变量当标志位，记录当前代码是否在执行
    // 如果空闲，则可以正常触发方法执行
    // 如果代码正在执行，则取消这次方法执行，直接return
    var canRun = true;
    document.getElementById("throttle").onscroll = function(){
        if(!canRun){
            // 判断是否已空闲，如果在执行中，则直接return
            return;
        }
        canRun = false;
        setTimeout(function(){
            console.log("函数节流");
            canRun = true;
        }, 300);
    };
    //================================================
    // 函数防抖
    // 函数防抖的要点，也是需要一个setTimeout来辅助实现。延迟执行需要跑的代码。
    // 如果方法多次触发，则把上次记录的延迟执行代码用clearTimeout清掉，重新开始。
    // 如果计时完毕，没有方法进来访问触发，则执行代码。
    var timer = false;
    document.getElementById("debounce").onscroll = function(){
        clearTimeout(timer); // 清除未执行的代码，重置回初始化状态
        timer = setTimeout(function(){
            console.log("函数防抖");
        }, 300);
    };
```

> 区别如下：

![image]({{ site.baseurl }}/assets/img/blog/2018-09-02-JavaScript/1.png)

![image]({{ site.baseurl }}/assets/img/blog/2018-09-02-JavaScript/2.png)























