---
title: JS 0815 AstroCamp
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: Javascript 
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}


建議不用jsbin的時候要關掉，今早發現他很耗效能......電腦發燙嚇了我一跳ＸＤ
原來我昨天筆電差點飛走是這個原因 lol
:::danger
不用jsbin的時候要關掉，他很耗效能
:::


---


:::warning
- 比較時，盡量用 “===”
- 呼叫function時，用foo( )
- 定義function 時，用{} 將範圍明確訂出來
    - function foo( ){ 
        //do something...
        }
:::

```javascript=
function fizzBuzz(number) {
  // 三的倍數回傳 'Fizz', 五的倍數回傳 'Buzz', 又是三又是五的倍數回傳 'FizzBuzz'
  // 除此之外回傳原數字的字串
  // put your code here

  if (number % 15 == 0) {
    return "fixxBizz";
  } else if (number % 3 == 0) {
    return "Fizz";
  } else if (number % 5 == 0) {
    return "Buzz";
  } else {
     return number.toString();
  }
}

let result = fizzBuzz(15);
console.log(result); // should print 'fizzBuzz'
```
>可以把條件設成變數來寫
>應該要回傳原數字的“字串” :cry: 
>

# ==Array==
```javascript=
//wire 
    
```


attribute and method
![](https://i.imgur.com/YWyYwed.png)
## ==常用屬性與方法(length, join, indexOf, concat)==
```javascript=
//常用屬性與方法
arr.length //這裡的length不用括號，因為它不是方法，只能算是一種屬性
arr.join()
arr.indexOf()
arr.concat()
```
![](https://i.imgur.com/3IWvHEv.png)


```javascript=
let chars = ['a','b','c']
chars.join()
chars.join('|')
chars.join('')

```


## ==比較少用的（pop, shift,  push, unshift）==
```javascript=
let chars = ['a','b','c']
chars.pop()   //把最後一個元素丟出來，改變原始資料
chars.shift() //把第一個元素丟出來，改變原始資料

chars.push('z')   //添加在最後面，改變原始資料
chars.unshift('e')//添加在最前面，改變原始資料

//FIFO)(shift) vs LIFO(pop)
```
## ==mutation== 會不會改變原始資料？ 
.slice 不會改變陣列本身
.splice 會改變陣列本身

## ==slice and splice==
```javascript=
> fruits2 = [ 'apple', 'banana', 'orange', 'pear' ]

> fruits2
[ 'apple', 'banana', 'orange', 'pear' ]

> fruits2.slice(1)
[ 'banana', 'orange', 'pear' ]

> fruits2
[ 'apple', 'banana', 'orange', 'pear' ]

> fruits2.splice(1, 1, 'x', 'y')
[ 'banana' ]

> fruits2
[ 'apple', 'x', 'y', 'orange', 'pear' ]
```
![](https://i.imgur.com/WOF9ord.png)

Prototype 的method是實體方法=> 是在這個類別的實體上操作，如果不是prototype就是用類別方法，需用類別呼叫


## ==展開巢狀陣列 flat==
```javascript=
> chars = [ 'a', 'b', 'c' ]

> chars
[ 'a', 'b', 'c' ]

> chars.flat()
[ 'a', 'b', 'c' ]

> let nested = [['a', 'b'], 'c', [], ['d']]
undefined
//空陣列會不見

> nested.flat()
[ 'a', 'b', 'c', 'd' ]
```
![](https://i.imgur.com/672IrNm.png)

## ==發散與收合 spread (...)==


```javascript=
> chars = [ 'a', 'b', 'c' ]

> chars
[ 'a', 'b', 'c' ]

> let newChars = [...chars, 'd']
undefined

> chars
[ 'a', 'b', 'c' ]

> newChars
[ 'a', 'b', 'c', 'd' ]

```
![](https://i.imgur.com/m9VSdJF.png)


```javascript=
> Math.max(1, 2, 3)
3

> n = [1, 2, 3]
[ 1, 2, 3 ]

> Math.max(...n)
3
```
![](https://i.imgur.com/1SFAW9c.png)

>echo $PATH
>

# ==iteration==
```javascript=
//ES6
for...of


```
```javascript=
let langs = ["javascript", "ruby", "Elixier","Haskell"]

langs = [...langs , "haha", "yoyo"].slice(1) 
//or
//langs = [...langs.slice(1) , "haha", "yoyo"] 

for (let lang of langs){
  console.log(`I love ${lang}`)
}
```
![](https://i.imgur.com/57YrvjS.png)
or
![](https://i.imgur.com/6zbD9kP.png)

>Q.push 和 slice 的回傳值？[color=#08a068]
>push 回傳的是 ==lenght of new array==
>slice 回傳的是 ==new array==
 
```javascript=
> arr = [1,2,3]
[ 1, 2, 3 ]
> arr.push(4)
4
> arr.push("z")
5
> arr.slice(1)
[ 2, 3, 4, 'z' ]

```

## 注意事項：
:::danger
==寫語法的大原則是 mutable 跟 immutable方法要分開寫，別同一行==
:::

## ==key-value pair==
>(a.k.a. Object in javascript)
>鍵值對是無序排列的[color=#08a068][]
用途：
-  查表
-  表示一個 Object 的狀態


```javascript=
> user
{ name: 'bob', age: 18 }

> Object.entries(user)
[ [ 'name', 'bob' ], [ 'age', 18 ] ]

> Object.keys(user)
[ 'name', 'age' ]

> Object.values(user)
[ 'bob', 18 ]

> Object.freeze(user)

user.age = 33;
// Throws an error in strict mode
// Object 中被 freeze 的數值不能被改動
// 18
```
老師例子完整版
```javascript=
let classRoom = {
  name : '308',
  teacher : { name: 'Namie', age: 40},
  assistant : { name: 'Pat', age: 20},
  students : [
  { name: 'Mary', age: 10}, 
  { name: 'Andy', age: 12},
  ]
}
function allAges(classRoom) {
  let teacherAge = classRoom.teacher.age
  let assistantAge = classRoom.assistant.age
  let studentAges = []
  for(s of classRoom.students) {
    studentAges.push(s.age)
  }
  return [teacherAge, assistantAge, ...studentAges]
}
console.log(ages = allAges(classRoom))
```
![](https://i.imgur.com/nqBqk6c.png)

## ==小組討論資料結構：購物車篇（純分享）==
我們討論經過的步驟：
1.列出所有購物車會有的東西
2.將同種類的進行分類
3.將重複的功能刪除
4.列出進入購物車選單後的順序流程
5.不會變動的功能與會變動的功能進行分類
6.會有value的用Objuect，不用特別輸入value的用Array
7.寫成code

所以產出了以下基本結果：
```
購物車：
{帳號、密碼}
購物車 {商品名稱、金額、特價、數量、尺寸、顏色、折扣{購物金、活動優惠、紅利折抵}、總件數
推薦商品、瀏覽紀錄}
待買清單
追蹤清單
商城規範
會員資料 {姓名、信箱、電話、地址、會員等級}
配送方式 {超商: 運費、宅配: 運費、郵局: 運費、面交: 免運費}
付款方式 [ATM、信用卡、貨到付款、第三方支付]
發票載具 [二聯、三聯、捐贈、電子發票]
訂單編號

123代表型態是number, 0.7代表型態是float
```
寫成code並分類後：
```javascript=
let cart = {
  user: { 帳號: "", 密碼: "" },
  cart: {            //問題零
    item: {          //問題一
      商品名稱: "",
      金額: 123,
      特價: 123,
      數量: 123,
      尺寸: 123,
      顏色: "",
      折扣: 0.7
    },             
    price: { 購物金: 123, 活動優惠: "", 紅利折抵: 123, 總金額: 123 },          
                                             //問題二
    recommand: {
      推薦商品: { 商品名稱: "", 金額: 123, 特價: 123 },
      瀏覽紀錄: ""    //問題三
    }
  },
  list: { 待買清單: "" },    //問題四
  trace: { 追蹤清單: "" },   //問題四
  member: {                //問題四
    姓名: "",   
    信箱: "",
    電話: "",
    地址: "",
    會員等級: 123
  },
  delivery: { 超商: 123, 宅配: 123, 郵局: 123, 面交: 0 },
  payment: ["ATM", "信用卡", "貨到付款", "第三方支付"],
  reciept: ["二聯", "三聯", "捐贈", "電子發票"]
};
```
泰安老師的評語：
1.第3行，問題零：
Ans: 最外面已經let card了，裡面的key要避免使用同樣名稱的命名。

2.第4行～第12行，問題一：
問：item這裡只有一個，如果今天有多個item的話，這樣子的資料結構可以寫在哪裡呢？
(此時我在台上空氣凝結...)
Ans: 這種寫法基本上可說是設計錯誤，應該會如下圖寫，用Array包住Object才能夠在資料上儲存多筆item。
```javascript=
    item: [{          //問題一
      商品名稱: "",
      金額: 123,
      特價: 123,
      數量: 123,
      尺寸: 123,
      顏色: "",
      折扣: 0.7}, {}, {} , {}......] //[]裡面就放第2行~第8行的Object
```

3.第13行，問題二：
問：那個總金額是...?直接輸入字串？
Ans:關於要做運算的事情，不需要寫在資料結構裡面，只要再外面用function運算完後再輸入回去就好。
![](https://i.imgur.com/0x0XzmF.png)

4.第16行~第17行，問題三：
問：商品名稱跟瀏覽紀錄..?一個字串？
Ans:同問題一，應有多筆資料的儲存應用Array包住Object。
```javascript=
recommand: [
      {推薦商品:'', 商品名稱: "", 金額: 123, 特價: 123},
      {瀏覽紀錄: "",瀏覽紀錄: "",瀏覽紀錄: ""}
      }], {} ,{}...
```

5.第20行～第21行，第22行～第27行，問題四：
Ans:同問題一，應有多筆資料的儲存應用Array包住Object。
```javascript=
list: [{ 待買清單: "" },{},{},{}...]  //問題四
trace: [{ 追蹤清單: "" },{},{},{}...]
member: [{               
    姓名: "",   
    信箱: "",
    電話: "",
    地址: "",
    會員等級: 123
    },{},{}....
```
補充：在item的部分，其實並不是你在Array裡寫幾個Object，就只能在購物車裡放幾個不同item，而是一開始的資料其實是個空陣列，當使用者按下"加入購物車"後，才會傳入一個資料進入Array。

結論：
1.若是同樣的東西，卻需要有多筆的時候，使用Array包住裡面的資料。
2.若只有單筆需要帶入key跟value的時候，使用Object。(第２９行)
3.避免key的命名與外部的變數名稱相同。

當然就算這樣設定，這樣的資料結構其實也還算是有問題的，但藉由此例大概可以對第一次寫資料結構的我們，有了一些些感受跟體驗。
