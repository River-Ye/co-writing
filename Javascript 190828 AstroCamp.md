---
title: Javascreipt 190828 AstroCamp
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: Javascript
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}
熬炸
歐嗨呦勾咂一罵死

```
$(documemt).ready(() => {})
縮寫
$(() => {})
```

![](https://i.imgur.com/Mpjuxp6.png)
Jquery好用在可以很直覺推測到功能
像上面的例子中，text()是取值，text('lalala')裡面有東西代表設值
text 抓的東西會忽略 html 標籤
![](https://i.imgur.com/nePyMbn.png)
如果要抓到標籤要改用 html 這個方法
![](https://i.imgur.com/oc5DIgF.png)



### .prop()
```javascript
// 優先使用prop
    $(elem).prop('checked')
```
.attr() 用在要拿到網頁初始狀態的值，就算操作過後改變了這個值，用attr() 拿到的還是不會改變

>parallax
>
### append
```javascript
$('#btn').click(function(){
$('.foo').append($('.baz'))
 //要被加的  //要再他的內層加入什麼
 //將.baz加入.foo這個div裡面
})
```

按下按鈕前
![](https://i.imgur.com/G5UO0Al.png)
按下按鈕後
![](https://i.imgur.com/QVSl7M3.png)
另一個例子:
![](https://i.imgur.com/uXcvfJ8.png)

template 這個標籤是用來複製用的，平常瀏覽器看不到，但IE不支援，如果像 bootstrap 套件會用到一整組東西，不能隨便更動的話，這時候就是 template 最好用的地方
![](https://i.imgur.com/guqUI0h.png)

幫還不存在的的節點綁定事件：


確認父元素在網頁載完時已存在

在這個例子中，img 要按下按鈕才會出來，所以要先把事件綁在已經存在的.box 上面，img 放在 on 裡面的第二個參數
```javascript
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
  <script>
    $(document).ready(function () {

      $('.show-img').on('click', function () {
        let resource = $('.url-insert').val();
        let pic = `<img class="img" src="${resource}" alt="" />`;
        $('.box').append(pic);

      })

      $('.box').on('click', '.img', function() {
        $(this).toggleClass('focus')
      })
    })
  </script>
```

Why "Node" is not an "element"?
```javascript=
    $('.foo')
    $('<div class = "foo"><img src="" /></div>')
```
## before,after
attend, attend_to 是加入到目標裡面，befroe, afrer是加到同層的旁邊，跟css的before after很像。
![](https://i.imgur.com/yXa9yhs.png)
## wrap(少用)
把要得tag用div包起來
$(目標).wrap('<div class="foo"></div>')
```htmlembedded=
<img src= >
<img src= >
變成

<div>
  <img src=....>
<div>

<div>
  <img src=....>
<div>

#.wrapAll(更少用，選兩個東西包起來) 
<div>
  <img src=....>
  <img src=....>
<div>
```









