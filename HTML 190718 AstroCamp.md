---
title: HTML 190718 AstroCamp 
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: false
tags: HTML
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}
###### 20190718   tags: `HTML`
# HTML




+ What is HTML ?
    + 標記語言
+ What is HTML for ?
    + 將==資料==轉換成==標記==或是==有意義的資料（內容）==
        +  特殊字元符號的標示方式
            +  [特殊字元符號](https://codertw.com/程式語言/533294/)
            +  [特殊符號-Copy&Paste](https://tw.piliapp.com/symbol/)
+ 所以，HTML是要給誰看的？
    + 透過不同的載體來讀取HTML(整理過的資料)，再將畫面呈現在使用者面前。
    + HTML其實是在將需求者想呈現的資料

---

     
    

#     軟體操作技巧
Sublime篇：

| 按鍵 | 目的/功能 |
|:------- |:----------- |
| cmd + D   | 選定範圍後，連續執行CMD + D，可快速選取相同內容。 |
| ctrl + W | 快速Wrap：選取範圍後執行，會呼叫出命令列，輸入對應標籤後，可在選取範圍外側加入新標籤。 |
| cmd + shift + P   | 呼叫命令面板：可快速切換檔案模式（輸入：html, css, ruby, ...）或是  挑選佈景主題（輸入：color）|
| cmd + L  |選取整行Code|
|cmd + Arrow Key|move to the edge.|
|cmd + ctrl + up/down|將選取範圍整個向上/下移動
|

---
# 標籤（Tag）
+ 標籤：為了能更清楚表達語意和語義
    + 重要的是資料之前的==結構==和==關聯性==    
 


| 標籤（Tag） | 存放內容 |
|:------- |:----------- |
| <html></html>   | 存放整個網頁的內容。 |
| <head></head>| 主要是放要給瀏覽器看的內容。 |
| <body></body>   | 要給人看的內容。|  



---

# 必裝外掛
Sublime篇：

| 套件名稱 | 功能 |
|:------- |:----------- |
|SublimeCodeIntel||
|Emmet|必裝 BJ4|
|AutoFileName||
|Chinese Lorem|產生中文假文|
|Fakimg|假圖產生器|
|||
|||

[其他Sublime外掛](https://codertw.com/%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80/119839/) (視個人需求安裝)




VScode篇：
| 套件名稱 | 功能 |
|:------- |:----------- |
| shell command | 在終端機輸入指令`code`，開啟檔案。 |
|||
|||
|||

 

---

# Web 資料的種類

+ 文字 
    + 標題   h1~h6
    + 段落 p 
    + 意義強調
        + strong
        + em 
    + 視覺強調(外觀)
        + b
        + i 
+ 文章區域
    + section
    + article
    + header
    + 

        
+ 圖片
    + img        
    + figure
        + figcaption
    + picture 
        + ==picture== ?
         
        + ==source== ?
            
    + 圖片格式
        + jpg
        + png
        + gif
        + ==webp== ?


+ 影音
    + vedio
    + audio 
+ 表單
    + form
    + input
        + type in
            + text
            + tel
            + email
            + password
            + search
            + url
            + number
        + post
            + button
            + submit
            + reset
            + image
        + date/time picker
            + date
            + datetime-local
            + month
            + week
            + time
        + other
            + file
            + range
            + hidden
        
    + texarea 
+ 清單
    + 無序清單 ul>li
    + 有序清單 ol>li   
    + 描述清單 dl>dt + dd
+ 表格
+ 資料區隔/群組
    + div：有關聯性的資料
    + span：某一個段落的資料
        
         
---
其他外掛(Sublime)
```
gitGutter
markdownlivepreview
AdvencedNewfile
```

a html target

