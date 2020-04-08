---
title: 异步编程解决方案——async/await
date: 2019-03-29 21:53:47
tags: [javascript,promise,async,await] 
---
# 异步编程解决方案——async/await
## 一、 异步
### 1. 单线程的JavaScript
`JavaScript` 的一大特点就是单线程。  
Brendan Eich用了十天时间创造 `JavaScript`，使得浏览器可以与网页互动。由于当时页面十分简单，为了避免过于复杂，`JavaScript` 就被设计为单线程。   
 
单线程意味着任务只能一个个执行，前一个执行完毕才执行下一个，例：  
```JavaScript
console.log(1)
while(true){} // 阻塞
console.log(2) // 永远不执行
```
但假如操作所需时间太长（比如请求一个很大的数据），就不得不等待请求结果，然后才进行下一步。  
考虑到这个问题，JS作者意识到可以使用其他线程来做这些操作，比如：处理AJAX请求的线程、处理DOM事件的线程、定时器线程、读写文件的线程（Node.js中），而**JS引擎中负责解释和执行JavaScript代码的线程只有一个，即所谓的单线程**。  
<!-- more -->

### 2. 同步与异步
由于有其他线程协助工作，所有的任务就可以分为两种，一种是**同步任务**，一种是**异步任务**。  
  
1. 同步任务就是在主线程上排队执行的任务，只有当前一个任务执行完毕，才会执行下一个任务。  
  
2. 异步任务不进入主线程，而是进入任务队列（job queue），只有当任务队列通知主线程，可以进行某个异步任务了（比如ajax请求完成，定时器已经到了设定的时间，或者触发了click事件等），该异步任务才会进入主线程执行。  

### 3. 事件循环
简而言之，主线程只会做一件事，就是执行事件、从任务队列里取任务、再执行事件、再取任务。当任务队列为空时，就会等待任务队列变为非空。而且主线程只有将当前的任务执行完成后，才会去取下一个任务。这种机制就叫做 **事件循环（event loop）** 机制，取一次任务并执行的过程叫做一次循环。  
  
事件循环示意图如下：  
![eventLoop](/blog/images/eventLoop.png) 
上图中，主线程运行的时候，产生堆（heap）和栈（stack），栈中的代码会调用各种API（DOM操作、ajax请求、定时器等），这些API会在任务队列中加入各种事件（onClick、onLoad、onDone等）。当主线程执行完栈中的代码，就会去读取任务队列，然后依次执行那些事件对应的**回调函数**。  
![eventLoop](/blog/images/eventLoop.gif) 

```JavaScript
console.log('Hi')

setTimeout(function cb() {
    console.log('there')
},0)

console.log('SJS')
```
## 二、回调（callback）
因为异步需要等待结果，而返回结果后需要有所反应，因此就需要有回调函数。  
### 1. 什么是回调？
被作为参数传入另一函数，并在该外部函数内被调用，用以来完成某些任务的函数，称为回调函数。  
回调函数可分为同步回调和异步回调，在这里只讨论异步的函数回调。  
上面的示例中，函数 `cb` 就是一个回调函数，在异步任务定时器到期时，函数 `cb` 就会进入任务队列，等待主线程获取任务。  

### 2. 回调地狱
假设我们用 getUser 来获取用户数据，它接收两个回调 sucessCallback 和 errorCallback，例：
```JavaScript
function getUser(successCallback, errorCallback){
  $.ajax({
    url:'/user',
    success: function(response){
      successCallback(response)
    },
    error: function(xhr){
      errorCallback(xhr)  
    }
  })
}
```
看起来不算太复杂。  
但如果获取完用户数据，还需要获取分组数据等，就会变成这样：
```JavaScript
getUser(function(response){
    getGroup(response.id, function(group){
      getDetails(groupd.id, function(details){
        console.log(details)
      },function(){
        alert('获取分组详情失败')
      })
    }, function(){
      alert('获取分组失败')
    })
  }, function(){
  alert('获取用户信息失败')
})
```
嵌套越多，代码越多，这就是**回调地狱（callback hell）**。  
一个例子：  
![callbackHell](/blog/images/callbackHell.jpg)  

回调地狱不仅可读性差，出现问题也不好调试，也会导致性能下降，那么应如何避免？  

## 三、 `Promise`
`Promise` 为异步编程而生，有了 `Promise` 对象就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。
`Promise` 编程的核心思想是如果 **数据就绪(promised)**，**那么(then)** 做点什么。  
  
### 1. `Promise` 是什么
`Promise` 的中文意思是承诺，即，`JavaScript` 对你许下一个承诺，会在未来某个时刻兑现。  
`Promise` 对象是一个构造函数，接受一个函数作为参数，这个函数的两个参数分别是 `resolve` 和 `reject`。  
  
例：  
```JavaScript
var promise = new Promise(function(resolve, reject){
  // ... 一些代码

  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
```
### 2. `Promise` 的生命周期
`Promise` 有三种状态：**进行中（pending）**、**完成态（fulfilled）** 和 **拒绝态（rejected）**。  
一开始，先设置好等状态从 `pending` 变成 `fulfilled` 和 `rejected` 的预案（当成功后做什么，失败时做什么）。  
  
**`resolve` 函数的作用是**，当满足成功的条件时，让状态从 `pending` 变成 `fulfilled` （执行 `resolve`）。  
  
**`reject` 函数的作用是**，当满足失败的条件时，让状态从 `pending` 变成 `rejected`（执行 `reject`）。  

### 3. `then`
`Promise` 实例都有 `then()` 方法，接受两个回调函数作为参数：第一个回调函数是当 `Promise` 状态变为 `fulfilled` 时调用，第二个回调函数是当 `Promise` 状态变为变为 `rejected` 时调用。  
举个例子：  
```JavaScript
function timeout(ms) {
  return new Promise(function(resolve, reject){
    setTimeout(resolve, ms, 'done');
  });
}

timeout(100).then(function(value){
  console.log(value);
});

// 'done'
```
timeout 函数返回一个 `Promise` 实例，传入指定的时间（参数ms），当过了指定的时间后，`Promise` 的状态变成了 `fulfilled` ，就会触发 `then` 绑定的回调函数。  
 
`then()` 方法是定义在原型对象 `Promise.prototype` 上的，会返回一个新的 `Promise` 实例，因此可以使用**链式调用**。  
例：  
```JavaScript
function 获取用户信息(){
  return new Promise(function(resolve,reject){
    console.log('第一次获取用户信息中……')
    resolve('姓名小黑')
  })
}

function 打印用户信息(用户信息){
  return new Promise(function(resolve,reject){
    console.log(用户信息)
    resolve()
  })
}

function 第二次获取用户信息(){
  return new Promise(function(resolve,reject){
    console.log('第二次获取用户信息中……')
    resolve('姓名小白')
  })
}

获取用户信息()
  .then(打印用户信息)
  .then(第二次获取用户信息)
  .then(打印用户信息)
```

### 4. Promise.prototype. ( finally/catch )
```JavaScript
// promise.finally
获取用户信息()
  .finally(fn)
// 等价于
获取用户信息()
  .then(fn,fn)
// 不管成功失败，都调用一个函数
```

```JavaScript
// promise.catch
获取用户信息()
  .catch(fn)
// 等价于
获取用户信息()
  .then(null,fn)

// 只在获取用户信息失败时调用函数
```
### 5. `Promise. (all/race)`
```JavaScript
// 只有当所有Promise实例的状态都变为fulfilled，才会被resolve。
// 只要有一个实例状态变为rejected，就会被reject，第一个被reject的实例的返回值会传递给回调函数。
Promise.all([获取用户信息1(),获取用户信息2(),获取用户信息3()])
  .then(function(results){
    console.log(results)
  },function(err){
    console.log(err)
  })

// ['小黑','小白','小灰']
```
```JavaScript
// race(赛跑)，只要有一个Promise实例的状态 率先 发生改变，就将那个Promise实例的返回值传递给回调函数。
Promise.race([获取用户信息('小白'),定时器(5000)])
  .then(function(result){
    console.log(result)
  },function(err){
    console.log(err)
  })

//'获取信息超时'
```
参数可以不是数组，但必须具有 `Iterator` 接口，并且每个成员返回的都是 `Promise` 实例。  
  
### 6. Promise与回调地狱
```JavaScript
getUser(function(response){
    getGroup(response.id, function(group){
      getDetails(groupd.id, function(details){
        console.log(details)
      },function(){
        alert('获取分组详情失败')
      })
    }, function(){
      alert('获取分组失败')
    })
  }, function(){
  alert('获取用户信息失败')
})

getUser()
  .then(getGroup)
  .then(getDetails)
  .then()
  ...
```
`Promise` 处理回调相比普通回调函数的写法简洁直观得多，并且也有 `Promise.all` 等可以监听所有异步操作的方法，但链式 `then()` 不过是回调地狱的“优雅”的写法。那么有没有更好的处理异步的方法？

## 四、 `Iterator`（迭代器/遍历器）
### 1. 为什么需要 `Iterator`
`JavaScript` 原有用来表示“集合”的数据结构有：数组( `Array` )和对象( `Object` )，在 `ES6` 中又新增了 `Map` 和 `Set`，并且还可以组合使用，比如数组的成员是 `Map` ， `Map` 的成员是对象，所以就需要有一种统一的接口机制，来处理所有不同的数据结构。  
  
`Iterator` 就是一种为各种不同的数据结构提供统一的访问接口的机制。任何数据只要部署了 `Iterator` 接口，就可以完成迭代/遍历操作。
### 2. `Iterator` 的实现
`Iterator` 是一个对象，它提供了一个 `next()` 方法，每次调用 `next()` ，都会返回一个结果对象，结果对象有两个属性： `value` 和 `done` 。其中， `value` 属性表示下一个将要返回的值， `done` 属性是一个布尔值，表示迭代/遍历是否结束，当没有更多可以返回的数据时， `done` 的值就是 `true`。  
举个例子：
```JavaScript
function makeIterator(array){
  var nextIndex = 0
  return {
    next: function(){
      return nextIndex < array.length ?
               {value: array[nextIndex++], done: false} :
               {value: undefined, done: true};
    }
  }
}

var iter = makeIterator(['yo','yeah'])
iter.next() // {value: 'yo', done: false}
iter.next() // {value: 'yeah', done: false}
iter.next() // {value: undefined, done: true}
```
`makeIterator` 函数是一个遍历器生成函数，它的作用就是返回一个遍历器对象。上例就是返回了数组 `['yo','yeah']` 的遍历器对象 (指针对象) `iter`。  
  
指针对象的 `next()` 用来移动指针，第一次调用时，指针指向 `yo`，第二次调用时，指向 `yeah`。  

`next()` 返回一个对象，表示当前数据成员的信息。这个对象拥有两个属性， `value` 和 `done` ： `value` 属性的值是当前成员， `done` 属性是一个布尔值，表示遍历是否结束，即是否还有必要再一次调用next方法。  

### 3. 默认的 Iterator 接口
一种数据结构只要部署了 `Iterator` ，就称这种数据结构为“可遍历的”（ `iterable` ）。  

`ES6` 规定，默认的 `Iterator` 接口部署在数据结构的 `Symbol.iterator` 属性，换句话说，一个数据结构只要具有 `Symbol.iterator` 属性，就可以说是 `iterable`。  
`String`， `Array`， `TypedArray（类数组，如DOM NodeList、arguments等）`，  `Map` 和 `Set` 是所有内置可迭代对象， 因为它们都具有 `Symbol.iterator` 属性。

举个例子：
```JavaScript
var a = 'yeah'
var b = a[Symbol.iterator](/blog/images/)

b.next() // {value: 'y', done: false}
b.next() // {value: 'e', done: false}
b.next() // {value: 'a', done: false}
b.next() // {value: 'h', done: false}
b.next() // {value: undefined, done: true}
```

### 4. for...of 循环
一个数据结构只要部署了 `Symbol.iterator` 属性，就被视为具有 `iterator` 接口，就可以用 `for...of` 循环遍历它的成员。也就是说， `for...of` 循环内部调用的是数据结构的 `Symbol.iterator` 方法。  
  

## 五、 `Generator`（生成器）
生成器是一个返回遍历器的函数，它的特点是可以交出函数的执行权。
### 1. 语法
```JavaScript
function* gen(x){
  let y = yield x * 2
  return y
}

var g = gen(1)
g.next() // {value: 3, done: false}
g.next() // {value: undefined, done: true}
```
生成器通过在 `function` 关键字的星号 `*` 来表示，函数中会用到 `yield`（退让、退位） 关键字。  
在上例中，调用 `gen(1)` ，会返回一个内部指针（即遍历器） `g`。即，执行 `Geneartor` 函数不会返回结果，而是返回指针对象。  
调用 `g` 的 `next` 方法，会移动内部指针。第一次调用 `next` 时，指向第一个遇到的 `yield` 语句，在上例中，即是执行 `x + 2` 为止。  
  
遍历器对象的 `next` 方法的运行逻辑如下：  
1. 遇到 `yield` 表达式，就暂停执行后面的操作，并将紧跟在 `yield` 后面的那个表达式的值，作为返回的对象的value属性值。  
2. 下一次调用 `next` 方法时，再继续往下执行，直到遇到下一个 `yield` 表达式。  
3. 如果没有再遇到新的 `yield` 表达式，就一直运行到函数结束，直到 `return` 语句为止，并将 `return` 语句后面的表达式的值，作为返回的对象的 `value` 属性值。  
4. 如果该函数没有 `return` 语句，则返回的对象的 `value` 属性值为 `undefined` 。  
5. `next` 方法还可以接受参数，向生成器函数体内输入数据。  
  
```JavaScript
function* gen(x){
  var y = yield x + 2
  return y
}

var g = gen(1)
g.next() // { value: 3, done: false }
g.next(2) // { value: 2, done: true }
```
### 2. 异步处理
`yield` 表达式可以暂停函数执行， `next` 方法可以恢复函数执行，因此生成器函数就适合将异步任务同步化。
```JavaScript
function getUser(name){
  return new Promise(function(resolve,reject){
    fetch(`https://api.github.com/users/${name}`)
      .then(function(data){
        resolve(data.json())
      },function(error){
        reject(error)
      })
  })
}

function* getUserByGenerator() {
    var user1 = yield getUser('qinggge')
    var user2 = yield getUser('haha')
    var user3 = yield getUser('hehe')
}

var g = getUserByGenerator()
var result = g.next().value
result.then(function(v){
  console.log(v)
  }, function(error){
  console.log(error)
})
```
上例中，当第一次调用 `next` 方法时，启动了遍历器对象，此时返回一个包含 `value` 和 `done` 的对象。因为 `getUser()` 方法返回了一个 `Promise` 实例，所以 `value` 的值就是一个 promise 对象，因此可以使用 `then` 方法获取到通过 `resolve` 传递过来的值。  
但是，生成器函数每次都需要通过 `g.next()` 的方法来执行，所以需要一个自动任务执行器来自动 `g.next()`，如下：  
```JavaScript
// fn是一个生成器函数，run是异步任务执行器
function run(fn){
  var gen = fn() // 调用生成器
  var result = gen.next() // 执行生成器的第一个next()，返回result
  function step() {
    if(!result.done) {
    // 如果done为false，则继续执行next()，并且循环step，直到done为true退出。
      result = gen.next(result.value);
      step();
    }
  }
  step(); // 开始执行step()
}

// 测试
run(function *() {
  var value = yield 1
  value = yield value + 20
  value = yield value + 30
  value = yield value + 40
	console.log(value)
})

// 91
```
自动执行器的出现使得 `Generator` 处理异步的操作，使得世界一下子清爽了好多，因此这种操作方法也被写入了规范，即 `async` / `await` 。

## 六、 `async` / `await` 
`ES2017` （即 `ES8` ）标准引入了 `async` 函数，使得异步操作变得更加方便。
### 1. async/await 是什么？
`async` 函数是生成器函数的语法糖。
```JavaScript
function getUser(name){
  return new Promise(function(resolve,reject){
    fetch(`https://api.github.com/users/${name}`)
      .then(function(data){
        resolve(data.json())
      },function(error){
        reject(error)
      })
  })
}

var getUserByGenerator = function* () {
    var user1 = yield getUser('qinggge')
    var user2 = yield getUser('haha')
    var user3 = yield getUser('hehe')
}
```
改写为 `async` 函数，变成这样：  
```JavaScript
var asycnGetUser = asycn function () {
    var user1 = await getUser('qinggge')
    var user2 = await getUser('haha')
    var user3 = await getUser('hehe')
}
```
`async` 函数就是将 `Generator` 函数的星号（ `*` ）替换成 `async` ，将 `yield` 关键字替换为 `await` 而已。  
`asycn` 函数的优点在于：  
1. 内置执行器  
  `Generator` 函数的执行必须要依靠执行器，而 `async` 函数自带执行器。也就是说，`async` 函数的执行，与普通函数一模一样，只要一行。  
    ```JavaScript
      asycnGetUser()
    ```
    调用 `asycnGetUser` 函数，就会自动执行，而不需要像 `Generator` 函数需要 `next`。  

2. 更好的语义
  `async` 可表示函数内有异步操作， `await` 则表示后面的表达式需要等待结果。  

3. 更广的适用性
  `async` 函数的 `await` 命令后面，可以是 `Promise` 对象和原始类型的值（数值、字符串和布尔值，但这时等同于同步操作）。  

4. 返回值是 `Promise`
  这意味着可以直接使用 `asycnGetUser().then()` 的方式来进行下一个操作，而不是通过 `next()` 方法。  

### 2. 用法
`async` 函数返回一个 `Promise` 对象，可以使用 `then` 方法添加回调函数。当函数执行的时候，一旦遇到 `await` 就会先返回，等到异步操作完成，再接着执行函数体内后面的语句。  
举个例子：
```JavaScript
async function getUserId(name) {
  let photos = await getUserPhotos(name)
  let friends = await getUserFriends(photos)
  return friends
}

getUserId('小白').then(function (result) {
  console.log(result)
})
```

### 3. 语法
1. 返回 `Promise` 对象  
  `async` 内部 `return` 语句返回的值，会成为 `then` 回调函数的参数。
    ```JavaScript
    async function fn(){
      return 'Hello world!'
    }

    fn().then((data)=>{console.log(data)})
    // 'Hello world!'
    ```
    `async` 函数内部抛出错误，会导致返回的 `Promise` 对象变为 `rejected` 状态，抛出的错误对象会被 `catch` 回调函数接收到。
    ```JavaScript
    async function fn() {
      throw new Error('出错了');
    }

    fn().then(
      v => console.log(v),
      e => console.log(e)
    )
    // Error: 出错了
    ```
2. `Promise` 的状态变化
  `async` 函数返回的 `Promise` 对象，必须等到内部所有 `await` 命令后面的 `Promise` 对象执行完，才会发生状态改变，除非遇到 `return` 或抛出错误。也就是说，只有 `async` 函数内部的异步操作执行完，才会执行 `then` 方法指定的回调函数。
    ```JavaScript
    async function getUserId(name) {
      let friends = await getUserFriends(name)
      let hobbys = await getUserHobbys(name)
      return `${name}的朋友是${friends}，爱好是${hobbys}`
    }

    getUserId('小白').then(msg=>console.log(msg))
    // 只有当两个 await 操作都执行完，才会执行 then 里的 console.log
    ```
3. `await` 命令
  正常情况下，`await` 命令后面是一个 `Promise` 对象。如果不是，就返回对应的值。
    ```JavaScript
    async function fn() {
      // 等同于
      // return 123
      return await 123
    }

    fn().then(v => console.log(v))
    // 123
    ```
    只要一个 `await` 语句后面的 `Promise` 变为 `reject` ，那么整个 `async` 函数都会中断执行。有时候我们希望即使前一个异步操作失败，也不要中断后面的异步操作，可以将这个 `await` 放在 `try...catch` 里，这样不管这个异步操作是否成功，第二个 `await` 都会执行。
    ```JavaScript
    async function fn() {
      try {
        await Promise.reject('出错了')
      } catch(e) {
      }
      return await Promise.resolve('hello world')
    }

    fn().then(v => console.log(v))
    // hello world
    ```
    如果有多个 `await` 则可以将其都放在 `try...catch` 里。  
  
### 4. `async` 实现原理
```JavaScript
async function fn(args) {
  // ...
}

// 等同于

function fn(args) {
  return spawn(function* () {
    // ...
  })
}
```
所有的 `async` 函数都可以写成第二种形式，`spawn` 函数就是自动执行器。  
`spawn` 函数的实现，就是上文自动执行器的翻版：  
```JavaScript
function spawn(genF) {
  return new Promise(function(resolve, reject) {
    const gen = genF()
    function step(nextF) {
      let next
      try {
        next = nextF()
      } catch(e) {
        return reject(e)
      }
      if(next.done) {
        return resolve(next.value)
      }
      Promise.resolve(next.value).then(function(v) {
        step(function() { return gen.next(v)})
      }, function(e) {
        step(function() { return gen.throw(e)})
      })
    }
    step(function() { return gen.next(undefined)})
  })
}
```

## 七、 总结
以上就是 `JavaScript` 异步处理的发展过程，从繁杂到精美，保留核心业务功能。