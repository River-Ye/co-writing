---
title: CSS 190729 AstroCamp 
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: HTML, CSS
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}
###### 190729
# ==CSS==

10:32 display-inblock 
border-top 解決高度問題
```=css

a { display: inline-block;
    padding: 10px 20px;
    vertical-align: top;
    border-bottom: 6px solid transparent;
    
}
/*hover後產生border，有向下推擠突出的效果*/
a:hover {
    border-bottom: 6px solid #f00;
    
}

---

a {
    display: inline-block;
    height: 40px;
    padding: 10px 20px;
    vertical-align: top;
}

/*hover後產生border，有向下推擠突出的效果*/
a:hover {
    border-bottom: 6px solid #f00;
    height: 34px;
}

```

有問題版面
![](https://i.imgur.com/hGRjEvm.png)
解決一 方便快速
![](https://i.imgur.com/7RcdfD6.png)
解決二 計算 多條透明邊界
![](https://i.imgur.com/G1Wugw1.png)


#google開發者工具
user agent 瀏覽器開發者css
預設內文 p 大小是16px


10:45
## ==text-align 和 verticle-align 的不同==
>text-align < 設定在inline物件的父層 對子物件也有用
>verticle-align <  對象:inline物件本身 效果：調整垂直置中

float(block)

## ==首字放大==
p:first-letter{
  font-size: 100px;
  float: left;
}
![](https://i.imgur.com/HEufq5s.png)

## ==偽元素 ::after ::before==
## ==偽類 :after :before==

>優先權等同於class
>瀏覽器都會轉成兩個冒號 結果一樣
>無中生有
>作用於==設定對象內容的前後==
>要和   ==content = "";== 搭配使用
>生成內容預設為==inline==

```html=
<p>變得航空能夠適用於大學，填寫控制奇怪成功律師，創作委員會來看關鍵少
女本地提問宜蘭，便宜貿易比賽後面盯着報名地圖環節一陣怎麼辦最新，電子
郵件，清楚對了針對處罰生產用途它是出生簡單顧客在我婦女，聯盟觀察新年
您的規範就會打造無派主要可以說行動，婦女例如，程度道。</p>
```
```css=
p{
  background-color: #ccc;
}
p:before{                  #寫單冒號ie8會支援 
  content:"i am before";   #沒有content就沒有before,after
  color: red;
  display: block;          #原本是inline特性 會排在一起
}
p:after{
  content: "hi iam after";
  color: blue;
  display: block;
}

```
![](https://i.imgur.com/QXya4m1.png)
![](https://i.imgur.com/IKpdcHa.png)

## ==::after取代clearfix== 
```html=
<div class="amos">
    <div class="item"></div>
    <div class="item"></div>
</div>
```
```css=
.amos{
  width: 600px;
  background-color: #ccc;
  padding: 10px;
}
.item{
  width: 280px;

  margin: 10px;
  background-color: #fa0; 
  height: 200px;
  float: left;
}
.amos::after{
  content: "";
  display: block;
  clear: both;
}
```

![](https://i.imgur.com/5Yrrr2a.png)
![](https://i.imgur.com/payXdT8.png)
高度歸零 content無文字
![](https://i.imgur.com/IG08fbP.png)

clearfix  業界常用做法
![](https://i.imgur.com/wc2x23W.jpg)





## ==css font-weight==
>normal：也就是預設字體厚度，其實可以不用特別寫出來。
bold：常用的粗體字。
bolder：比粗體更粗一點。
lighter：比一般字體更細。
100~900：數字越大越厚，數字小於 500 似乎效果不明顯。

各種應用

用border出問題的
![](https://i.imgur.com/JdIP5jT.png)
用after修正後
![](https://i.imgur.com/l8Mq2Lg.png)

或是 標題前面的線
![](https://i.imgur.com/cwwF7X0.png)

# ==排版重點：==
- basic
    - reset
    - box-model

    - block & inline & inline-block
    - float-clear
    - position
- advance
    - flex
    - grid
- RWD


## ==Position==

static
==fixed== #vscode快捷鍵 pof
==absolute== #vscode快捷鍵 poa
==relative== #vscode快捷鍵 por
sticky #IE不支援![](https://i.imgur.com/adCGyre.png)
---
### ==Static==

- 所有定位效果無效化。

### ==Fixed==
- 當物件設定為fixed時，該物件會從資料流當中抽離。
- 當設定為fixed 的物件"尚未設定TRBL的屬性時，該物件會停留在原始物件留所在的位置"，並永遠固定且不隨視窗捲軸捲動。
- 設定TRBL的屬性後，會依照TRBL與視窗邊緣做定位。
- 設定衝突時，同時設定TB和LR時，會依據資料流的流向


設定在top
![](https://i.imgur.com/vsV4rim.png)
也可以在bottom left right
![](https://i.imgur.com/evSGl4D.png)

top/bottom 0 + margin:auto 可以Y軸置中
left/right 0 + margin:auto 可以X軸置中
TBRL 0 + margin:auto 可以置中中央
![](https://i.imgur.com/mHc16Pj.jpg)

### ==Absolute==
- 當設定為absolute 的物件"尚未設定TRBL的屬性時，該物件會停留在原始物件留所在的位置"，自己獨立一個圖層。
- 設定尚未TRBL的屬性時，該物件會停留在物件流所在位置，但當是霜捲動時，會隨視窗捲軸卷走。
- 設定TRBL的屬性後，該物件將會開始尋找具備定位設定的父層。
- 當父層具備有定位設定時，poa的物件將會依據父層的空間做定位。
- 有效的父層設定:fixed/absolute/relatvie/sticky 
- 當父層沒有訂位設定時，物件將繼續往更上層尋找具備定位設定的父父層。
- 當所有父層皆無設定時，最終將會定位於視窗。
- 定位於視窗的poa物件，僅定位一次，捲動時將會隨著卷軸被捲走。

#### 當設定為absolute 的物件"尚未設定TRBL的屬性時，該物件會停留在原始物件留所在的位置"，自己獨立一個圖層。
![](https://i.imgur.com/G9q1U53.jpg)

#### 設定尚未TRBL的屬性時，該物件會停留在物件流所在位置，但當視窗捲動時，會隨視窗捲軸卷走。
![](https://i.imgur.com/lIK6eOh.jpg)


#### 設定TRBL的屬性後，該物件將會開始尋找具備定位設定的父層。
![](https://i.imgur.com/St7sA7k.png)


#### 當父層具備有定位設定時，poa的物件將會依據父層的空間做定位。
#### 有效的父層設定:fixed/absolute/relatvie/sticky 
![](https://i.imgur.com/nQvvIZo.png)

#### 當父層沒有訂位設定時，物件將繼續往更上層尋找具備定位設定的父父層。
#### 當所有父層皆無設定時，最終將會定位於視窗。
![](https://i.imgur.com/pTrDPAn.png)

#### 定位於視窗的poa物件，僅定位一次，捲動時將會隨著卷軸被捲走。
![](https://i.imgur.com/hGp9Xxz.png)
![](https://i.imgur.com/ww2JCUg.png)
![](https://i.imgur.com/J7IngZn.png)

### ==Relative==

- 當物件設定relative後，維持在原資料流的位置上。
-  當設定relative的物件設定TRLB後，會依照原本的位置(自身資料排列位置)去做偏移顯示。
-  當relative物件，同時設定TB or RL ，物件將會忽略R&B的設定。

#### 當物件設定relative後，維持在原資料流的位置上。
![](https://i.imgur.com/wMrjNOW.png)

#### 當設定relative的物件設定TRLB後，會依照原本的位置(自身資料排列位置)去做偏移顯示。
![](https://i.imgur.com/xPbmUwm.png)

#### 當relative物件，同時設定TB or RL ，物件將會忽略R&B的設定。


也可以相互對調
（注意 寬高 和 margin...）
![](https://i.imgur.com/ib1qHmD.png)

可以用來調：before位置
![](https://i.imgur.com/Sl3E9ql.png)
調整後
![](https://i.imgur.com/WIfOxbT.png)

卡片式網頁的貼標兩種作法
![](https://i.imgur.com/479aVGo.jpg)
![](https://i.imgur.com/dc3TC2U.jpg)

16:45 hot

## ==做箭頭貼標方法(三角形)==
![](https://i.imgur.com/d9dEurE.jpg)
![](https://i.imgur.com/q68aeAa.png)
![](https://i.imgur.com/wLrLjPl.png)
![](https://i.imgur.com/lc8eAE2.png)






