---
title: JS里的数据类型转换
date: 2018-05-03 18:53:47
tags: [JavaScript] 
---
`JavaScript` 是一种动态类型语言，变量是没有类型的，可以随时赋予任何值。但是，数据本身和各种运算是有类型的，因此运算时，变量需要转换类型。

# 转换为数值
## Number 函数
使用 `Number` 函数，可以将任意类型的值转换成数数值。  
* 数值： 转换之后还是原来的值。
* 字符串： 如果可以被解析为数值，则转换为相应的数值，否则会转换成 `NaN` 。空字符串转换为 `0`　。　　
* 布尔值： `true` 转换为 `1`　， `false` 转换为 `0`　。
* undefined： 转换为 `NaN` 。　　
* null： 转换为 `0` 。  
<!-- more -->

```JavaScript
Number('123321')  // 123321
Number('123321abc')  // NaN
Number('')  // 0
Number(true)  // 1
Number(false)  // 0
Number(undefined)  // NaN
Number(null)  // 0
```
  
## parseInt() 函数
`parseInt()` 函数将其第一个参数转换为字符串，解析它，并返回一个整数或NaN。如果不是NaN，返回的值将是作为指定基数（基数）中的数字的第一个参数的整数。  

1. 语法  

```JavaScript
parseInt(string,radix)
``` 

2. 参数  
`string`  
要被解析的值。如果参数不是一个字符串，则将其转换为字符串。字符串开头的空白符会被忽略。  
`radix`  
一个介于 2 和 36 之间的整数，表示上述字符串的基数。比如，参数 "10" ，表示使用十进制数值系统，通常情况下，需指定该参数，以免产生不可预见的错误。  
若没有指定基数，或基数为0， `JavaScript` 会作如下处理：  
    * 如果字符串 `string` 以 `'0X'` 或者是 `'0x'` 开头，则基数是16（十六进制）
    * 如果字符串 `string` 以 `'0'` 开头，基数是8（八进制）或10（十进制），具体由实际环境决定。<small>EMCAScript规定使用10，但是并不是所有的浏览器都遵循这个规定。因此，<strong>永远都要明确给出 `radix` 参数的值！</strong></small>
    * 如果字符串 `string` 以其他任何值开头，则基数是10（十进制）

3. 示例  

```JavaScript
parseInt('10')  // 10
parseInt('0x12')  // 18 十六进制
parseInt('010')  // 10或8 十进制或八进制，视环境而定
parseInt(' 23')  // 23
parseInt(' 23 abc')  // 23
parseInt(' 23abc')  // 23

parseInt('19', 10)  // 19
parseInt('11', 2)  // 3 二进制
parseInt('17', 8)  // 15 八进制
parseInt('19', 18)  // 27 十八进制

parseInt('2', 2)  // NaN 字符串数字大于基数
parseInt('11', 1)  // NaN 基数不在2和36之间
parseInt('abc123')  // NaN
```     

4. 拓展  
`['1','2','3'].map(parseInt)` 输出什么？  

```JavaScript
['1','2','3'].map(parseInt)

// 等同于

['1','2','3'].map(function(ele,index){
    return parseInt(ele,index)
})
```

```JavaScript
1. parseInt('1', 0)  // 基数为0时，根据参数 `radix` 里的描述可知为十进制，返回 1
2. parseInt('2', 1)  // 基数为1，不在2-36的范围内，输出NaN
3. parseInt('3', 2)  // 基数为2，字符串3大于基数，输出NaN

// 即 输出[1,NaN,NaN]
```

## parseFloat() 函数
`parseFloat()` 函数解析一个字符串参数并返回一个浮点数。  

e.g.  
```JavaScript
parseFloat('3.14')  // 3.14
parseFloat('314e-2')  // 3.14
parseFloat('0.0314E+2')  // 3.14
parseFloat('3.14whatever')  // 3.14

parseFloat('FF2')  // NaN
```

## 运算符
`JavaScript` 遇到预期为数值的地方，就会将参数值自动转为数值。系统内部会自动调用 `Number` 函数。除了加法运算符（ `+` ）有可能把运算子转为字符串，其他运算符都会把运算子自动转为数值。  

e.g.  
```JavaScript
'5' - '2'  // 3
'5' * '2'  // 10
true - 1  // 0 
false + 1  // 1
'1' - 1  // 0
'5' * []  // 0 
false / '5'  // 0
'abc' - 1  // NaN
null + 1  // 1 null转为数值时为0
undefined + 1  // NaN undefined转为数值时为NaN
```
一元运算符也会把运算子转为数值。
e.g.
```JavaScript
+'abc'  // NaN
-'abc'  // NaN
+true  // 1
-false // -0
```

# 转换为字符串
## String() 函数
`String` 函数可以将任意类型的值转换为字符串。
* 数值： 转为相应的字符串。
* 字符串： 转换后还是原来的值
* 布尔值： `true` 转换为字符串 `'true'`， `false` 转换为字符串 `'false'`
* undefined： 转换为字符串 `undefined`
* null： 转换为字符串 `null`

```JavaScript
String(123)  // 123
String('abc')  // 'abc'
String(true)  // 'true'
String(undefined)  // 'undefined'
String(null)  // 'null'
```

如果 `String` 方法的参数是对象，返回一个类型字符串；如果参数是数组，则返回该数组的字符串形式。

```JavaScript
String({a: 1}) // "[object Object]"
String([1, 2, 3]) // "1,2,3"
```

##  x.toString() 函数
```JavaScript
(1).toString()  // '1'
true.toString()  // 'true'
null.toString()  // 报错
undefined.toString()  // 报错
[1,2,3].toString()  // '1,2,3'
{}.toString()  // unexpected token .
({}).toString()  // '[object Object]'
```

## 运算符
字符串使用加法运算时，会产生字符串的自动转换。当一个值为字符串，另一个值为非字符串，则后者转换为字符串。  
```JavaScript
'5' + 1  // '51'
'5' + true  // '5true'
'5' + false  // '5false'
'5' + {}  // '5[object Object]
'5' + []  // '5'
'5' + [1,2,3]  // '51,2,3'
'5' + function(){}  // '5function(){}'
'5' + undefined  // '5undefined'
'5' + null  // '5null'
```

# 转换为布尔值
## Boolean(x)
```JavaScript
Boolean(123)  // true
Boolean(0)  // false
Boolean('123')  // true
Boolean('')  // false
Boolean(null)  // false
Boolean(undefined)  // false
Boolean([])  // true
Boolean({})  // true
```

## !!x
```JavaScript
!!123  // true
!!0  // false
!!'123'  // true
!!''  // false
!!null  // false
!!undefined  // false
!![]  // true
!!{}  // true
```

## 五个 falsy 值
`''` , `0` , `NaN` , `null` , `undefined` 