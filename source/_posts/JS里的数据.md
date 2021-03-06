---
title: JS里的数据
date: 2018-04-30 18:53:47
tags: [JavaScript] 
---
JavaScript 里有七种数据类型，其中包括六种基本数据类型（ `Number` , `String` , `Boolean` , `Null` , `Undefined` , `Symbol` ），和一种复杂数据类型( `Object` )。
<!-- more -->
# Number
JavaScript 内部，所有的数字都是以64位浮点数的形式储存，整数也是如此。因此，`1`和`1.0`是相同的，是同一个数。  
`1 === 1.0 // true`  
这就是说，JavaScript 语言的底层根本没有整数，所有数字都是小数（64位浮点数）。容易造成混淆的是，某些运算只有整数才能完成，此时 JavaScript 会自动把64位浮点数，转成32位整数，然后再进行运算。  
由于浮点数不是精确的值，所以涉及小数的比较和运算要特别小心。
```  JavaScript
0.1 + 0.2 === 0.3  // false
0.3 / 0.1  // 2.9999999999999996
(0.3 - 0.2) === (0.2 - 0.1) // false
```

十进制：`1`, `1.0`, `.1`  
二进制：`0b1(1)`, `0b10(2)`  
八进制：`011(9)`  ！！注意！！
十六进制：`0X11(17)`  

# String
字符串就是零个或多个排在一起的字符，放在单引号或者双引号之中。  
`'你好'`，`"你好"`，`''`（空字符串），`""`（空字符串），`' '`（空格字符串），`" "`（空格字符串）  

多行字符串：  
```JavaScript
var a = '12345 \
67890'

var b = '12345' + 
        '67890'
```

ES6中，可用<strong>反引号</strong>将需要换行的字符串接起来：
```JavaScript
var c = `12345
67890` // 6之前不能有空格，包含回车字符。
```

# Boolean
`&&` ：与  
`||` ：或  
`true` ， `false`  
五个 falsy 值： `false` 、 `undefined` 、 `null` 、 `0` 、 `NaN`  
所有的 `Object` 都是 `true`。

# Null 与 Undefined
一般认为：  
1. 当一个变量没有值时，应为 `undefined`
2. 有一个非对象，不想赋值时，应为 `undefined`
3. 有一个对象，不想赋值时，应为 `null`  

# Object
简单来说，对象（ `Object` ）就是一组”键值对“（ `key-value` ）的集合，是一种无序的复合数据集合。  
```JavaScript
var obj = {
    name: 'Qing',
    age: 18
}
```
上面代码中，大括号里定义了一个对象。它被赋值给变量 `obj` ，所以变量 `obj` 就指向一个对象。该对象内部包含两个 `key-value` ，分别是 `name: 'Qing'` ， `age: 18` ， `name` 和 `age` 为键名( `key` )， `Qing` 和 `18` 为键值( `value` )，两个 `key-value` 之间用逗号分隔开。   

对象的所有键名都是字符串（ES6中也可用 `Symbol` 值作为键值），所以加不加引号都可以。以上代码也可以写成下面这样：
```JavaScript
var obj = {
    'name': 'Qing',
    'age': 18
}
```
如果键名是数值，会被自动转为字符串：
```JavaScript
var obj = {
  1: 'a',
  3.2: 'b',
  1e2: true,
  1e-2: true,
  .234: true,
  0xFF: true
};

obj
// Object {
//   1: "a",
//   3.2: "b",
//   100: true,
//   0.01: true,
//   0.234: true,
//   255: true
// }

obj['100'] // true
```
上面代码中，对象 `obj` 的所有键名虽然看上去像数值，实际上都被自动转成了字符串。

如果键名不符合标识名的条件（比如第一个字符为数字，或者含有空格或运算符），且也不是数字，则必须加上引号，否则会报错。
```JavaScript
// 报错
var obj = {
  1p: 'Hello World'
};

// 不报错
var obj = {
  '1p': 'Hello World',
  'h w': 'Hello World',
  'p+q': 'Hello World'
};
```
上面对象的三个键名，都不符合标识名的条件，所以必须加上引号。

# typeof 操作符
typeof 操作符可以返回一个值的数据类型：
```JavaScript
typeof 123 // "number"
typeof `123` // "string"
typeof false // "boolean"
```

其中有两个特殊值需特别注意：
```JavaScript
1.  typeof null // "object"

2.  function fn(){}
    typeof fn // "function"
```
1中本应返回 `"null"` ,2中本应返回 `"object"`，此为历史遗留问题。