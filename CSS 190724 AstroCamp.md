---
title: CSS 190724 AstroCamp 
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: HTML, CSS
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}
###### 20190724   tags: `CSS`
```=HTML
<html>
    <head></head>
    <body>
    </body>
</html>
   
```
```=CSS

h1 {
    font-size: 1.2em;
}
    
```
# ==HW Review==

>釐清資料的"語意"，always on the first
>[name=邱宏毅][color=#00cc00]
>網站程式開發需考量使用者在選擇或使用上矛盾
# 
## ==Q&A==
>Q.<label></label>有 ==分散式== 和 ==包覆式== 的寫法，使用的情境區別？
>>排版哪個比較適合、便利，依照情況使用[name=唐睿聰]
>>>分散式：可以分開，遠端操作，EX:漢堡選單
>>>包覆式：單純的資料呈現？[name=邱宏毅][color=#dd5d6c]
```hmtl=
<label for="amos">label 分離</label>
  <input type="text" id="amos">

  <label>
    label在裡面
    <input type="text">
  </label>
```


# ==Box Model 盒模型==
```html=
  <div class="amos"></div>
  <div class="tang"></div>
```
```css=
.amos{
  width: 500px;
  height: 300px;
  background: #ccf;
  float: left;
}
.tang{
  width: 500px;
  height: 300px;
  background: #cfc;
  float: left;
}
```
![](https://i.imgur.com/nPQS1IU.png) thx><

- setting item

    - width、height
    - padding:資料與框線之間的距離
    - border:（寬度，色彩，樣式）
    - margin:物件與其他物件之間的距離
    - 
可見的空間 = width + padding + border
佔據的空間 = width + padding + border + margin

![](https://i.imgur.com/WcRXl9n.png)

```html=
  <div>Lorem ipsum dolor sit amet consectetur 
adipisicing elit. Minus necessitatibus non nihil labore 
aliquid odio perspiciatis cumque earum ipsam blanditiis 
ex autem sed sint iste libero debitis eius, molestias 
dolores aut corrupti quidem aliquam id! Animi sed 
recusandae voluptatum atque.</div>
```
```css=
*{
  margin: 0;
  padding: 0;
}
div{
  background-color: #ccc;
  width: 400px;
  border: 25px solid #f00;
  padding: 25px;
  margin: 25px;
}

```

>跑版怎麼辦？先檢查Box-model的尺寸

ex. 兩欄
```css=
*{
            margin: 0px;
            padding: 0px;
        }
        .wrap{
            width: 1000px;
        }
        .box-a{
            width: 200px;
            margin-right: 10px;
            height: 300px;
            background-color: red;
            float: left;
        }
        .box-b{
            width: 750px;
            margin-left: 10px;
            height: 300px;
            padding: 0px 15px;
            background-color: greenyellow;
            float: left;
        }
```
![](https://i.imgur.com/AOh8I2M.png)


## box-sizing
>Def:==how the total width and height of an element is calculated.==
>
box-sizing: content-box (預設)
![](https://i.imgur.com/4ohn0tm.png)

box-sizing: padding-box
![](https://i.imgur.com/2l8aUCa.png)


box-sizing: border-box
![](https://i.imgur.com/arAeKpT.png)



### ==Q. border-box如何設定寬度？==
>border-box: 可視寬度 = border + padding + 內容的width
[name=邱宏毅]
>border-box: width設定多少，看到就會是多少 [name=唐睿聰]


## overflow 溢位
```css=
  overflow: scroll;   #強制x、y軸(出現捲軸)
  overflow: hidden;   #超出去的會隱藏
  overflow: visible;  #超出去的會被看見(預設)
  overflow: auto;     #有超出去的會才會有x、y軸
                      (常見於閱讀條款)
```
ex. 圖片overflow
![](https://i.imgur.com/OHAgul2.png)


### 0724 1330 切hw adidas ~14:00
![](https://i.imgur.com/UWgTsDX.png)
![](https://i.imgur.com/rv1Ar1e.jpg)
![](https://i.imgur.com/MddLIiQ.jpg)
![](https://i.imgur.com/1VyOnd2.jpg)
更多:checked介紹
<iframe width="560" height="315" src="https://www.youtube.com/embed/XCyrzYp3aCY" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## ==選取器 +==
```html=
  <div>div</div> 文字
  <hr>
  <div>div</div>
  <span>文字</span>
  <span>文字</span>
```
```css=
  div + span{
    color: red;
  }
```
![](https://i.imgur.com/DVOR19t.png)

## ==Selector==
- simple selector
    - tag
- combinators
    - "+"

CSS reset
Meyer網頁實驗室
https://meyerweb.com/eric/tools/css/reset/

before
![](https://i.imgur.com/k6inNCJ.png)

after
![](https://i.imgur.com/MUMegIn.png)

可以將reset樣式表 事先做好 套用
![](https://i.imgur.com/4Rko1l8.png)

VScode:
Snippet Creator 簡碼產生器


## ==物件的類型==

| -------- | inline   | Block    | inline-block |
| -------- | -------- | -------- | --------     |
| width    | ✖       | ==✔==        |==✔==            |
| height   | ✖       | ==✔==        |==✔==             |
| 預設排列方式 | 排成一列| 自己一列  | 排成一列       |
| Margin左右 | ==✔==      | ==✔==        | ==✔==            |
| Margin上下 | ✖      | ==✔==        | ==✔==            |
| Padding左右 | ==✔== | ==✔==     | ==✔== |
| Padding上下 | ✖ | ==✔==     | ==✔== |


Q.哪些物件是inline?哪些物件是block?
block 物件，凸顯他是一個區塊(Block)
ex. article section main nav h1 div p figure figcation footer
Q.為什麼要將inline > inline-block?

Q.為什麼要將block > inline-block?


# ==float== 
>Def:places an element on the left or right side of its container, 
allowing text and inline elements to wrap around it. 
The element is removed from the normal flow of the page, 
though still remaining a part of the flow (in contrast to absolute positioning).
-- from [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/float)
>


![](https://i.imgur.com/s7uucnM.png)

![](https://i.imgur.com/BzWh7Tj.png)


![](https://i.imgur.com/xR9MR0P.png)

![](https://i.imgur.com/w1EGUS4.png)

## ==clear 清除浮動==
>Def: sets whether an element 
>must be moved below (cleared) 
>floating elements that precede it
>--from [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/clear)
>
```html=
<div class="wrap">
    <div class="item item1">A</div>
    <div class="item item2">B</div>
    <div class="clearfix"></div>
</div>
```

```css=
.wrap{
  width: 1000px;
  padding: 10px;
  background-color: #faa;
}
.item{
  float: left;
}
.item1{
  width: 200px;
  height: 200px;
  background-color: #afa;
}
.item2{
  width: 500px;
  height: 300px;
  background-color: #aaf;
}
.clearfix{
  clear: both;
  border: 1px solid #000; #0px就可以看不到
}

```
![](https://i.imgur.com/IbOhg09.png)

ex.基礎四欄
![](https://i.imgur.com/IpXh9PZ.png)

```htmlmixed=
<div class="wrap">
        <div class="item aa"></div>
        <div class="item bb"></div>
        <div class="item cc"></div>
        <div class="item dd"></div>
        <div class="clearfix"></div>
</div>
```
```css
       *{
            margin: 0px;
            padding: 0px;
            list-style: none;
        }
        .wrap{
            width: 960px;
            background-color:gray;
            margin: auto;
        }
        .item{
            width: 210px;
            height: 300px;
            float:left;
            box-sizing: border-box;
            margin: 0px 15px;
        }
        .aa{
            background-color: red;
        }
        .bb{
            background-color: white;
        }
        .cc{
            background-color: green;
        }
        .dd{
            background-color: blue;
        }
        .clearfix{
            height: 0px;
             clear: left;
        }
```

ex.進階四欄
![](https://i.imgur.com/M84mMsy.png)

```htmlmixed=
    <div class="wrap">
        <div class="item aa">
            <img src="patt.jpg" alt="pat">
            <div class="text">
                <h1>Pat</h1>
                <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Inventore doloremque saepe, amet necessitatibus provident qui laborum molestias?</p>
            </div>
        </div>
        <div class="item bb">
            <img src="patt.jpg" alt="pat">
            <div class="text">
                <h1>Pat</h1>
                <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Inventore doloremque saepe, amet necessitatibus provident qui laborum molestias?</p>
            </div>
        </div>
        <div class="item cc">
            <img src="patt.jpg" alt="pat">
            <div class="text">
                <h1>Pat</h1>
                <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Inventore doloremque saepe, amet necessitatibus provident qui laborum molestias?</p>
            </div>
        </div>
        <div class="item dd">
            <img src="patt.jpg" alt="pat">
            <div class="text">
                <h1>Pat</h1>
                <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Inventore doloremque saepe, amet necessitatibus provident qui laborum molestias?</p>
            </div>
        </div>
        <div class="clearfix"></div>
     </div>
```
```css=
       *{
            margin: 0px;
            padding: 0px;
            list-style: none;
        }
        .wrap{
            width: 960px;
            background-color:goldenrod;
            margin: auto;
        }
        .item{
            width: 210px;
            float:left;
            box-sizing: border-box;
            margin: 15px;
            padding: 10px;
            border: 6px solid white;
            box-sizing: border-box;
            border-radius: 4%;
            text-align: center;
        }
        .item img{
            width: 100%;
            overflow: hidden;
            border-radius: 50%;
        }
        .text{
            padding: 10px;
            font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
        }
        p{
            text-align:left;
            font-size: 12px;
        }
        .aa{
            background-color: plum;
        }
        .bb{
            background-color:lightskyblue;
        }
        .cc{
            background-color:lightgreen;
        }
        .dd{
            background-color:lemonchiffon;
        }
        .clearfix{
            height: 0px;
             clear: left;
        }
```

