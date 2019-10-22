---
title:  Ruby 190812 AstroCamp
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: Ruby, Rails
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}


## ==Time code==
10:43 Rack
11:35 Middle ware
12:05 中介疊疊樂（p23.）
12:14 sinatra
14:31 get '/cats/:id'
14:57 共用版面
15:53 表單
>https://railsbook.tw/extra/rack.html

 Rack 寄生在Web Server上的程式（介面卡） 
 HTTP狀態：一連串的數字
 1.「一切都正常」的 200
 2.「這個頁面目前不在這裡，請你去這裡看看」的轉址 301 「不知道為什麼，東西不在這裡」的找不到頁面 404
 3.「伺服器錯誤」的 500
 
 Rack的三種規格：
1. 狀態
2. Header
3. Body
 ![](https://i.imgur.com/Od7bCk3.png)
 
 這裡的run 方法是由rackup 提供的，
 ａｐｐ是自己命名的
 ![](https://i.imgur.com/SmazA2h.png)
 在網址打上localhost加上跑出來的數字就會出現自己編輯的頁面
 ![](https://i.imgur.com/eGB0dgP.png)
 ![](https://i.imgur.com/Geec46n.png)


run 後面的物件只要支援 call方法即可，因此也可以自己定義一個類別，重新定義call 方法

每修正一次，就要Ctrl+C退出，儲存一次後再rackup一次。

![](https://i.imgur.com/J7CVmvW.png)
若是自己定義call method而不給變數會產生下圖結果
![](https://i.imgur.com/Yv9lO4w.png)
故若要寫一個方法的時候，也就是rack在呼叫call時，需要給他一個環境變數env(使用者瀏覽器版本環境相關資訊)也可以是個x
![](https://i.imgur.com/0eu01NC.png)
Block裡面其實也有env這個環境變數，只是省略不寫而已
![](https://i.imgur.com/wKVvphs.png)


## ==MiddleWare==

可以解讀成rack上的外掛，就像疊積木會有好幾層，最下面的就是rack，上面會疊好幾層middleware。
為了模組化，不會將所有的東西都寫在rack，故會寫好幾層。

==Ruby on Rails 本身就是一個Rack應用程式==
```irb
    rails middleware #try it!
```

![](https://i.imgur.com/jTXm3xs.png)
上圖的app是 Proc.new後面這整陀，也就是上一個程序執行完的結果，會叫進來下一個middleware，也因為他是Proc因此可以用@app.call這個方法，在每個middle ware最後同樣也要回傳相同格式的規格回去給下一個middleware

## ==sinatra==

DSL = Domain Specific Language
>領域特定語言指的是專注於某個應用程式領域的計算機語言。

Sinatra = Web DSL
Rails = Web DSL

```irb
  require 'sinatra'
  get '/frank-says' do
    'Put this in your pipe & smoke it!'
  end
```

遞迴：https://i.imgur.com/U92Q9ms.gif

## 解決Sinnatra 無法自動Reload
```ruby
require "sinatra"
require "sinatra/reloader" if development?
```


![](https://i.imgur.com/fpHzNIq.png)
erb = embedded ruby 呼叫一個名叫about的erb檔案
在erb檔案裡面 可以用<%= %>裡面的空間寫ruby的語言
![](https://i.imgur.com/XHHfPoK.png)


```ruby=
get '/cats' do
  erb :all_cats
end

#用:id傳入值 , 透過 params[:id]  取得:id 的值
#:id其實是Hash，所以可以給多個elemant，例:id/:aaa/:bbb
get '/cats/:id' do
  "hi, #{params[:id]} 號貓"
end
``` 
![](https://i.imgur.com/5XoBZQb.png)

共用版面：
![](https://i.imgur.com/2WDWGsD.png)
![](https://i.imgur.com/mgaxd7Y.png)
![](https://i.imgur.com/7q18mFP.png)

雖然erb檔案裡面可以用 <%= %>用ruby語言，但在views 裡面的文件理論上只是要印出畫面，盡量只放一些被動的東西

所以像下面一樣，在主程式去抓資料
在erb檔裡面去被動的接收實體變數
![](https://i.imgur.com/WWAzc2U.png)
![](https://i.imgur.com/MMjY8Bk.png)

==後端只認name屬性==
![](https://i.imgur.com/Tab92oL.png)

## get 和 post 的差別 => get 會在網址上面跑出數值 post不會
![](https://i.imgur.com/OLvwNTb.png)
![](https://i.imgur.com/hcoK7QF.png)
## BMI 計算
```ruby
get '/form' do 
  erb :form
end

post '/bmi_calc' do
  # "#{params}"
  h = params[:height].to_f / 100
  w = params[:weight].to_f 
  @bmi = (w / (h**2)).round(2)
  
  @status = case @bmi
  when 13..17.99
    "過輕"
  when 18..22.99
    "標準"
  when 23..29.99
    "微胖"
  when 30..
    "砍掉重練"
  end
  
  erb :bmi_result
end
```
```html
<!--form.erb-->
<h1>BMI 計算</h1>

<form action="/bmi_calc" method="post">
  <label for="h">身高</label>
  <input type="text" name="height" id="h">  
  <br />
  <label for="w">體重</label>
  <input type="text" name="weight" id="w">
  <br />
  <input type="submit" value="計算!!">
</form>
```
```html
<!--bmi_result.erb-->
你的 BMI 是： <%= @bmi %>
健康狀態：<%= @status%>
```

## 登入試做
```htmlmixed=
    <!--login.erb-->
<h1>登入頁面</h1>
<form action="" method="post">
  帳號：<input type="text" name="username"><br />
  密碼：<input type="password" name="password"><br />
  <input type="submit">
</form>
```

```ruby
# sinatra 預設session未啟用
enable :sessions

get '/' do
  if session[:kakamonster]
    "<h1>親， #{Time.now}</h1>"
  else
    "<h1>Hi #{Time.now}</h1>"
  end
  
end

                    #......

get '/login'  do
  erb :login
end

post '/login' do
  if params[:username] == "kk" && params[:password] == "123"
    #pass 
    session[:kakamonster] = params[:username] 
  else
    session.clear
  end
  redirect '/'
end
```
==Q.session 和 cookie 的差別==
session 存放在server端
cookie  存放在client端（號碼牌）   
兩邊要match才會有效
２


