---
title: Javascript 190821 AstroCamp
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: Javascript, 
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}

摸寧
早喔
おはようございます！
10:53 其他集合類型

# ==Recap==

## Review

simple Array
```javascript
let arr = [1,2,3,"haha","hoho"]
```

simple Object
```javascript
let hsh = {
    key: "value", "anotherkey": 100,
    "special key here": true
}
```
==value could be Object too==

Object include Array

## 其他集合類型：
- Set: 不重複的有序集合。基本上跟Array一樣，只是裡面不能塞重複的元素。
- Weak Map: key不一定是字串，可以是object。

# 變數作用域
文章參考:https://ithelp.ithome.com.tw/articles/10199428
```javascript=


function foo (){
  var x = 1;
  console.log(x)
}

foo()
console.log(x)      //印出1,error
```

```javascript=
var x = 1 ;
var y = 999 ;

function foo (){
  var x = 2;
  console.log(y)
}

foo()
console.log(x)      //印出999,1
```

```javascript=
function foo(){
  z = 100
}
foo()               // 有呼叫foo()，才印得出z
console.log(z)      //印出100
// JS會非常“貼心”的把 z 設定為全域變數
```


# ==Hoisting==
>https://developer.mozilla.org/zh-TW/docs/Glossary/Hoisting
```javascript=
foo();

function foo() {
  console.log("lalala")
}
```
## hoisting anout var
var x = 1寫在最下面的話，JS會幫你提升變數的“宣告”，但不會提升變數的“賦值”，所以一般在寫的時候會自主把宣告往上提升；
用let 的話不需提升（但老人XD習慣還是會這麼做）
## hoisting about function 
具名函式會連body一起hoisting（提升）
白話文:只要有寫方法foo，不管寫在哪裡都可以呼叫得到他，但寫程式的好習慣，變數宣告的地方越接近使用地方越好。

```javascript=
for(var i = 0; i <100; i++){

}
console.log(i) //會印出100，因為 i 被 hoisting
```
```javascript=
for(let i = 0; i <100; i++){

}
console.log(i)  //改用let 就會比較像一般的程式語言
```

- 需要重複產生相同結構時，可以建立一個Factory來處理

JavaScript所做的最好決定就是把函式當一等公民，可以獨立存在。

匿名函式，不會被 hoisting
```javascript=
foo()

var foo = function () {

        }
```

```javascript=
console.log(console.log)

console.log(console)
```
console.log是一個函式
console是一個object
```javascript=
let p = console.log

p("lalala")
```

# ==高階函式：map reduce filter==
```javascript=
let r = [1,2,3,4,5].map(addTwo)

console.log(r)

function addTwo(i){
    return i + 2
}
```
![](https://i.imgur.com/irrUzfT.png)

```javascript=

let r = [1, 2, 3, 4, 5].map(addTwo).filter(isEven)
console.log(r)

function addTwo(i){
  return i += 2
}

function isEven(i){
  return i % 2 == 0
}

// [ 4, 6 ]
```
```javascript=
let r = [1, 2, 3, 4, 5].map(function(i) {return i + 3})
                       .filter(function(i) {return i % 2 === 0})
// [4, 6, 8]
```
```javascript=
    let r = [1, 2, 3, 4, 5].map(i => i + 2)
                           .filter(i => i % 2 === 0)  
                           .reduce(i => i + 2)
```


```javascript=
function boomboom(i) {
    console.log(`boomboom! ${i} is ready`)
}
function putInPot(i) {
    console.log(`put ${i} in pot`)
}

function cook ( ig, sauce,cookMethod){
    cookMethod(ig)
    cookMethod(sauce)
    console.log(`${sauce} ${ig} is ready!`)
}

cook( "chicken","coconut", boomboom)
cook( "beef","oil", putInPot)

// boomboom! chicken is ready
// boomboom! coconut is ready
// coconut chicken is ready!
// put beef in pot
// put oil in pot
// oil beef is ready!

```
可簡化為
```javascript=
function cook ( ig, sauce,cookMethod){
    cookMethod(ig)
    cookMethod(sauce)
    console.log(`${sauce} ${ig} is ready!`)
}
cook('chicken', 'coconut', i => console.log(`boomboom! ${i} received`))
cook('beef', 'oil', i => console.log(`put ${i} in put`))

//"boomboom! chicken received"
//"boomboom! coconut received"
//"coconut chicken is ready"
//"put beef in put"
//"put oil in put"
//"oil beef is ready"
```

型別宣告(TextScript)
```javascript=
strName
funCookMethod
```
 
>理解TypeScript 
https://www.youtube.com/watch?v=seNBnxXHj9E


==mindset==

```javascript=

function start (opts) {
    let w =getWashElement(opts.wash)
    let de =getWashElement(opts.dehy)
    let dry =getWashElement(opts.dryTemp, opts.dryTime)

}

start(
    wash : 'normal', 
    dehy: 40, 
    dryTemp:60,
    dryTime:30) 
```
==Skill==
```javascript=
let wm = {
wash: wash,
dehy: dehy,
dry: dry
}

//資料結構深層，另外用一個淺層的wm 去收集
//ES6 中:
//drop key, if key same as value 
```

