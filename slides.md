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
transition: fade
---

# 如何Promise


<v-clicks>

<div class="slidev-vclick-target slidev-vclick-hidden" style="margin-bottom:3vh;">

**Promise的构造：**

两个参数
>resolve: 表示 Promise 成功的状态

>reject: 表示 Promise 失败的状态
</div>

**Promise是一个对象：**
<div class="slidev-vclick-target slidev-vclick-hidden">
三个状态：

>pending: 初始状态

>fulfilled: 操作成功

>rejected: 操作失败
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

```ts
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
layout: two-cols
transition: fade
---

# 如何Promise(详解)

**Promise的构造函数：**
-

Promise 构造函数接受一个函数作为参数。当 Promise 被构造时，它会被同步执行，所以也被成为起始函数。起始函数有两个参数：
>resolve: 表示 Promise 成功的状态

>reject: 表示 Promise 失败的状态

函数执行成功时，应调用 resolve 函数传递成功的结果

函数执行失败时，应调用 reject 函数传递失败的原因

<v-click>

- 请注意：

1、resolve 和 reject 的**作用域只有起始函数**，then、catch等里面是不能用滴

2、resolve 和 reject 不能使起始函数停止运行，记得 return

</v-click>

::right::
<br/>

```ts{1-15}
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
transition: fade
layout: two-cols
---

# 如何Promise(详解)



**Promise是一个对象：**
-

有三个状态：

>pending: 初始状态

>fulfilled: 操作成功

>rejected: 操作失败


::right::
<br/>

```ts{0}
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
transition: fade
layout: two-cols
---

# 如何Promise(详解)



**Promise是一个对象：**
-

三个方法：

>then：用于处理 Promise 成功状态的回调函数。


>catch：用于处理 Promise 失败状态的回调函数。

>finally：无论成功失败都会执行的回调函数。

这三种方法都会**返回一个Promise**，因此可以使用顺序写法表示回调的嵌套。即使你所写的函数返回值不是一个Promise，它也会被处理为一个Promise。



::right::
<br/>

```ts{15-24}
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
layout: two-cols
---

# 如何Promise(详解)




**Promise是一个对象：**
-

另外：

- 其实在then( successCallback, failureCallback )中，successCallback 是一个函数，参数是resolve()传入的值；failureCallback 也是一个函数，参数是reject()传入的值。所以理论上**then是可以处理rejected状态**的，但我们**一般会把它交给catch处理**。

- finally 与 then 一样会按顺序执行，但是 catch 块只会执行第一个，除非 catch 块里有异常。同时Promise的错误会**随着链向下传递**。没有迫切需要的话我们一般在**最后才安排一个catch进行错误处理**。

- then 块默认会向下顺序执行，**return 不能中断**，但可以 **通过throw 跳转至catch** 实现中断。


::right::
<br/>

```ts{15-24}
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

回到我们刚才的问题：

虽然但是 用了Promise的顺序写法的代码比直接嵌套还丑

怎么办？

接下来有请我们伟大的——

---
layout: section
---

# async & await

---
layout: two-cols
---

# async & await

Promise 的语法糖

更进一步，把：

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

::right::

<br/>
<br/>
<br/>

变成：

```ts
async function print(message,delay) {
  setTimeout( ()=>{
    console.log(message)
  },delay)
}

async function main() {
  await print('First step',1000)
  await print('Second step',2000)
  await print('Third step',3000)
}
```
---
layout: two-cols
---

# async & await

Promise 的语法糖


<div style="width:80%">

>awiat只能在async中使用。

>async可能包含0个或多个 await。

- async:在一个函数的开头添加 async，可以使它成为一个**异步函数**。

<p style="text-decoration:line-through;">顾名思义awiat就是等等的意思（）</p>

- await:在调用一个返回 Promise 的函数前添加 await，可以使得异步函数**在该点暂停**，直到其等待的、基于promise 的异步操作完成再继续运行。此时 Promise 的**响应被当作返回值**，或者**被拒绝的响应被作为错误**抛出。


</div>

::right::

<br/>

```ts
async function print(message,delay) {
  setTimeout( ()=>{
    console.log(message)
  },delay)
}

async function main() {
  await print('First step',1000)
  await print('Second step',2000)
  await print('Third step',3000)
}
```

<v-click>

此时如果我们用一个东西接受await的返回值：

```ts
async function main() {
  const result = await new Promise((resolve) =>
    setTimeout(() => resolve('First step'),1000),
  );
  console.log(result)
}
main();
```

就可以无痛拿到resolve里面的东西

</v-click>

---

虽然但是

aysnc没有像then、catch方法这样

处理异常的机制吗？

<div style="font-size:60px;" v-click >有的！</div>

---
layout: two-cols
---

# async & await 的try-catch
异常处理机制

try里用一个东西来接受solve的值

catch用来处理错误

::right::

<br/>
<br/>

```ts
async function print(num,delay) {
  setTimeout( ()=>{
    if(num>100) resolve('good!')
    else reject('fail')//throw也可以
  },delay)
}

async function main() {
  try {
    let result = await print(20,1000)
  } catch (err) {
    console.log(err);
  }
  console.log(result)
}

main();
```

---
layout: section
---

# TS

**T**ype**S**cript

---

# Ts、Js?
<br/>

Js是弱类型语言，有着1/3~1/2的代码错误是类型错误

Ts是Js的拓展，需要先编译成Js再运行。它的优势在于能够基于类型进行代码检查，减少许多类型错误的出现。

---

# Ts基本类型

变量名：类型 = 什么什么 

最基本的几个类型

```ts
let num:number = 3; //数字
let isPrime:boolean = true; //布尔值
let name:string = '下赫晓'; //字符串

let unusable:void = undefined; //undefined 一个没有设置值的变量
let nil:void = null; //null 一个空对象的引用 typeof检测会返回object
```
元组和数组

```ts

let x:[number,string] = [3,'下赫晓'];//元组
let arr:number[] = [2,3,5];//数组类型
let arr:Arrau<number> = [2,3,5];//数组泛型
```
any类型

```ts
let x:any = 3;
x = true;
x = '下赫晓';//常用于变量值会动态改变
let y:any[] = [3,true,'下赫晓'];//或是存有各种类型数据的数组
```

---

# Ts基本类型

变量名：类型 = 什么什么 

联合类型的声明 用或运算符|连接
```ts
let x:number|boolean|string;
x:any = 3;
x = true;
x = '下赫晓';
```

类型断言 通常用在你确定一个实体有比它现有类型更确切的某个类型

```ts
let x:any = "I'm a string!"
let lx:number = (x as string).length;
let lx:number = (<string>x).length;
```
---

# Ts函数类型、
function 函数名字（参数列表）：返回值类型 { 函数主题}

?表示可选参数

```ts
function main(a:number,b:number,calback:(result:number)=>void,c?:string):void {
  callback(a+b)
}
```

复杂的类型可以起别名

```ts
type ResultCallback = (result:number)=>void;

function main(a:number,b:string,calback:ResultCallback,c?:string):void {
  callback(a+b)
}
```

---

# 类与对象

TypeScript 是面向对象的 JavaScript

类(class)和对象(object)是两种以计算机为载体的计算机语言的合称。

类是一种抽象的数据类型，对象是类的实例，类是对象的模板。

---
layout: two-cols
---

# 类的创建
字段+构造函数+方法

<div style="width:85%;">

定义类的关键字为 class，后面紧跟类名，类可以包含以下几个模块（类的数据成员）：

>字段:类里面声明的变量,表示对象的有关数据。

>构造函数:类实例化时调用，可以为类的对象分配内存。

>方法:对象要执行的操作。

this指向新创建的对象，通过 this 可以在对象内访问自身的属性与方法

**注意！构造函数的参数名应与字段名相同**

</div>

::right::

<br/>
<br/>

```ts
class Tenzor { 
    // 字段 
    name:string 
    age:number
 
    // 构造函数 
    constructor(name:string,age:number) { 
      this.name = name 
      this.age = age

    }  
 
    // 方法 
    grow():void { 
      this.age += 1
    } 
}
```

---

# 实例（对象）的创建与访问
new一下

用new关键字通过类创建对象

用、或【】访问对象属性

```ts
let hgtc = new Tenzor('hgtc',50)

console.log(hgtc.name)
console.log(hgtc[name])

hgtc.grow()
console.log(hgtc.age)
hgtc.['grow']()
console.log(hgtc.age)
```

---

# 类的静态方法

理论上方法是在实例上调用的，不能直接通过类调用的

但是如果我们在它前面加上static关键字把它转变为静态方法，它就可以通过类来直接调用了

```ts
class calculator { 
    add(x:number,y:number):number { 
      return x+y
    } 
    static subtract(x:number,y:number):number { 
      return x-y
    } 
}
calculator.add(1,2)//会报错
calculator.subtract(1,2)//不会报错，返回-1
```

---
layout: section
---

# Thank you