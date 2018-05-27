---
title: JS实现jQuery的API
date: 2018-05-27 19:21:42
tags: [JavaScript] 
---
使用原生 `JavaScript` 实现类似于 `jQuery` 的 API。
<!-- more -->
```JavaScript
window.jQuery = function(node){
	let myNode = document.querySelectorAll(node)
	myNode.addClass = function(myClass){
		for(let i = 0;i < myNode.length;i++){
			myNode[i].classList.add(myClass)
		}
	}
	myNode.setText = function(myText){
		for(let i = 0;i < myNode.length;i++){
			myNode[i].textContent = myText
		}
	}
	return myNode
}
window.$ = jQuery

var $div = $('div')
$div.addClass('blue') // 可将所有 div 的 class 添加一个 red
$div.setText('hi') // 可将所有 div 的 textContent 变为 hi
```

1. 首先在 `window` 对象中定义一个 `jQuery` 方法，该方法接收一个参数 `node` 。
2. 在方法 `jQuery` 中，调用 `document.querySelectorAll`，将传入的字符串参数 `node` 把获取到的字符串通过该 API 获取成一个对象（伪数组），存入变量 `myNode` 中。
3. 给 `myNode` 设置两个方法： `myNode.addClass` 以及 `myNode.setText` 。
4. 两个方法都会将 `myNode` 对象进行遍历，然后通过参数 `myClass` 或 `myText` 对 `myNode` 里的节点进行操作。
5. 返回该对象。
6. 将 `jQuery` 的内存地址赋值给 `window.$` 。
7. 此时，将需要设置的元素 `$('node')` （括号里应为字符串）赋值给变量 `$node` ，即可通过 `$node` 调用 `addClass` 或 `setText` 方法。