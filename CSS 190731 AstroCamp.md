---
title: CSS 190731 AstroCamp 
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: HTML, CSS
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}

==改善方向==
縮排
塞～四層
分段＝＝＝＝＝＝＝＝＝＝＝＝

用背景色去確認設定
資料是死的或活的（）
資料的數量會影響


用層的觀念去處理
cart float r
nav 文字置中

圖片float + 文字設定，padding-left : 圖片w

pic float text float

one 
  section>h>p
  
  float + clear 配合
  
  va-c 處理圖片
  
 inline block 
 
 flex 設定再父層（block）影響子層
 
 ==分析技巧==
 
 用滑鼠指標或是A4紙去確認對齊
 
 滿版:    div(不設定寬度)
 
 非滿版:    div> container
 
 一開始就將標題分類（整理成組件）
 
 區分各個區塊
  
  
  重複設定的項目，額外賦予相同的class name
    可以簡化設定
  <div class= "section about">設定padding
      <div class="container" > 
         存放資料
      </div>        
  
  </div>
  
  class命名不建議使用編號
  
  padding 
  
  container裡面再用margin 
  
  ==Codereview==
  
==  一般來說不會針對資料內容去設定h==

Q.什麼時候會去設定高度？
選單，獲是資料固定的時候才做
  
```css
p {
 columns:2; #直接
 colume-gap:;
 colume-rule:;
 colume-width:
 colume-count:
}

```
<p>    </p> <btn>    </btn>

對<p> 設定mr 避免btn 被蓋住

避免使用位置去命名(同一組的時候才用編號)

圖片本身有寬度，保險的做法：在另外設定一個寬度給圖片
---

# ==CSS==

## 變數

![](https://i.imgur.com/4FCfygE.png)

## 漸層色
```css
background-image: 
linear-gradient(270deg,red 0%, yellow 100%);
 
> 角度（方向），色標（色彩 位置），色標（色彩 位置）
```
![](https://i.imgur.com/FgR0jUB.jpg)

互動式（圖片上有漸層）

![](https://i.imgur.com/1icICbh.jpg)

互動式 （圖片上有文字有漸層 還可以加作動畫）

```htmlmixed=
<div class="pic">
     <img src="https://picsum.photos/id/1000/500/500" alt=“圖片說明”>
     <div class="text">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Fuga, quas.</div> 
  </div>
```
```css
/*解法一：用display設定，.text 設定為display:none;時
(.text 無法被選取)，所以用.pic:hover .text 來設定 display:block; */
.pic {
      width: 500px;
      position: relative;
      
    }

    .pic img {
      vertical-align: top;

    }

       /*before 是屬於div .pic的，所以是pic:hover*/
       
       /*解法一*/
    .text {
      content: '';
      position: absolute;
      width: 100%;
      height: 100%;
      top: 0;
      color: #fff;

      display: none;
      
      background-image: 
        linear-gradient(45deg,#000 0%,rgba(0,0,0,0) 100%);
    }
 
    .pic:hover .text {
      display: block;
```
```css


/*解法二：用opacity設定（透明可以被選中），:hover設定在.text上*/
.text {
      content: '';
      position: absolute;
      width: 100%;
      height: 100%;
      top: 0;
      color: #fff;
      opacity: 0;

      /*display: none;*/
      
      background-image: linear-gradient(
        45deg,#000 0%,rgba(0,0,0,0) 100%);
    }
    /*before 是屬於div .pic的，所以是pic:hover*/
    .text:hover {
      /*display: block;*/
      opacity: 1;
    }
```
![](https://i.imgur.com/z7Hj03b.jpg)

##  其他Selector
```css
+ 
~ 
> 
```
h1 + p
![](https://i.imgur.com/qrkXSrm.png)
h2 + p
![](https://i.imgur.com/YkWD4VE.png)
h1 ~ p
![](https://i.imgur.com/KLBxX7t.png)
h1 ~ p 有div的p
![](https://i.imgur.com/66zwAvW.png)

![](https://i.imgur.com/q4646Qj.jpg)
![](https://i.imgur.com/odLsgDm.jpg)

```css
ul li:nth-child(-n+5) {
      background-color: #f00;
    }
    
    an+b    (A個裡面，第B個)
    an-b
    an
    a
    even
    odd
    -n+b
    n+b
    
    /*n 是個陣列，從0開始*/

```
odd
![](https://i.imgur.com/qqwrpZp.png)
even
![](https://i.imgur.com/RHGEKb7.png)
an+b
![](https://i.imgur.com/NEWe3oL.png)
a
![](https://i.imgur.com/0zArjEV.png)
-n+b
![](https://i.imgur.com/rJ4FMVt.png)
n+b
![](https://i.imgur.com/sJGCKif.png)







