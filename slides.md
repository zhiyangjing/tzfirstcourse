---
theme: seriph
background: https://source.unsplash.com/collection/94734566/1920x1080
class: text-center
highlighter: shiki
lineNumbers: false
drawings:
  persist: false
transition: slide-left
title: Welcome to Slidev
mdc: true
---

# 挑战网前端第一次培训

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    启动！ <carbon:arrow-right class="inline"/>
  </span>
</div>


<div class="absolute bottom-10">
  <span class="font-700">
    主讲人：jzy
  </span>
</div>

---
transition: fade-out
layout: quote
---

# 课程安排


- **Javascript 异步**

- **Typescript**

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---
layout: statement
---
# 启动！

---
layout: section
---

# JS 异步编程

---
layout: two-cols
---

# 同步、异步?

异步和同步是相对的


<br/>

**同步**：按照代码顺序执行。每个线程依次执行任务，当一个线程获得了一个事件，需要在这个事件完毕后才能执行下一个事件。**（不是说所有步骤同时运行！）**

**异步**：不按照代码顺序执行。主线程可以发射子线程来完成任务，每个线程在等待某事件的过程中可以继续做自己的事，不需要等这一事件完成后再工作。

::right::

<br/><br/><br/>

<img src="/img1.png" alt="同步和异步" style="zoom:60%;text-align:left;" />

---

# 异步有什么好处？

效率高、避免阻塞

在网页响应的过程中，有的事件可能消耗时间较长，有的甚至可能出现死循环的状况。

主线程作为一个线程，不能同时接受多面的请求。当一个事件没有结束时，界面将无法处理其他请求。如果我们在主线程中进行了这样的事件，有可能使网页的相应时间变得很长，破坏用户体验。

而当我们使用异步编程时，由于**子线程独立**于主线程，即使出现阻塞也不会影响主线程的运行。

因此我们常用异步编程在子线程中处理一些**耗时较长**的事件。

---

# 为什么要回调？

在子线程结束后进行一些操作

主线程发射出子线程之后就和它失去了同步，我们**无法知道子线程进行到了哪里**。

但我们常常要在子线程结束后对它返回的信息进行一些操作，所以我们需要**在启动它的时候就告诉它**：

<p style="font-size:20px">完成后你要干嘛！</p>

<p style="text-decoration:line-through;">然后就可以让它自生自灭了</p>

<br/>

回调函数就是用来进行这样一个操作的：

```ts
setTimeout(()=> console.log('111'),5000)
// 在5s后输出111
```

常见的回调函数还有setTimeout和setInterval等等


---
layout: two-cols
---
# 为什么要回调？

<br/>

这时我们又遇到了新的问题：

<div style="width:50%;">
```ts
setTimeout( ()=> {
  console.log('First step')
  setTimeout( ()=> {
    console.log('Second step')
    setTimeout( ()=> {
      console.log('Third step')
    },3000)
  },2000)
},1000)
```
</div>

::right::

<br/>
<br/>
<br/>

**“回调地狱”**

当我们需要在任务1进行完后进行任务2，在任务2完成后进行任务3，在任务3完成后进行任务4...时，就需要不断的嵌套回调函数。当回调嵌套的层数太多时，代码就变得十分丑陋，不利于维护。

为了更优雅的回调

就需要我们用到伟大的——

---
layout: section
---
# Promise
---
layout: two-cols-header
---

# 为什么Promise

把嵌套的代码变成了顺序的代码 但实际上干的还是嵌套的事情

<p style="text-decoration:line-through;">怎么感觉更丑了</p>

<p style="position:absolute;top:10vh;left:90vw;">
  <!-- <div>我知道你很急但你先</div> -->
  <div style="font-size:80px;" v-click >别急</div>
</p>

::left::

把：

<div style="width:60%;">
```ts
setTimeout( ()=> {
  console.log('First step')
  setTimeout( ()=> {
    console.log('Second step')
    setTimeout( ()=> {
      console.log('Third step')
    },3000)
  },2000)
},1000)
```
</div>

::right::

变成：

<div style="width:90%;">
```ts
new Promise(function (resolve, reject) {
    setTimeout(()=>{
        console.log("First step");
        resolve();
    }, 1000);
}).then(()=>{
    return new Promise(function (resolve, reject) {
        setTimeout(()=>{
            console.log("Second step");
            resolve();
        }, 2000);
    });
}).then(()=>{
    setTimeout(()=>{
        console.log("Third step");
    }, 3000);
});
```
</div>
---
layout: two-cols
---

# 如何Promise


<v-clicks>

<div class="slidev-vclick-target slidev-vclick-hidden" style="margin-bottom:3vh;">

**Promise的构造函数：**

两个参数
>resolve: 表示 Promise 成功的状态

>reject: 表示 Promise 失败的状态
</div>

**Promise是一个对象：**
<div class="slidev-vclick-target slidev-vclick-hidden">
三个状态：

>pending: 初始状态，不是成功或失败状态。

>fulfilled: 意味着操作成功完成。

>rejected: 意味着操作失败。
</div>
<div class="slidev-vclick-target slidev-vclick-hidden">
三个方法：

>then：用于处理 Promise 成功状态的回调函数。

>catch：用于处理 Promise 失败状态的回调函数。

>finally：无论成功失败都会执行的回调函数。
</div>
</v-clicks>

::right::
<br/>

```ts{all|1-12|13-14|16-21|22-24}
const zaoGrandma_IQ = 5
const promise = new Promise((resolve, reject) => {
  //一个异步操作
  setTimeout(() => {
    if (zaoGranma_IQ > 150) {
      resolve('clever!');
    } else {
      reject('stupid');
    }
  }, 5000);
});
// Promise的构造函数
// resolve和reject的名字其实可以随便写
// 位置对了就好
 
promise.then(result => {
  console.log(result);
}).catch(error => {
  console.log(error);
});
// Promise 的方法
// resolve传递的成功结果到会.then里面
// reject传递的失败结果会.catch里面
```

---

