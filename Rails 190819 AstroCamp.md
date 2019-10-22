---
title: Rails 190819 AstroCamp
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: Rails, Ruby, Gem, 
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}


16:25 hirb-unicode套件安裝

new 新增的頁面
![](https://i.imgur.com/Iy2jsv4.png)


create 新增的動作？


## InvalidAuthenticityToken
![](https://i.imgur.com/uHyHvbb.png)

![](https://i.imgur.com/JEzuqs3.png)
### (CSRF token)
在rails為了安全，會在每次提交的時候產生一個token，且每次重新整理都會是一次新的token，所以得加入一個隱形的hidden token（第5行）

==因為是html 所以不會有錯誤訊息== 
例如: line:3 method 打成methods


## ==建立Model==
```
rails generate model Candidate name:string party:string age:integer policy:text

//刪除：把ggenerate(g) 換成destroy(d)


/app/models/candidate.rb
/db/migrate/....rb
```
![](https://i.imgur.com/cUknyWL.png)
string可以不用寫 其他必須要
![](https://i.imgur.com/E5sXBwZ.png)

建立資料表
![](https://i.imgur.com/iWEfVpv.png)會生出 含id 7個欄位喔～
然後建立資料表吧
![](https://i.imgur.com/BipMzrO.png)
==db:migrate 只會執行一次==
換個說法：==每個migration檔，只會被執行一次==

如果想重做可以倒退
![](https://i.imgur.com/ZoIV5CM.png)


:::danger
執行 rails db:rollback
慎用！！
:::
:::info
若想新增資料 再做一次新的migration，除非非常確定，否則用了rollback後沒辦法再回復
:::
![](https://i.imgur.com/JbUQ9Ya.png)
![](https://i.imgur.com/q0euWSL.png)

![](https://i.imgur.com/RMwalTD.png)
![](https://i.imgur.com/kRDdpnB.png)

text_field, text_area 都是rails設計者自己做的方法


views
![](https://i.imgur.com/DwBVmAd.png)
control
![](https://i.imgur.com/hxJgH4G.png)
```htmlembedded=
<!--  new.html.erb -->
<%= form_for(Candidate.new) do |form| %>
  ......
<% end %>

<!-- 不應該在表單產生新的物件，讓他在controller 就完成 -->
<!-- 改寫如下： -->

<%= form_for(@candidate) do |form| %>
  ......
<% end %>

```

```ruby=
#candidates_controller.rb
 def new 
    @candidate = Candidate.new
 end
 ```
 第8行 
 html: params["candidate"] 
 輸出 {"name"=>"hhh", "party"=>"","age"=>"","policy"=>""}
 
 html: params["candidate"]["name"]
 輸出 hhh
 
## ForbiddenAttributesError
 ![](https://i.imgur.com/eZpYdJc.png)
==require +  permit解決==
![](https://i.imgur.com/H5vQTWD.png)

rails console   (可簡寫rails c)
![](https://i.imgur.com/ZcCczQX.png)

![](https://i.imgur.com/SDUHjNg.png)

rails c 使用的資料庫跟真正的是同一個，所以亂改有危險，若要做測試可以用 rails c --sandbox

輸入完資料轉到首頁
![](https://i.imgur.com/kyJA6GP.png)

![](https://i.imgur.com/JOXyA75.png)
![](https://i.imgur.com/RBoGn4o.png)

讓首頁出現輸入名單
![](https://i.imgur.com/644RD6E.png)
![](https://i.imgur.com/TJ7J5Wl.png)
資料驗證


![](https://i.imgur.com/CwBWEl3.png)
```htmlembedded=
    validates :name, presence: true
    validates (:name, presence: true)
    validates (:name, {presence: true})
    要習慣第一行的寫法
```
![](https://i.imgur.com/FMgOMML.png)
但洗掉已填資料很煩 可以改用

![](https://i.imgur.com/OgM5cos.png)
輸入錯誤並回到輸入頁面後，原本寫的資料不會因此洗掉。
![](https://i.imgur.com/pl5QDKa.png)

:new 是去找new.html.erb檔案，而不是上面的new method

>精心設計的巧合（或者說是，規則）：
>
>@candidate 剛好同時存在於 new.html.erb 與 create 這個方法裡面（而且正好夾帶了一包params）。
>
>當執行 @candidate.save 失敗的時候，
在render時，借用new.html.erb，需要有 @candidate。
>
>此時，create 方法裡的 @candidate 一個神支援，讓一切順利運行。[color=#39efd4]

![](https://i.imgur.com/8RSXAhF.png)


![](https://i.imgur.com/Uuboupu.png)


Model來判斷使用者輸入的資料
![](https://i.imgur.com/rCAo5Fa.png)
手冊
https://guides.rubyonrails.org/active_record_validations.html#numericality


==Black Magician saied:==
:::success
Come with me and let me be your faith.
:::
```htmlembedded
<%= link_to candidate.name, candidate_path(candidate.id) %>
<%= link_to candidate.name, candidate_path(candidate) %>
<%= link_to candidate.name, candidate %>
```
![](https://i.imgur.com/C9DcfeM.png)
![](https://i.imgur.com/0sbiNGa.png)

![](https://i.imgur.com/f7KxvFj.png)
讓資料排列顯示，較易閱讀

## hirb-unicode 
https://rubygems.org/gems/hirb-unicode

進入rails console 後

須執行： Hirb.enable
![](https://i.imgur.com/8pM3H6T.png)
