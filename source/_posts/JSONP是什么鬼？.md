---
title: JSONP是什么鬼？
date: 2018-06-20 09:10:11
tags: [JavaScript,JSONP]
---
# 同源政策
1995年，NetScape公司（火狐前身）提出了浏览器的同源政策，目的是保护使用网站的用户的信息安全。  
所谓的同源是指“三个相同”：  
* 协议相同
* 域名相同
* 端口相同  
  
<!-- more -->
举个例子，`http://www.abc.com/save/index.html` 这个网址，`http://` 是协议，`www.abc.com` 是域名，端口是 `80` （默认端口，可以省略）。  

# 浏览器向服务器提交数据
浏览器向服务器提交数据，可以使用 `form` 表单。以下模拟用 `form` 表单付款这一操作。  
```HTML
<h5>您的账户余额是<span id="amount">200</span></h5>
<button id="button">付款1块钱</button>
<form action="/pay" method="post">
    <input type="text" name="number" value="1">
    <input type="submit" value="付款">
</form>
```
使用 `POST` 发起请求有一个致命缺点，就是请求的成功失败，只能在页面刷新后才能知道。此时可以使用 `<iframe>` 标签在本页面显示请求结果。
```HTML
<form action="/pay" method="post" target="result">
  <input type="text" name="number" value="1">
  <input type="submit" value="付款">
</form>

<iframe name="result" src="about:blank" frameborder="0"></iframe>
```
这样一来，请求结果就可以不用刷新，直接在页面内做响应。  
账户余额 `200` 也不需要写死，可以用占位符表示，并从服务器获取。
```HTML
<!-- 页面 -->
<h5>您的账户余额是<span id="amount">&&&amount&&&</span></h5>
```
```JavaScript
// JS
button.addEventListener('click',(e) => {
    let n = amount.innerText
    let number = parseInt(n,10)
    let newNumber = number - 1
    amount.innerText = newNumber
})
```
```JavaScript
// 服务端
var amount = fs.readFileSync('./db','utf8')
string = string.replace('&&&amount&&&',amount)
......
response.wrire(string)
```

# 其他方式向服务端发起请求
众所周知，`<img>`、`<link>`等标签都可以发起请求，例如：
```JavaScript
let image = document.createElement('img')
image.src = '/pay'
image.onload = () => {
    alert('打钱成功')
    amount.innerText -= 1
}
image.onerror = () => {
    alert('打钱失败')
}
```
使用这类方法也可以在页面中做出响应，但也只能发起 `GET` 请求，可以说是浏览器端发起请求的一大奇招。  
同时，伟大的前端程序员们将目光转向了 `<script>` 标签。
  
# 动态创建JS脚本发起请求
动态创建JS，并在页面中插入：
```JavaScript
//用js脚本发起请求  
    let script = document.createElement('script')
    script.src = '/pay'
    document.body.appendChild(script)
    script.onerror = function() {
      alert('failed')
    }
    
   ...
   //服务器端一般这么干
   if(path === '/pay') {
    var amount = fs.readFileSync('./db', 'utf8')
    var newAmount = amount - 1
    
    fs.writeFileSync('./db', newAmount)
    response.setHeader('Content-Type','application/javascript')
    response.statusCode = 200
    response.write(`
      amount.innerText = amount.innerText - 1
    `)
    response.end()
  }
```
在动态创建JS之后，需要在页面中插入该 `<script>` 标签，才会发起请求。  
如果这么操作，每发起一次请求，就会在页面中新增一个 `<script>` 标签，所以需要在其创建后立即移除：  
```JavaScript
// 用js脚本发起请求  
    let script = document.createElement('script')
    script.src = '/pay'
    document.body.appendChild(script)
    script.onload = function(e) {
      e.currentTarget.remove() //加载完了，就移除
    }
    script.onerror = function(e) {
      alert('failed')
      e.currentTarget.remove() //加载完了，就移除
    }
```
虽然把标签移除了，但是标签所造成的影响已经反应出来了。这种方案叫做 `SRJ` （Server rendered javascript）服务器返回的JavaScript。  

# 请求另一个网站的script
```JavaScript
button.addEventListener('click', (e) => {
    //用js脚本发起请求  
    let script = document.createElement('script')
    let functionName = 'qing' + parseInt((Math.random()*100000), 10)
    window[functionName] = function (result) {
      if (result === 'success') {
        amount.innerText = amount.innerText - 1
      } else {
      }
    }
    script.src = 'http://想访问的另一个网站:端口号/pay?callback=' + functionName
    document.body.appendChild(script)
    script.onload = function(e) {
      e.currentTarget.remove()
    }
    script.onerror = function(e) {
      alert('failed')
      e.currentTarget.remove()
    }
  })
```
代码中：  
1. `let functionName = 'qing' = parseInt(Math.random()*100000,10)`  
随机构建自己的函数名字，可以与服务器代码完美解耦，服务器只需要获取查询参数 `?callback=functionName` 里的 `functionName` 就可以了。  
2. `window[functionName] = function (result) {}` 在window全局对象上添加 `functionName` 属性，该属性是一个函数，当服务端返回响应之后，浏览器端的写的函数的参数就是服务器端的success，我们就知道我的数据成功了。
    ```JavaScript
    // 服务器端只需要这样就可以了，不关心你写的是什么函数名字  

    response.write(`
    ${query.callback}.call(undefined, 'success')
    `)
    ```
    当然不要忘记把全局对象的属性去掉：
    ```JavaScript
    script.onload = function(e) {
        e.currentTarget.remove()
        delete window[functionName]
    }
    script.onerror = function(e) {
        alert('failed')
        e.currentTarget.remove()
        delete window[functionName]
    }
    ```
  
# jQuery实现
强大的 `jQuery` 可以十分简洁明了地实现以上代码所实现的功能：  
```JavaScript
$.ajax({
    url: "http://想访问的另一个网站:端口号/pay",

    // The name of the callback parameter, as specified by the YQL    service
    jsonp: "callback",

    // Tell jQuery we're expecting JSONP
    dataType: "jsonp",

    // Work with the response
    success: function (response) {
        if(response === 'success') {
        amount.innerText = amount.innerText - 1 
        }  
    }
})
```
虽然是用 `$.ajax` 实现的，但是这个方法与 `ajax` 并没有什么关系。
  
# 什么是JSONP？
`JSONP` （JSON with Padding）是 `JSON` 的一种使用方式，可以让网页从别的域名（网站）获取数据，即跨域读取数据。  
请求方是一个网站（服务器端），响应方是另一个网站（服务器端）。  
1. 请求方动态创建一个 `script` 脚本，属性 `src` 是响应方的地址，同时传递一个查询参数 `?callbackName=functionName` ，通常约定 `callbackName` 用 `callback` 来代替，`functionName` 通过字母+随机数来代替。  
2. 响应方根据收到的查询参数 `callbackName=functionName` ，来构造形如：`functionName.call(undefined,'success')` 的响应。
3. 浏览器收到响应，就执行 `functionName.call(undefined,'success')`  。
4. 此时，请求方就知道该请求已经成功响应了。  
  
以上就是 `JSONP` 的原理。

# 为什么JSONP不支持POST请求？
1. `JSONP` 是通过动态创建 `script` 实现的。
2. 动态创建出来的 `script` 只能用 `GET` ，不支持 `POST`。