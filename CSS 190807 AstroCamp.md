---
title: CSS 190807 AstroCamp 
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: HTML, CSS, RWD, Bootstrap
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}
###### 190807 
## Time Code
0807影片時間條
10:36 手機選單
10:55 手機選單demo
11:55 :checked
13:45 ios android 手機測試
14:00 boostrap
14:25 	隔線系統在boostrap上應用
14:40 為何要pading-15px
15:18 排版demo 用隔線系統
15:36 max-width
15:45 調整順序order
16:32 nav導覽列
16:39 nav導覽列排序

---
漢堡的寫法
選單的控制：
1.CSS
2.JS

實例：台灣虎航網站
手機
![](https://i.imgur.com/n53Xy4K.jpg)

平板(直)
![](https://i.imgur.com/EagPIxT.jpg)

桌機&平板(橫)
![](https://i.imgur.com/4OitA1R.jpg)



>挑戰: 做出選單 logo在中間 超連結在左右的排版
logo外層做絕對定位 超連結其中一個設定margin left剛好等於logo大小


# ==漢堡的寫法：==
拆解：共同與差異(手機和桌機)

HTML內容
![](https://i.imgur.com/p7zMBs1.jpg)
![](https://i.imgur.com/EFLnxme.jpg)

1.step1 .main-nav
![](https://i.imgur.com/QLNE3Sx.jpg)

2.step2 .main-header .logo
![](https://i.imgur.com/eGwWaDR.png)

桌機部分處理
(logo一排 導覽列一排)
1.讓桌機版看不到漢堡
![](https://i.imgur.com/9mgC6Go.png)
2.logo要居中的話
![](https://i.imgur.com/Q53vBH1.png)
3.nav就不需要絕對定位
![](https://i.imgur.com/FypVruU.png)

桌機部分
(若要logo一排 導覽列並排)
讓main-header去flex
![](https://i.imgur.com/BARRHs9.jpg)

==如何讓漢堡能控制==
## ==控制:==
選取器 :checked 的用法
可以利用label遠端遙控的特性，選到label讓checkbox打勾，再用checked讓後面的div產生變化
![](https://i.imgur.com/JdeRfbY.png)

運用這原理回到原來導覽列題目
![](https://i.imgur.com/33OaxrY.jpg)
![](https://i.imgur.com/Hmmzbvl.jpg)
![](https://i.imgur.com/fAsk5lx.jpg)
此時手機模式沒問題，但桌機模式沒有漢堡無法讓導覽列出現
要去思考不同模式的共用不共用狀態
only手機：
漢堡 讓導覽列display: none
![](https://i.imgur.com/DuEWc1R.png)
![](https://i.imgur.com/unlmh9Q.png)

接著處理checkbox藏起來
如果用display:none 會有明明不存在卻使用功能的矛盾
因此amos習慣用絕對定位讓他到視窗外面
![](https://i.imgur.com/ta8TZ11.png)

接著手機版fix 桌機版不要
![](https://i.imgur.com/6kzMB0B.png)
![](https://i.imgur.com/qiHEP3j.png)

接著選單要有動畫效果
![](https://i.imgur.com/zfmeJFZ.jpg)
![](https://i.imgur.com/g8LsZ1m.jpg)
![](https://i.imgur.com/uv35FXy.png)


===老師上課的code

範例1
```html=
    <div class="main-header">
        <div class="logo">
          <a href="#">
            <img src="https://picsum.photos/100/50?random=10" alt="圖片說明">
          </a>
          <div class="menu-switch">
            切換選單
          </div>
        </div>
        <nav class="main-nav">
          <a href="#">link</a>
          <a href="#">link</a>
          <a href="#">link</a>
          <a href="#">link</a>
          <a href="#">link</a>
          <a href="#">link</a>
        </nav>
      </div>
    
    
      <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Voluptatibus odio, blanditiis quaerat quam autem laborum dolore sed doloribus. Nulla aperiam voluptate sit, incidunt, temporibus nostrum dolores nemo quos distinctio magnam!</p>
      <p>Possimus non corporis consectetur sed consequatur ipsa eaque eveniet quae dignissimos minus laborum praesentium porro sit culpa quaerat sint recusandae, tenetur voluptates, incidunt modi facilis. Quasi excepturi nobis sint quis.</p>
      <p>Perferendis maiores error accusamus, voluptates, debitis excepturi soluta aliquid nesciunt recusandae omnis perspiciatis explicabo, ut quibusdam amet eos in neque natus quidem ad iusto officiis. Porro est asperiores sunt ipsa!</p>
```
```css=
    *{
	margin: 0;
	padding: 0;
	list-style: none;
}
.main-header{}
.main-header .logo{
	display: flex;
	flex-flow: row nowrap;
	justify-content: space-between;
	align-items: center;
	padding: 6px 15px;
	background-color: #ccc;
}
@media screen and (min-width: 768px){
	.main-header .logo{
		justify-content: center;
	}
}
.logo img{
	vertical-align: middle;
}
.logo .menu-switch{
	width: 30px;
	height: 30px;
	background-color: #666;
	font-size: 0;
}
@media screen and (min-width: 768px){
	.logo .menu-switch{
		display: none;
	}
}
.main-nav{
	display: flex;
	flex-flow: column;
	position: absolute;
	width: 100%;
	background-color: rgba(0,0,0,.7);
}
@media screen and (min-width:768px){
	.main-nav{
		position: relative;
		margin-bottom: 20px;
		flex-flow: row nowrap;
		background-color: #CD0803;
	}
}
.main-nav a{
	width: 100%;
	box-sizing: border-box;
	border-bottom: 1px solid #f00;
	padding: 10px;
	text-align: center;
}
```
範例2
```html=
    <div class="main-header">
        <div class="logo">
          <a href="#">
            <img src="https://picsum.photos/100/50?random=10" alt="圖片說明">
          </a>
          <div class="menu-switch">
            切換選單
          </div>
        </div>
        <nav class="main-nav">
          <a href="#">link</a>
          <a href="#">link</a>
          <a href="#">link</a>
          <a href="#">link</a>
          <a href="#">link</a>
          <a href="#">link</a>
        </nav>
      </div>
    
    
      <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Voluptatibus odio, blanditiis quaerat quam autem laborum dolore sed doloribus. Nulla aperiam voluptate sit, incidunt, temporibus nostrum dolores nemo quos distinctio magnam!</p>
      <p>Possimus non corporis consectetur sed consequatur ipsa eaque eveniet quae dignissimos minus laborum praesentium porro sit culpa quaerat sint recusandae, tenetur voluptates, incidunt modi facilis. Quasi excepturi nobis sint quis.</p>
      <p>Perferendis maiores error accusamus, voluptates, debitis excepturi soluta aliquid nesciunt recusandae omnis perspiciatis explicabo, ut quibusdam amet eos in neque natus quidem ad iusto officiis. Porro est asperiores sunt ipsa!</p>
```
```css=
    *{
	margin: 0;
	padding: 0;
	list-style: none;
}
@media screen and (min-width:768px){
	.main-header{
		display: flex;
		flex-flow: row nowrap;
		align-items: center;
	}
}
.main-header .logo{
	display: flex;
	flex-flow: row nowrap;
	justify-content: space-between;
	align-items: center;
	padding: 6px 15px;
	background-color: #ccc;
}
@media screen and (min-width: 768px){
	.main-header .logo{
		justify-content: center;
	}
}
.logo img{
	vertical-align: middle;
}
.logo .menu-switch{
	width: 30px;
	height: 30px;
	background-color: #666;
	font-size: 0;
}
@media screen and (min-width: 768px){
	.logo .menu-switch{
		display: none;
	}
}
.main-nav{
	display: flex;
	flex-flow: column;
	position: absolute;
	width: 100%;
	background-color: rgba(0,0,0,.7);
}
@media screen and (min-width:768px){
	.main-nav{
		position: relative;
		/*margin-bottom: 20px;*/
		flex-flow: row nowrap;
		background-color: #CD0803;
	}
}
.main-nav a{
	width: 100%;
	box-sizing: border-box;
	border-bottom: 1px solid #f00;
	padding: 10px;
	text-align: center;
}
```
範例3
```html=
    <div class="main-header">
      <div class="logo">
        <a href="#">
          <img src="https://picsum.photos/100/50?random=10" alt="圖片說明">
        </a>
        <label class="menu-switch" for="menu-control">
          切換選單
        </label>
      </div>
      <nav class="main-nav">
        <a href="#">link</a>
        <a href="#">link</a>
        <a href="#">link</a>
        <a href="#">link</a>
        <a href="#">link</a>
        <a href="#">link</a>
      </nav>
    </div>
  
  
    <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Voluptatibus odio, blanditiis quaerat quam autem laborum dolore sed doloribus. Nulla aperiam voluptate sit, incidunt, temporibus nostrum dolores nemo quos distinctio magnam!</p>
    <p>Possimus non corporis consectetur sed consequatur ipsa eaque eveniet quae dignissimos minus laborum praesentium porro sit culpa quaerat sint recusandae, tenetur voluptates, incidunt modi facilis. Quasi excepturi nobis sint quis.</p>
    <p>Perferendis maiores error accusamus, voluptates, debitis excepturi soluta aliquid nesciunt recusandae omnis perspiciatis explicabo, ut quibusdam amet eos in neque natus quidem ad iusto officiis. Porro est asperiores sunt ipsa!</p>
```
```css=
    *{
	margin: 0;
	padding: 0;
	list-style: none;
}
@media screen and (max-width:767px){
	body{
		padding-top: 80px;
	}
	.main-header{
		position: fixed;
		width: 100%;
		top: 0;
	}
}
@media screen and (min-width:768px){
	.main-header{
		display: flex;
		flex-flow: row nowrap;
		align-items: center;
	}
}
.main-header .logo{
	display: flex;
	flex-flow: row nowrap;
	justify-content: space-between;
	align-items: center;
	padding: 6px 15px;
	background-color: #ccc;
}
@media screen and (min-width: 768px){
	.main-header .logo{
		justify-content: center;
	}
}
.logo img{
	vertical-align: middle;
}
.logo .menu-switch{
	width: 30px;
	height: 30px;
	background-color: #666;
	font-size: 0;
}
@media screen and (min-width: 768px){
	.logo .menu-switch{
		display: none;
	}
}
.main-nav{
	flex-flow: column;
	position: absolute;
	width: 100%;
	background-color: rgba(0,0,0,.7);
}
#menu-control{
	position: absolute;
	opacity: 0;
}
@media screen and (max-width:767px){
	.main-nav{
		display: none;
	}
	#menu-control:checked ~ .main-header .main-nav{
		display: flex;
	}
}

@media screen and (min-width:768px){
	.main-nav{
		display: flex;
		position: relative;
		/*margin-bottom: 20px;*/
		flex-flow: row nowrap;
		background-color: #CD0803;
	}
}
.main-nav a{
	width: 100%;
	box-sizing: border-box;
	border-bottom: 1px solid #f00;
	padding: 10px;
	text-align: center;
}
```
範例4
```html=
  	<input type="checkbox" name="" id="menu-control">

	<div class="main-header">
		<div class="logo">
			<a href="#">
				<img src="https://picsum.photos/100/50?random=10" alt="圖片說明">
			</a>
			<label class="menu-switch" for="menu-control">
				切換選單
			</label>
		</div>
		<nav class="main-nav">
			<a href="#">link</a>
			<a href="#">link</a>
			<a href="#">link</a>
			<a href="#">link</a>
			<a href="#">link</a>
			<a href="#">link</a>
		</nav>
	</div>


	<p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Voluptatibus odio, blanditiis quaerat quam autem laborum dolore sed doloribus. Nulla aperiam voluptate sit, incidunt, temporibus nostrum dolores nemo quos distinctio magnam!</p>
	<p>Possimus non corporis consectetur sed consequatur ipsa eaque eveniet quae dignissimos minus laborum praesentium porro sit culpa quaerat sint recusandae, tenetur voluptates, incidunt modi facilis. Quasi excepturi nobis sint quis.</p>
	<p>Perferendis maiores error accusamus, voluptates, debitis excepturi soluta aliquid nesciunt recusandae omnis perspiciatis explicabo, ut quibusdam amet eos in neque natus quidem ad iusto officiis. Porro est asperiores sunt ipsa!</p>
```
```css=
    *{
	margin: 0;
	padding: 0;
	list-style: none;
}
@media screen and (max-width:767px){
	body{
		padding-top: 80px;
	}
	.main-header{
		position: fixed;
		width: 100%;
		top: 0;
	}
}
@media screen and (min-width:768px){
	.main-header{
		display: flex;
		flex-flow: row nowrap;
		align-items: center;
	}
}
.main-header .logo{
	display: flex;
	flex-flow: row nowrap;
	justify-content: space-between;
	align-items: center;
	padding: 6px 15px;
	background-color: #ccc;
	position: relative;
	z-index: 1;
    /* 會用relative原因：第59行的main-nav已設absolute，故會直接浮起來
     * 定位在上面，為了使它可以抓回原本那層的位置（跟main-nav同層)，故用relative
     * 定位後，用z-index:1浮起來，就會跟main-nav同層了，main-nav即會排列在
     * logo後面 */
}
@media screen and (min-width: 768px){
	.main-header .logo{
		justify-content: center;
	}
}
.logo img{
	vertical-align: middle;
}
.logo .menu-switch{
	width: 30px;
	height: 30px;
	background-color: #666;
	font-size: 0;
}
@media screen and (min-width: 768px){
	.logo .menu-switch{
		display: none;
	}
}
.main-nav{
	display: flex;
	flex-flow: column;
	position: absolute;
	width: 100%;
	background-color: rgba(0,0,0,.7);
}
#menu-control{
	position: absolute;
	opacity: 0;
}
@media screen and (max-width:767px){
	.main-nav{
		top: -100vh;
		transition: top .5s;
	}
	#menu-control:checked ~ .main-header .main-nav{
		top: 100%;
	}
}

@media screen and (min-width:768px){
	.main-nav{
		position: relative;
		/*margin-bottom: 20px;*/
		flex-flow: row nowrap;
		background-color: #CD0803;
	}
}
.main-nav a{
	width: 100%;
	box-sizing: border-box;
	border-bottom: 1px solid #f00;
	padding: 10px;
	text-align: center;
}
```

## 手機裝置如何接電腦使用開發者工具:
Android:
1關於手機 =>軟體資訊=>版本號碼點10下=>成為開發人員
2手機開發人員選項 =>USB偵錯打開

# ==BOOTSTRAP==
重要的4檔案:
1. BS CSS (可從BS下載)
2. JQ js
3. popper js
4. BS js (可從BS下載)

>234順序很重要 不要寫錯了～～ 

選單: component => navbar 內容複製貼上
![](https://i.imgur.com/RNPTjnz.png)

## ==重要觀念（Timecode 14:40~14:45）==
>- .container-fluid #滿版 內建 padding: 0 15px;
>- .container       #限制寬度
>- .row 
>    - display-flex #控制flex
>    - margin: 0 -15px;
>- .col
>
![](https://i.imgur.com/APcOxco.png)

### ==.row隔線系統為何要margin-left:-15px, margin-right: -15px?==

col-12的時候，代表col與row與container會等寬。
假設今天container裡面只有要放一層的資料，如果還要去寫成container包row包col-12太麻煩了，當然直接寫container然後裡面塞資料就好，但因為container帶有padding 15px，col也有padding 15px的寬度，加起來會是30px，所以在container裡面才會有margin: -15px 這個設定。

紅色：container
藍色：row
綠色: col-4
紫色➡️: 原先預設的padding 15px
灰色➡️:原先預設的padding 15px    

![](https://i.imgur.com/E17xqWr.png)

因為.container原本就有padding 15 px（紫色），row裡的col也有padding 15px（灰色）加起來是30px，所以才要設margin -15px去抵銷多出來的15px。


### ==container vs container-fluid==
![](https://i.imgur.com/XrmxkrT.jpg)

![](https://i.imgur.com/UyJNdyj.jpg)

max-width:100% 套用在圖片上，最大不超過圖片本身大小
![](https://i.imgur.com/SH4I8JR.jpg)

![](https://i.imgur.com/KZk2lUN.png)

### ==max-width: 100% =>將圖片填滿至固定寬高==
max-width: 100% =>將圖片填滿至固定寬高(最大不會大超過圖片的原始尺寸)

所以若圖片是200/200.   固定寬高為100/100. 就可以填滿
但如果圖片是100/100	 固定寬高為200/200. 就無法填滿 (為了避免圖片放大會有馬賽克)

### ==伴隨flex一起產生的屬性：order(預設值為0)==
.order-first = order:-1
.order-last  = order:13
.order-1 ~ .order-13
order的預設值為0
![](https://i.imgur.com/gPR3DR5.png)
![](https://i.imgur.com/bGAhuxu.png)

不是剛好欄寬然後居中
![](https://i.imgur.com/RWgQ41u.png)
![](https://i.imgur.com/77AYXsc.png)

### ==row: no-gutters: 讓col之間的間隔消失==

但最外面的margin還是存在（左右兩側的空白），row的margin -15px會被設為0，所以最邊邊會有空白變成沒有滿版，要自己新增一個class加上margin left & right -15讓它消失
![](https://i.imgur.com/lCe9mPf.png)

BS 的 margin class
mr = margin right
ml = margin left
mt = margin top
mb = margin bottom
mx = ml + mr
my = mt + mb

>### 觀念: margin 拿到auto的那一份就可以瓜分剩下的空間
![](https://i.imgur.com/FBgP4Z1.jpg)

ex: 
ul裡面的mr-auto：將剩餘空間，分配到ul的右邊
ul裡面的ml-auto：將剩餘空間，分配到ul的左邊
ul裡面的mx-auto：將剩餘空間，平均分配到ul左右兩邊

