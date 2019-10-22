---
title: CSS 190801 AstroCamp 
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: HTML, CSS
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}

## ==多張背景圖==

```html=
<div class="box"></div>
```

```css=
.box{
  width: 700px;
  height: 600px;
  border: 1px solid black;
  margin: auto;
  background-image: url("https://fakeimg.pl/200x200/aaa"),
                    url("https://fakeimg.pl/100x100/fa0");
  background-position: left top, right top;
  background-repeat: no-repeat, repeat-y;
  /* background: 背景色彩 圖片路徑 固定方式 是否重複 位置X 位置Y / 尺寸W 尺寸H;*/
  /* 
  background: url("https://fakeimg.pl/200x200/aaa") no-repeat left top,
              url("https://fakeimg.pl/300x300/ccc") repeat-y right top;
  */
}
```

![](https://i.imgur.com/RxDx7ci.png)

```css
/*cover 塞滿，不惜切掉多餘的部分*/
background-size: cover
/*把圖片整張塞進去，不會填滿多餘的部分*/
background-size: contain
```
cover
![](https://i.imgur.com/8eVskgj.jpg)
contain
![](https://i.imgur.com/6EfQN5B.png)

## ==利用 background+linear-gradient 設計一個隔線==

```html=
<!--000000-->
```
```css=
*{
  margin: 0;
  padding: 0;
  list-style: none;
}
body{
  background-image: linear-gradient(0deg, #aaa 1px, transparent 1px), 
                    linear-gradient(90deg, #aaa 1px, transparent 1px);
  background-size: 30px 30px;
}

```
![](https://i.imgur.com/rVF92RG.png)
![](https://i.imgur.com/0H38Khp.png)

## ==利用 background+linear-gradient 設計血條==
![](https://i.imgur.com/6iZb0ol.jpg)
![](https://i.imgur.com/vImPje5.jpg)


```html=
<div class="bar-block">
    <div class="bar"></div>
</div>
```
```css=
.bar-block{
  width: 1000px;
  height: 30px;
  background-color: #aaa;
  margin: 0 auto;
}
.bar{
  width: 50%;
  height: 100%;
  background-image: linear-gradient(110deg, #ccc 0px,  #ccc 12px,
                                            #111 12px, #111 32px,
                                            #ccc 32px, #ccc 50px );
  background-size: 50px 100%;
  background-repeat: repeat;
}
```

![](https://i.imgur.com/jOehY7v.png)

---

## ==Flex==

```html=
<div class="wrap">
    <div class="item">1</div>
    <div class="item">2</div>
    <div class="item">3</div>
</div>
```

```css=
.wrap{
  width: 960px;
  height: 300xp;
  border: 6px solid #aaa;
  margin: auto;
  display: flex; /*父層 啟用flex*/
  
  /*預設橫向，主軸方向，資料走向
    (row,column)*/
  flex-direction: row;
  /*預設不換列 (no-wrap,wrap)/
    次軸方向(warp-reverse)*/
  flex-wrap: wrap; 
  
  /*主軸的對齊與分佈
    (space-between, space-around,
        space-evenly)*/
  justify-content: space-evenly; 
  /*次軸的對齊與分佈*/
  align-content: ;
  /*次軸 彈性列*/
  align-items:;
    
}
.item{
  width: 1000px; /*收縮 完全不影響*/
  height: 200px;
  margin: 10px;
  border: 10px solid #fa0;
  background-color: #ccc;
}
```

![](https://i.imgur.com/bayYmTC.png)
![](https://i.imgur.com/NMh8JAe.png)
![](https://i.imgur.com/KAoAcum.png)

 ```htmlmixed
 
 .wrap {
      width: 960px;
      height: 700px;
      background-color: #fff;
      border: 1px solid #f00;
      margin: auto;
      /*啟用flex*/
      display: flex; 
      /**/
      /*預設橫向，主軸方向，資料走向*/
      /*flex-direction: column;*/
      /*預設不換列*/
      /*flex-wrap: wrap; */
      /*主軸的對齊與分佈*/
      justify-content: space-between; 
      /*交叉軸的對齊與分佈*/
      align-content: ;
    }
    .item {
      width: 200px;
      height: 200px;
      margin: 10px;
      font-size: 40px;
      background-color: #ccc;
      border: 1px solid #00f;
    }
 ```

### ==flex-direction(預設row)==
主軸 資料走向 與 justify-content有關
column
![](https://i.imgur.com/HJVBeil.png)
### ==flex-wrap==
預設nowrap
![](https://i.imgur.com/5RGTFop.png)
wrap後
![](https://i.imgur.com/cQltl3z.png)

 文字由右至左
 原始
 ![](https://i.imgur.com/9NIpsoH.png)
 row-reverse（不建議）
![](https://i.imgur.com/tFGWIod.png)
div裡設定 dir='rtl'
![](https://i.imgur.com/gqTESTk.png)

### ==justify-content==
有六種
flex-start
flex-end
center
space-between
space-around
space-evenly

space-between
![](https://i.imgur.com/qNmJump.png)
space-around
![](https://i.imgur.com/hJEus1l.png)
space-evenly(業界不愛)
![](https://i.imgur.com/G7vj276.png)

血條（有文字）用space-between
![](https://i.imgur.com/bh5M8aY.png)

用flex做導覽列
![](https://i.imgur.com/F0DZmMC.png)
次軸 (cross axis) 的對齊方式
### ==align-items==
共有 5 種選項
flex-start
flex-end
stretch
baseline
center


## ==子層==
### ==order==
![](https://i.imgur.com/8K6jrsu.png)

### ==flex-grow==
![](https://i.imgur.com/M15bHWm.png)

### ==flex-shrink==
超出了
![](https://i.imgur.com/WDZytxk.png)
設定flex-shrink
![](https://i.imgur.com/McuHbNJ.png)

>*flex-grow flex-shrink 不會同時作用
>因為物件不會同時超出並有剩餘空間可以分配



### flex-basic:物件==主軸的長度==
before
![](https://i.imgur.com/XgbSBFM.png)
after
![](https://i.imgur.com/5Xna0fZ.png)
也可以運用column
![](https://i.imgur.com/2P3pR9e.png)


![](https://i.imgur.com/d6kpMgW.png)
![](https://i.imgur.com/nJyafHI.png)


# ==！FLEX排版大整理！ 一定要看～！！== 
>排版大整理：https://codepen.io/BeastRush/pen/WLjPJx
>青蛙學flex：https://flexboxfroggy.com/#zh-tw
>六角海盜學flex：https://hexschool.github.io/flexbox-pirate/index.html#/way
>排版入門：https://codepen.io/BeastRush/pen/mamERr
[name=唐睿聰]











