---
title: CSS 190725 AstroCamp 
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags:  HTML, CSS
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}


今天頭腦有點鈍orz ouououoou 
Me too!! 
早喔

資料的關係性、流向
閱讀的習慣

class命名方式
CSS不是你想像的那麼簡單
float 寬度計算

同一html 做出不同方塊組合
```htmlmixed=
 <div class="wrap">
        <item class="item item1">1</item>
        <item class="item item2">2</item>
        <item class="item item3">3</item>
        <item class="item item4">4</item>
        <item class="item item5">5</item>
        <div class="clearfix"></div>
    </div>
```
![](https://i.imgur.com/1ZtwoyC.png)
```css
<style>
        *{
            margin: 0px;
            padding: 0px;
            list-style: none;
        }
        .clearfix{
            height: 0px;
             clear: both;
             width: 100%;
        }
        .wrap{
            width: 960px;
            background-color:peru;
            margin: auto;
        }
        .item{
            background-color: lightgreen;
            margin: 10px;
            font-size: 60px;
            float: left;
        }
        .item1{
            width: 460px;
            height: 460px;
        }
        .item2{
            width: 220px;
            height: 220px;
        }
        .item3{
            width: 220px;
            height: 220px;
        }
        .item4{
            width: 220px;
            height: 220px;
        }
        .item5{
            width: 220px;
            height: 220px;
        }
    </style>
```

![](https://i.imgur.com/uTnUeL4.png)

```css
*{
            margin: 0px;
            padding: 0px;
            list-style: none;
        }
        .clearfix{
            height: 0px;
             clear: both;
             width: 100%;
        }
        .wrap{
            width: 960px;
            background-color:peru;
            margin: auto;
        }
        .item{
            background-color: lightgreen;
            margin: 10px;
            font-size: 60px;
            float: left;
        }
        .item1{
            width: 460px;
            height: 460px;
        }
        .item2{
            width: 300px;
            height: 300px;
        }
        .item3{
            width: 120px;
            height: 120px;
        }
        .item4{
            width: 120px;
            height: 120px;
        }
        .item5{
            width: 220px;
            height: 200px;
        }
```


## ==inline-block==

>display:inline-block
>> float
>> inline-block
>> flex
>>>同版型試著用三種方式來切
>>>

![](https://i.imgur.com/9TWd5gA.png)
![](https://i.imgur.com/e0aYSED.png)


# ==版面跑掉，先檢查width,size does matter!!==
14:25 Amos 切版

## ==vertical-align==
字母有頂線、中線、基線、底線
vertical-align: baseline(default)
是用來設定基準線


原始圖片問題
![](https://i.imgur.com/luBTpD9.png)


# ==CSS權重==
1. 相同的selector 後者 > 前者
1. class > Tag
1. class 多 > class 少
1. ID > class

>==ID 要謹慎使用，因為權重太高，無法複寫。== 
>==CSS設定的對象越精準，權重越高== [name=邱宏毅][color=#2c3bdd]
> 



```=css
.amos.vivian {
同時擁有兩個class
}
```

## ==inline-style== (build by program)
## ==!important== (Evil Evil Evil)

>!important(千萬不能用plz) > inline-style > ID > class > Tag [name=Amos] [color=#aaf]
>
# ==16進位法的色彩==

## rgb/rgba
rgb (r,g,b) 
rgba (r,g,b,a) 
 
r,g,b =0..255
a = 0.0..1.0

## hsla/hsl
hsla(色相,飽和度(鮮豔度),亮度(光的含量),透明度)
h = 0..360 (0 = red, 120 = blue, 240 = blue)
s = 0%..100%(標準50%，往下扣，越接近灰階)
l = 0%..100%(標準50%，往上加越白)

## [16進位法的色彩(HEX COLOR CODE)](https://5xruby.tw/posts/understand-hex-color-codes/?utm_source=newsletter&utm_medium=email&utm_campaign=2019w18_tech_post)

```css
div {
    background-color: #c84;
}
```
1.選出主色 (三選二 或 三選一)
2.亮度 (簡碼的值==和==的大小，決定亮度的高低)
3.飽和度（簡碼的值==差==越小，越接近灰階）
