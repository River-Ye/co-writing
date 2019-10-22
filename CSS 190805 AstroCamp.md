---
title: CSS 190805 AstroCamp 
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: HTML, CSS
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}
###### 190805

# http://csscoke.com/webq/
(用手機溝通 不開桌機)
內文低標：16px
標題和內文的字體大小，需考慮使用者年齡族群

“企劃” “設計” “前端” 三方溝通
ex.摺行嗎？行距呢？

# [Emmet 簡碼速查](hhttps://docs.emmet.io/cheat-sheet/)
# Selector
## ==:nth-child(n)==

>第n個『子物件』(順序是重點)
---
![](https://i.imgur.com/QoxEvud.png)
![](https://i.imgur.com/TaolTUd.png)
![](https://i.imgur.com/nSO2W0l.png)

## ==:nth-of-type==

>第n個類型的子物件
:nth-child vs. :nth-of-type
![](https://i.imgur.com/UFmLMw0.png)
![](https://i.imgur.com/QM0lVbR.png)

:nth-of-type(沒有指定Tag的狀態下)
表示所有類型的第2個物件都被選取
但不建議這樣
![](https://i.imgur.com/GEmMKHn.png)



## ==表格 :nth-child==

```html=
    <!-- table.table>thead>tr>th{title}*6^^tbody>tr*20>td{data}*6 -->
  <table class="table">
    <thead>
      <tr>
        <th>title</th>
        <th>title</th>
        <th>title</th>
        <th>title</th>
        <th>title</th>
        <th>title</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>data</td>
        <td>data</td>
        <td>data</td>
        <td>data</td>
        <td>data</td>
        <td>data</td>
      </tr>
      <tr>
        <td>data</td>
        <td>data</td>
        <td>data</td>
        <td>data</td>
        <td>data</td>
        <td>data</td>
      </tr>
      <tr>
        <td>data</td>
        <td>data</td>
        <td>data</td>
        <td>data</td>
        <td>data</td>
        <td>data</td>
      </tr>
      <!-- 以下省略 -->
    </tbody>
  </table>
```

```css=
.table,th,td{
  border: 1px solid #000;
}
th,td{
  padding: 10px 20px;
}
.table tbody tr:nth-child(3n+1){
  background-color: #ccc;
}
```

:nth-child :nth-of-type 在這都一樣
![](https://i.imgur.com/VYYmVdQ.png)
![](https://i.imgur.com/lr8aiZs.png)
也可以設定單獨某行
![](https://i.imgur.com/NhPpBX2.png)

### 美化表格 水！

```css

 border-collapse: collapse
 /*border-collapse: separate */
 
```
![](https://i.imgur.com/orRya0L.png)

## ==其他pseudo-class選取器==

```css=

/*從頭選取*/
:first-child
:first-of-type
:nth-child(an+b)
:nth-of-type(an+b)
---------------
/*從尾選取*/
:last-child
:last-of-type
:nth-last-child(an+b)
:nth-last-of-type(an+b)

:not(不要選到的類型或選取器)
:empty()

```

![](https://i.imgur.com/zBGayVl.png)
![](https://i.imgur.com/hjwRku1.png)

![](https://i.imgur.com/s2cDQUe.png)

![](https://i.imgur.com/vFW6hLg.png)


```html=
<div class="nav">
  <ul>
    <li><a href="#">link</a></li>
    <li><a href="#">link</a></li>
    <li><a href="#">link</a></li>
    <li><a href="#">link</a></li>
    <li><a href="#">link</a></li>
    <li><a href="#">link</a></li>
  </ul>
</div>
```

```css=
*{
  margin: 0;
  padding: 0;
  list-style: none;
}
.nav{
  background-color: #eee;
}
.nav ul{
  display: flex;
  justify-content: center;
}
.nav a{
  display: block;
  padding: 10px 20px;
  text-decoration: none;
  color: #000;
}
.nav li:not(:first-child) a{
  border-left: 3px solid #fa0;
}
```

![](https://i.imgur.com/Re6mQa9.png)
---
>Q.互動（a）與裝飾（li）分離  或是 視覺聚焦（只設定在a上面）

![](https://i.imgur.com/hZkVp0s.png)
![](https://i.imgur.com/WUH1hCE.png)

FB 購物車 數字訊息
重點在 
position:absolute min-width min-height設定
![](https://i.imgur.com/fXtkDwf.png)
![](https://i.imgur.com/vHtLR4W.png)
![](https://i.imgur.com/i3VP0Yw.png)


## ==超連結狀態(:visited :hover :active :focus)==

```css
:link
:visited
:hover
:active
:focus (取得焦點，表單，超連結)
```
>==順序很重要!==
```html=
  <a href="#">Amos</a>
```

```css=
a{
  display: inline-block;
  padding: 20px 30px;
  background-color: #ccc;
  color: #fff;
}
/*順序很重要!*/
a:focus{
  background-color: #00f;
}
a:hover{
  background-color: #000;
}
a:active{
  background-color: #f00;
}
```


原始
![](https://i.imgur.com/7qAf4TO.png)
### :hover
![](https://i.imgur.com/usufqbf.png)
### :active
![](https://i.imgur.com/wHsNyh1.png)
### :focus
![](https://i.imgur.com/f7doAyf.png)


# ==RWD==

設定
![](https://i.imgur.com/OwpVtoF.png)

＠media 裝置 and (條件){
 ...要設定項目}
![](https://i.imgur.com/9k1o9S5.png)
![](https://i.imgur.com/kNG4aaS.png)

![](https://i.imgur.com/xm2ragD.png)
![](https://i.imgur.com/QGRrPmq.jpg)

## RWD CSS設定：
1. 共同設定
1. 手機專用 (max-width: 767px)
1. 平板以上專用（桌機也會套用）(min-width: 768px)

![](https://i.imgur.com/GD7pzFC.png)
![](https://i.imgur.com/uhm0bOB.png)


```html=
<div class="wrap">
    <div class="item">
      <img src="https://fakeimg.pl/300x200/ccc">
      <h3>title</h3>
      <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Quo, facere?</p>
    </div>
    <div class="item">
      <img src="https://fakeimg.pl/300x200/ccc">
      <h3>title</h3>
      <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Quo, facere?</p>
    </div>
    <div class="item">
      <img src="https://fakeimg.pl/300x200/ccc">
      <h3>title</h3>
      <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Quo, facere?</p>
    </div>
  </div>
```

```css=
*{
  margin: 0;
  padding: 0;
  list-style: none;
}
.wrap{
  padding: 0 15px;
}
.item{
  box-sizing: border-box;
  margin-bottom: 30px;
  border: 1px solid #000;
  padding: 10px;
}
.item img{
  width: 100%;
}

/* 手機 */
@media (max-width:767px){
  .item{
    background-color: #ff0;
  }
}
/* 平板以上含桌機 */
@media (min-width:768px){
  .wrap{
    display: flex;
  }
  .item{
    width: 31.666666667%;
    margin: 0.833333333%;
  }
}
/* 平板限定 */
@media (min-width:768px) and (max-width:992px){
  .item{
    background-color: #aaa;
  }
}
```
![](https://i.imgur.com/EnLmuvd.png)

![](https://i.imgur.com/DscMJeG.png)
![](https://i.imgur.com/oqHtXGs.png)
![](https://i.imgur.com/FCyKM4z.png)
![](https://i.imgur.com/zSwAfXe.png)


## ==calc==

```html=
<div class="wrap">
    <div class="item">
      <img src="https://fakeimg.pl/300x200/ccc">
      <h3>title</h3>
      <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Ullam, officia.</p>
    </div>
    <div class="item">
      <img src="https://fakeimg.pl/300x200/ccc">
      <h3>title</h3>
      <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Ullam, officia.</p>
    </div>
    <div class="item">
      <img src="https://fakeimg.pl/300x200/ccc">
      <h3>title</h3>
      <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Ullam, officia.</p>
    </div>
</div>
```

```css=
*{
  margin: 0;
  padding: 0;
  list-style: none;
}
.wrap{
  width: 1200px;
  display: flex;
  flex-flow: row wrap;
  padding: 10px;
  background-color: #fa0;
  margin: 0 auto;
}
.item{
  margin: 0 10px;
  padding: 10px;
  border: 1px solid #000;
  box-sizing: border-box;
  width: calc(100% / 3 - 20px);
  background-color: #aaa;
}
.item img{
  width: 100%;
}
```

![](https://i.imgur.com/KAK62HQ.png)
![](https://i.imgur.com/9vedEHh.jpg)


分12欄切法
![](https://i.imgur.com/iy6ppIF.jpg)
![](https://i.imgur.com/PMP42xO.jpg)

```html=
    <div class="container">
    <div class="col col-3">
      <div class="item">
        <h3>title</h3>
        <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Fugiat obcaecati dolor expedita iure itaque doloribus amet! Vel sint magni repudiandae.</p>
      </div>
    </div>
    <div class="col col-3">
      <div class="item">
        <h3>title</h3>
        <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Fugiat obcaecati dolor expedita iure itaque doloribus amet! Vel sint magni repudiandae.</p>
      </div>
    </div>
    <div class="col col-3">
      <div class="item">
        <h3>title</h3>
        <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Fugiat obcaecati dolor expedita iure itaque doloribus amet! Vel sint magni repudiandae.</p>
      </div>
    </div>
    <div class="col col-3">
      <div class="item">
        <h3>title</h3>
        <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Fugiat obcaecati dolor expedita iure itaque doloribus amet! Vel sint magni repudiandae.</p>
      </div>
    </div>
  </div>
```

```css=
*{
  margin: 0;
  padding: 0;
  list-style: none;
}
.container{
  display: flex;
  flex-flow: row wrap;
}
.col{
  padding: 0 15px;
  box-sizing: border-box;
}
.col-1{ width: 8.333333%; }
.col-2{ width: 16.666666%; }
.col-3{ width: 25%; }
.col-4{ width: 33.333333%; }
.col-5{ width: 41.666666%; }
.col-6{ width: 50%; }
.col-7{ width: 58.333333%; }
.col-8{ width: 66.666666%; }
.col-9{ width: 75%; }
.col-10{ width: 83.333333%; }
.col-11{ width: 91.666666%; }
.col-12{ width: 100%; }

.item{
  background-color: #ccc;
  padding: 10px;
}
```

## ==屬性選擇器==
[屬性 * = 值] 任一”字串”等於
~= 任一”單字”等於
^= 開頭等於
$= 結尾等於
|= 開頭等於

Q: ^= 跟 |= 有何不同? 從W3S看起來都一樣


