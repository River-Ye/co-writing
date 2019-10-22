---
title: JS 0829 AstroCamp
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags:  Javascript
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}

TimeCode
15:20 require


為什麼需要Rails migration 檔 => 把資料庫的變動轉成 code
過去不同組做完專案 PUSH 上去 github，拉下來之後發現檔案都會爆掉，因為大家的 table 長不一樣
跑 db:migrate 的時候他會去認 migration 檔前面的時間有沒有跟本地端的資料庫同步，如果沒有的話她就會去更新

reverse comparing => 可以減少手殘機率
``` javascript
a==1 改成 1==a，這樣萬一少打一個 = ，1=a會報錯
``` 
ES6

![](https://i.imgur.com/sFoAbvH.png)




function 簡化流程
![](https://i.imgur.com/knsBOae.png)

foo2 只有一個參數a時可以省略（）變成foo3
foo3 只有一行code時可以省略{return }變成foo4

```javascript=
var a = 999

let obj = { 
    a: 100,
    b: function() { console.log(this.a) }
}
//  呼叫時決定 this
obj.b() //100

var obj2 = {
    a:200,
    b: () => { console.log(this.a) }
    
}
//定義時決定 this
obj2.b() //999
```
>箭號函式不要用 this
>
```javascript=
function getObj3(){
    var a = 300
    return () => console.log(this.a) //????
}

let b = getObj3()

console.log(b)//??? 
```


```javascript=
let getAry = () => [1,2,3,4,5]
    
let ary = getAry()
console.log(ary)
```


```javascript=
//let getObj = () => {a:1, b:2} 
//failed  因為＝>後面的第一組{}會被認為是function body
let getObj = () => ( {a:1, b:2} )

let obj = getObj()
console.log(obj)
```

屬性簡寫：
變數 和 key值相同時，適用。
![](https://i.imgur.com/51jUDfy.png)

方法簡寫：

解構賦值

```javascript=
let person = {
    name: "Ann",
    age: 18,
    interests: ['Jazz','tango']
}

let {name: userName} = person

console.log(userName) //Ann
```
```javascript=
let person = {
    name: "Ann",
    age: 18,
    interests: ['Jazz','tango']
}

let {interests, ...rest} = person

console.log(interests) //
console.log(urest) //
```
Recursive
```javascript=
let numbers = [1,2,3,4,5,6,7,8,9,10]

function sum(ary) {
    if(ary.length === 0){return 0}
    let[head,...tail] = ary //head:1 tail: [2~9]
    return head + sum(tail)
}
console.log(sum(numbers))
```
原理
``` javascript
let [head,...tail] = [1]
console.log(head) //1
console.log(tail) //[]

let [head2,...tail2] = []
console.log(head2) //undefined
console.log(tail2) //[]
``` 
精簡
```javascript=
let numbers = [1,2,3,4,5,6,7,8,9,10]

function sum([head,...tail]) {
    if( head === undifined ){return 0}
    return head + sum(tail)
}
console.log(sum(numbers))
```

空陣列第一個值是undefined
空陣列的尾巴還是空陣列
一個陣列的尾巴也是空陣列

head因為陣列內沒有東西所以顯示undifined，tail因為除了head的undifined之外沒抓到東西，所以回傳空陣列
```javascript=
let name = person.name
let age = person.age
let gender = person.gender

#等同上面三行
let {name, age, gender} = person
```

## regular expression
***
``` javascript
"regex".match(/regex/) // 回傳符合的陣列
/regex/.exec("regex")
/regex/.test("regex") // 回傳布林值
``` 
回傳的陣列長得像下面這樣
![](https://i.imgur.com/nD7gpQk.png)
看起來像陣列但其實並不是，可以取值，但不能做map之類的運算

## 條件regular expression

```javascript=
問號前面的字，可以存在或不存在（零個或一個以上）
/ab?c/.exec("abc") //可   
/ab?c/.exec("ac")  //可
/ab?c/.exec("aec") //不可
/ab?c/.exec("abbbbbbc") //不可

加號前面的字，一個以上
"abde".match(/abc+de/)

星號前面的字，零到多個都可以
"abccccde".match(/abc*de/) 
```

```javascript
"02-2882-5252".match(/0\d-?\d{4}-?\d{4}/)
//翻譯: 0開頭加上1個號碼，-可要可不要，後面都要各跟4個數字
//可簡化為
"02-2882-5252".match(/0\d(-?\d{4}){2}/)
```


