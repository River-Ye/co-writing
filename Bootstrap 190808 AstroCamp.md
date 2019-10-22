---
title: Bootstrap 190808 AstroCamp
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: HTML, CSS, RWD, Bootstrap
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}
==0808時間軸==
10:57 transform
11:43 位移
11:58 boostrap card
12:21 flex:_ _ _;
13:40 Image overlays (文字蓋在圖片上)
14:57 d-md-none , d-lg-none
15:45 SCSS
16:48 控制按鈕 收合
```css
/*自動分配欄寬*/
flex-basic:0;
flex-grow:1;
```
超連結<a></a>同時包覆:圖片，標題，段落，按鈕，合理？
只要你想包什麼都KE 連div也可以ouo
SEO考量 : 以 a tag 包整個card VS 每個card部件包一個 a tag，SEO 可能將後者判斷為賺連結數的作弊行為。

```css
    

```

==tranform==
transform: 縮放 傾斜 旋轉 位移
縮放:scale (1, 2) ，變大且不影響周圍物件。1為x軸放大100%，2為軸放大200%   ->較少用
![](https://i.imgur.com/XWmneQh.png)
傾斜：skew (20deg, 0deg) ->較少用
旋轉：rotate(45deg) 大拇指指向你要的軸向（x,y,z軸），四指彎曲的方向為正。 
![](https://i.imgur.com/1WwWSpm.jpg)

位移：translate(-50%, -50%)  ->垂直置中技巧之一
![](https://i.imgur.com/Gu1ipO3.png)

transform 的設定順序不同，對結果會有不同的影響（可能不是你想要的結果）
用matrix()設定，會比較精準

>TFTL的垂直置中方法：
>https://codepen.io/bad_printer/pen/GXRJYZ

## 鐵人賽一定要看的垂直置中技巧:
https://ithelp.ithome.com.tw/users/20112550/ironman/2092?page=1
Day1 *Day3(必學) Day4 Day5 Day6 
Day7 Day9
Day13 Day14(行有餘力)

# ==Boostrap==
## ==Boostrap card==
這邊複製貼上
https://getbootstrap.com/docs/4.3/components/card/

![](https://i.imgur.com/qQVIvLz.png)

![](https://i.imgur.com/eZJXkP8.png)


![](https://i.imgur.com/C9x5C81.png)

flex: flex-grow flex-shrink flex-basis ;
https://ithelp.ithome.com.tw/articles/10208741
==如何客製化調整Bootstrap的預設格式？==
自定義class處理：

```css=

  .card-my-set img{
    width: calc(100% - 1.25rem * 2);
    margin: 1.25rem auto;
  }
  .card-my-set2 img{
    padding: 1.25rem 1.25rem 0;

  }

```
```html=
<div class="card card-my-set">
    <img src="https://picsum.photos/id/30/300/200" alt=“圖片說明” class="card-img-top"> 
    <div class="card-body">
      ...
    </div>
</div>

<div class="card card-my-set2">
    <img src="https://picsum.photos/id/30/300/200" alt=“圖片說明” class="card-img-top">
    <div class="card-body">
      ...
    </div>
</div>

```
調整寬度的class:
w-25
w-50
w-75
w-100
調整對齊方式的class
text-center
text-right

card-group

card-deck

CSS專家密技：Time Code 29:00

<iframe width="560" height="315" src="https://www.youtube.com/embed/m4zQuURWd7Q?start=1741" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

![](https://i.imgur.com/9KPRdHX.png)
![](https://i.imgur.com/GOzHUd8.jpg)
youtube找iframe：分享->嵌入

![](https://i.imgur.com/lnxelG3.png)
當中的sm/lg 不是只在某個裝置下 而是他的大小縮放 

==scss==
![](https://i.imgur.com/MDpQHkK.png)
scss去修正原始的boostrap.scss(下方watch scss要按)
例如：
可改primary的顏色，no-gutter的30px，自己客製成自己要的boostrap後，產生新的boostrap.css，之後要用再html裡面link過來就好。

自己寫scss(有變數可使用)
![](https://i.imgur.com/ukzfrer.png)

# ==補充scss==
>其中有三種 編譯器 分別是scss,sass,less
>如果有興趣的可以看這篇：
>https://blog.kdchang.cc/2016/10/11/sass-scss-tutorial-introduction/

>ALEX教學SCSS
>https://www.youtube.com/watch?v=mzuKtTuimEE


@mixin and @include

==控制按鈕、收合==
![](https://i.imgur.com/PEDDS1U.png)
![](https://i.imgur.com/MLzJtkv.png)
