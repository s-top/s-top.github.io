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





