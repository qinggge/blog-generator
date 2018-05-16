---
title: JS里的数据类型转换
date: 2018-04-30 18:53:47
tags: [JavaScript] 
---
`JavaScript` 是一种动态类型语言，变量是没有类型的，可以随时赋予任何值。但是，数据本身和各种运算是有类型的，因此运算时，变量需要转换类型。

# 转换为数字：
## Number 函数：
使用 `Number` 函数，可以将任意类型的值转换成数字。  
* 数值：　转换之后还是原来的值。
* 字符串：　如果可以被解析为数值，则转换为相应的数值，否则会转换成 `NaN` 。空字符串转换为 `0`　。　　
* 布尔值：　`true` 转换为 `1`　， `false` 转换为 `0`　。
* undefined：　转换为 `NaN` 。　　
* null : 转换为 `0` 。  

```JavaScript
Number('123321')  // 123321
Number('123321abc')  // NaN
Number('')  // 0
Number(true)  // 1
Number(false)  // 0
Number(undefined)  // NaN
Number(null)  // 0
```