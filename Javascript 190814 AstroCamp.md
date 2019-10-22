---
title: JS 190814 AstroCamp
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: Javascript, HTML
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}
哩後～～～
安安
谷摸寧

>技能堆疊(Skill Stack)：
>- 問題分析
>- 解法分析
>- 溝通
>- 合作、領導
>
>Full Stack = Front + End + SQL
>[color=#d302b0]

JavaScript主要是處理網頁的“動作”。
HTML處理"結構"，CSS處理視覺。

[用玩電玩的態度來說英文！(Why you should speak English like you're playing a video game | Marianna Pascal | TED PenangRoad)](http://bit.ly/2ZjTZ5c)

>摩爾定律教會我們的：不要追框架，先理解基本的原理。[color=#d302b0]

寫JavaScript的良好態度
先講求不傷身體、再講求效果。

## ==JavaScript: The Good Parts PDF==
>檔案很大 慎入orz
[JavaScript: The Good Parts PDF](https://github.com/NorthPaulo/research/blob/master/Frontend-books%26research/JavaScript%20-%20The%20Good%20Parts%20-%20Douglas%20Crockford%20-%20May%202008.pdf)

## ==BigDecimal==
>0.1 + 0.2 == 0.3 ?!? (Ruby & JS)
https://www.rubyguides.com/2016/07/numbers-in-ruby/

https://jsbin.com
![](https://i.imgur.com/UwYt6U8.png)
```ruby=
  name = 'Tai'
  p "My name is #{ name }"
 
```
```javascript=
let name = 'Tai'
console.log("My name is " + name + "!")
console.log(`My name is ${name}!`)
```
![](https://i.imgur.com/40MH1Dy.png)

![](https://i.imgur.com/rqht1hi.png)

但沒有ruby那種
![](https://i.imgur.com/eHriHTZ.png)
```ruby
  # ruby
  
  Const = "ruby"
  def meow(name)
    y = 2
    x = 1
  end
```
```javascript
    // javascript
   
    const = "javascript"
    function meow(name){
        var y = 2
        let x = 1  
    }
```

```javascript=
for (var i = 0 ; i <100; i++) {
  
}

console.log(i)

for (let a= 0 ; a <100; a++) {
  
}
console.log(a)
```
![](https://i.imgur.com/OmgcUSD.png)


慣例：
變數名稱取法
Ruby 變數 snack case
 ex. user_name = 'Pat'
JS 變數 camel case
 ex. userName = 'Pat'
程式檔名取法
函式的順序
檔案與資料夾結構

---

#  變數宣告

## ==區塊變數 let==
>自己專案 沒有支援IE8 全部都用let 
宣告過後可以改


## ==常數 Const==
>你很知道你要幹嘛的時候才用 const
宣告過後永遠都是這個值 不會去改

## ==常見，舊版 Var==

>var 與 ES6 let const 差異
https://ithelp.ithome.com.tw/articles/10209121


```javascript=
function printAddOne(x){
  console.log(x + 1)
}
printAddOne(10) 
// 11
```
# ==return== 

```javascript=
function a(x) {
  return x + 1
}
let result = a(4)

console.log(result)
```

## ==打折計算 discount==
```javascript=
function countPrice(itemPrice, itemCount){
  return itemPrice * itemCount
}

function discount(price,rate){
  return price * rate
}

let orginalprice = countPrice(10, 20)
let discountedprice = discount(orginalprice, 0.8)
console.log(discountedprice)
```
![](https://i.imgur.com/I4053IN.png)
![](https://i.imgur.com/CGu69WG.png)

小心 忘記return
![](https://i.imgur.com/6MmkiH2.png)

![](https://i.imgur.com/2QsNlHo.png)

# ==Identity==
> 加/減法的 Identity 是 0
> 乘/除法的 Identity 是 1
> 陣列的 Identity 是 [] (空陣列)
> 
Null and undefined

## ==interpolation==

```javascript
let userName = 'Tai'
let greeting = `Hello ${userName}! Nice to meet you.`

```


## ==function裡面+判斷==
``` javascript
function countPrice(itemPrice, itemCount){
  return itemPrice * itemCount
}

function discount(price){
  if (price >= 200) { 
    price *= 0.8
  } 
  else {  
    price *= 0.9
  }
  return price
}

let price = countPrice(10, 20)
let realprice = discount(price)
console.log(realprice)
```
![](https://i.imgur.com/10cby74.png)

## ==Another way to solve it==

``` javascript
function countPrice(itemPrice, itemCount){
  return itemPrice * itemCount
}

function discount(price){
  let rate = price >= 200 ? 0.8 : 0.9
  return price * rate
}

let price = countPrice(10, 20)
let realprice = discount(price)
console.log(realprice)
```

