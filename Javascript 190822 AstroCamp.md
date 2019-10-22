---
title: Javascript 190822 AstroCamp
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: Javascript, jQuery
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}
在非洲，每六十秒，就有一分鐘過去
在台灣也是
早安。
摸寧
Chào buổi sáng
叫天天不應，叫哥哥布林殺手
Bonjour tout le monde.

---


![](https://i.imgur.com/taYxNr8.png)

![](https://i.imgur.com/jvVtSvV.png)

目前很少用 Jquery 做動畫，因為 CSS3 的效能比較好，而且也能做很多動畫，目前 Jquery 有 99% 機率是只拿來綁 class
![](https://i.imgur.com/bIMeWLk.png)

![](https://i.imgur.com/BSmVdyo.png)

binding event with on(pair with off)

why $(document).ready()
![](https://i.imgur.com/UDHIPqM.png)

![](https://i.imgur.com/UtoCteD.jpg)


>CORS
>https://ithelp.ithome.com.tw/articles/10204004

>IIFE
>https://developer.mozilla.org/zh-TW/docs/Glossary/IIFE
>
```javascript=
<script src="/path/to/your.js"></scrpt>
```
 * 預設用法
 * 下載後會先執行，執行完此js後頁面才會繼續繪製下去。
 * 如果這個js很大或是要執行不少時間，畫面會卡卡的

在HTML5中新增屬性 async, defer
async屬性
```javascript=
<script async src="/path/to/your.js"></scrpt>
```
 * 一定要搭配 src的屬性才有作用
 * 下載後會先執行，但執行此js同時也繼續載入頁面及執行其他js。
 * 目前的所有瀏覽器都支援(2016)
 * 使用時機：此js和其他的js無連帶關係，即不需等待其他的js執行完，可獨立作業。

defer屬性
```javascript=
<script defer src="/path/to/your.js"></scrpt>
```
 * 一定要搭配 src的屬性才有作用
 * 要整個頁面都下載及分析完成後才會執行，非常類似於把js放在頁尾的情況。
 * 目前的所有瀏覽器都支援(2016)
 * 使用時機：此js一定要頁面全繪完再執行才行


Javascript & BOM & DOM

[BOM & DOM](https://www.happycoding.today/posts/43)


DOM level 2 事件模型分成兩個階段：
capture phase 
bubbling phase  <= jQuery 只處理這一段

![](https://i.imgur.com/YDLGwFf.png)
https://dotblogs.com.tw/neil_coding/2018/11/08/event_phase

![](https://i.imgur.com/9X18fYj.png)

proxy and CDN(cloudflare)

可以用 dev tool 的 source確認自己的檔案有沒有正常被讀取
![](https://i.imgur.com/VqjJIh2.png)

:::danger
 this 在使用箭頭函式"=>"時，會失效(fail)

:::

解決bubbling phase的問題:
如果只加了以下
``` JS
$('#users-list').on('click', function(){
    console.log('hoo')
  })
```
會發生不管是點到 ul 還是 li 都會冒出 hoo 的問題，這就是因為 bubbling phase，點到 li， ul 也會認為他被點到
![](https://i.imgur.com/C6qNUV3.png)

因此要在 li 被點到的時候，讓他停止 bubble
```javascript=
$('#users-list > li').on('click',function(){
if($(this).hasClass('focused')){
  $(this).removeClass('focused')
} else{
  $('#users-list > li').removeClass('focused') 
  $(this).toggleClass('focused')
}
return false
})
```
```javascript=
$('#users-list > li').on('click',function(evt){
    evt.stopPropagation()
    if($(this).hasClass('focused')){
      $(this).removeClass('focused')
    } else{
      $('#users-list > li').removeClass('focused') 
      $(this).toggleClass('focused')
    }
  })
```


```javascript=
$('#users-list > li').on('click', function(evt){
    console.log(arguments)
    evt.stopPropagation()
    // console.log('lalala')
    if ($(this).hasClass('focused')) {
      // console.log('if true')
      $(this).removeClass('focused')
    } else {
      // console.log('if false')
      $('#users-list > li').removeClass('focused')
      $(this).addClass('focused')
    }
    // return false
  })
```
---
```javascript=
 $('#users-list > li').on('click', function(event) {
      event.stopPropagation()
      // $(this).toggleClass('focused')
        if($(this).hasClass('focused')) {
           $(this).removeClass('focused')
      } else {($('#users-list > li').removeClass('focused'))
           $(this).addClass('focused')
      }
      // return false
    })
```
想要做到點擊其中一個li的時候，其他已被點擊的li(藍色)變回原來顏色(不然一整片藍藍的)

this: 其中一個li;
第6行:#users-list > li : 全部的li

else後面的解釋:
當全部li的focused被移除的時候，就讓其中一個li加入focused。




---
暫時借放
![](https://i.imgur.com/Rh4fZvD.png)
Js大坑洞


==.reduce in Ruby and JS==
```ruby=
#irb
>[1,2,3,4].reduce([]){|x,i| x + [i*2] }
=> [2, 4, 6, 8]
```
```javascript=
//node
> [1,2,3,4].reduce((x,i) =>  x + i*2  ,[])
'2468'//這不是你要的
```
```javascript=
//node
> [1,2,3,4].reduce((x,i) =>  [...x,i*2]  ,[])
[ 2, 4, 6, 8 ]
```
:::danger
    在javacsript 裡，"+" 要慎用：
- 純數字運算
- 字串串接
:::



==CodeReview==
```javascript=

let table = {
      "G": "C",
      "C": "G",
      "A": "U",
      "T": "A",
  }

let ender = ['UAG', 'UAA', 'UGA']

let Rna = {
  transcript: function(str) {
    return str.split('')
              .map(c => table[c])
              .reduce( (accu, c) => checker(accu) ? accu : [...accu, c], [])
              .join('')
  }
}

function checker(accu) {
  let last3 = accu.slice(-3).join('')
  return ender.includes(last3)
}

console.log(Rna.transcript('ACGTGGTCTTAA'));
```