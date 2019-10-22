---
title: Rails 190916 AstroCamp
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: Rails ,  Ruby, 
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}
# Rails 190916

## 產生訂單的套件
friendly id
https://github.com/norman/friendly_id
```
gem 'friendly_id', '~> 5.2.4' 
```
```
rails g migration add_slug_to_order slug:uniq
```
```
rails generate friendly_id
```

```ruby
# order.rb
extend FriendlyId
  friendly_id :order_generator, use: :slugged
  
#上略
#寫在最下面

private
  def order_generator
  year = Time.now.year
  month = Time.now.month
  day = Time.now.day
  serial = [*"A".."Z", *0..9].sample(8).join
  "%0d%02d%02d%s" % [year, month, day, serial]
end
```

```bash
# rails c
Order.find_each(&:save)
```
```ruby
#下面這兩個是一樣的
list.select { |l| l.odd? }
list.select(&:odd?)
```
```
Order.update_all(slug: nil)
```
## 加上 show 的頁面 11:15
```ruby
#orders_controller.rb
def show
  @order = current_user.orders.friendly.find(id: params[:id])
end
```
```htmlmixed
#views>orders>show.html.erb
<h1>訂單 <%= @order.slug.upcase%> </h1>
```
----
備註：如果在git push的時候有alert訊息，可到GitHub看是什麼訊息，上課例子是devise '4.7' 要更新到'4.7.1'，可用下面指令更新所有要更新的檔案。
```bash
bundle update
```
---
persisted? => 判斷資料是不是已經存在資料庫
```ruby
# order.rb
# 改為
private
def order_generator
  if persisted?   #如果存在  這裡省略self.
    year = created_at.year
    month = created_at.month
    day = created_at.day
  else
    year = Time.now.year
    month = Time.now.month
    day = Time.now.day
  end
  serial = [*"A".."Z", *0..9].sample(8).join
  "%0d%02d%02d%s" % [year, month, day, serial]
end
```
## 11:48 付款(這裡金鑰直接寫上去是不好的行為)
```ruby
#orders>index.html.erb
#上略
<%= link_to "付款", payment_order_path(order), class: 'btn btn-danger btn-sm' if order.may_pay? %>
```
```ruby
#orders_controller
before_action :find_order, only: [:show, :payment]
#下面private加入
def find_order
    @order = current_user.orders.friendly.find(params[:id])
end
```
讓按下付款後出現總價(total_price)，沒有total_price方法就做給他
### 方法一：
```ruby
#models>order.rb
  def total_price
    total = 0
    order_items.each do |item|
      total += item.product.price * item.quantity
    end
    
    #或
    
    order_items.reduce(0) do |sum, item|
      sum + (item.product.price * item.quantity)
    end 
  end
```
### 方法二：
```ruby
#order_item.rb
def total_price
  product.price * quantity
end

#order.rb
def total_price
    order_items.reduce(0) do |sum, item|
      sum + item.total_price
    end 
end
```
## 12:00 金流
braintree 註冊(記得用sandbox)
右上角？-> Developer docs -> Get started
![](https://i.imgur.com/pwyd6Zh.png)
Set Up Your Client -> Js 3 ->找下面程式碼

```htmlmixed=
#payment.html.erb
#貼上
<script src="https://js.braintreegateway.com/web/dropin/1.20.1/js/dropin.min.js"></script>

<div id="dropin-container"></div>
  <button id="submit-button">Request payment method</button>
  <script>
    var button = document.querySelector('#submit-button');

    braintree.dropin.create({
      authorization: 'CLIENT_TOKEN_FROM_SERVER',
      container: '#dropin-container'
    }, function (createErr, instance) {
      button.addEventListener('click', function () {
        instance.requestPaymentMethod(function (err, payload) {
          // Submit payload.nonce to your server
        });
      });
    });
</script>
```
Simple Sever -> Ruby -> 找程式碼
```
gem "braintree", "~> 2.98.0"
```
```ruby
#orders_controller.rb
#private下貼上
def braintree_gateway
    gateway = Braintree::Gateway.new(
    :environment => :sandbox,
    :merchant_id => "use_your_merchant_id",
    :public_key => "use_your_public_key",
    :private_key => "use_your_private_key",
    )
end

#改payment方法
def payment
  @token = braintree_gateway.client_token.generate
  #官方文件裡找的，只是貼上
end
```
Braintree -> 右上角設定 -> API
找public_key, merchant_id, private_key後貼上
![](https://i.imgur.com/V2CC4SV.png)

![](https://i.imgur.com/z6O2nyO.png)
```erb
#payment.html.erb裡面
改掉
authorization: 'CLIENT_TOKEN_FROM_SERVER' 

改成
authorization: '<%= @token%>'

#上面改成
<h1>付款(<%= @order.total_price%>元)</h1>

#按鈕難看
<button id="submit-button" class='btn btn-lg btn-danger'>付款</button>
```
![](https://i.imgur.com/Ef8W12y.png)
VISA測試卡號:4111 1111 1111 1111
日期：不要過期就好

13:40
```erb
#payment.html.erb

<%= form_with(url: '', local: true) do |form| %>
  <div id="dropin-container"></div>
  <%= form.submit '付款', class: 'btn btn-lg btn-danger', id: 'pay' %>
<!--   把付款按鈕改成用 form builder 做 -->
<!--   <button id="submit-button">付款</button> -->
<% end %>

<script>
  var button = document.querySelector('#pay');
            <!--   下略   -->
</script>
```
圖片支援：
![](https://i.imgur.com/amaVGoA.png)

這裏如果不加 local: true 會用 ajax 方式送出去，到時候還要再接一個call back回來，這樣設計後面會比較麻煩。可看流程圖步驟4，回來到your sever後，就要往Braintree sever走了，所以要把 remote 關掉。

14:05
龍哥改js v2
Set Up Your Client -> Js 2 -> 找下面程式碼

```htmlmixed=
#payment.html.erb

<h1>付款(<%= @order.total_price%>元)</h1>

<script src="https://js.braintreegateway.com/web/dropin/1.20.1/js/dropin.min.js"></script>

<form id="checkout" method="post" action="/checkout">
  <div id="payment-form"></div>
  <input type="submit" value="Pay $10">
</form>
    

  <script src="https://js.braintreegateway.com/js/braintree-2.32.1.min.js"></script>
<script>
// We generated a client token for you so you can test out this code
// immediately. In a production-ready integration, you will need to
// generate a client token on your server (see section below).

// var clientToken = "CLIENT_TOKEN_FROM_SERVER"; #改掉
   var clientToken = "<%= @token%>";


braintree.setup(clientToken, "dropin", {
  container: "payment-form"
});
</script>
```
```htmlmixed
#第7~10行改成
<%= form_with(url: '...', local: true) do |form| %>
  <div id="payment-form"></div>
  <input type="submit" value="付款", class= 'btn btn-lg btn-danger'>
<% end %>
```
![](https://i.imgur.com/HYqOrNV.png)
```ruby
#route.rb
resources :orders, only: [:index, :show, :create] do
    member do
      get :payment
      post :transaction #加入路徑
    end
end

#payment.html.erb
<%= form_with(url: transaction_order_path(@order), local: true) do |form| %>
#下略

#orders_controller.rb
#印出來看看
def transaction
  render html: params
end
```
按下付款後，印出來看看
![](https://i.imgur.com/LMBHlA9.png)
發現這些資訊裡面是沒有卡號的
JS 是拿著卡號給 braintree 並不是拿回來給 server，這是好事因為 server 存著卡號並不好， server 拿到的只有 nonce 

```ruby=
#orders_controller.rb
#找文件程式碼貼上，第7~15行
#@order，記得before_action要加上:transaction
#
 def transaction
   if @order.may_pay? #may_pay? 是AASM 產生的方法
      result = braintree_gateway.transaction.sale(
      # :amount => "10.00", #改掉
      :amount => @order.total_price,
      # :payment_method_nonce => nonce_from_the_client, #改掉
      :payment_method_nonce => params[:payment_method_nonce],
      :options => {
        :submit_for_settlement => true
        }
      )

      if result.success?  #braintree手冊內建方法
        @order.pay!       #pay! 是AASM 產生的方法
        redirect_to orders_path, notice: '刷卡已完成'
      else
        redirect_to orders_path, notice: '付款出現錯誤！'
      end
      
     else
    redirect_to orders_path, notice: '訂單已完成付款'
     end  

    # render html: params 
 end
```
Success 這個方法是 braintree 提供的
![](https://i.imgur.com/jYiMG1A.png)
![](https://i.imgur.com/jmzLEDM.png)
刷卡成功的話可以在 braintree 頁面看到這個畫面
![](https://i.imgur.com/KuaJv58.png)

## 15:02 把金鑰藏起來
套件 Figaro
https://github.com/laserlemon/figaro
```
gem "figaro"
```
```bash
bundle exec figaro install
```
![](https://i.imgur.com/whLPfAv.png)
找到下面程式碼貼上
![](https://i.imgur.com/fVlYz8N.png)

```yml
#application.yml

pusher_app_id: "2954"
pusher_key: "7381a978f7dd7f9a1117"
pusher_secret: "abdc3b896a0ffb85d373"

#改成自己要的名字
braintree_merchant_id: "2954"
braintree_public_id: "7381a978f7dd7f9a1117"
braintree_private_id: "abdc3b896a0ffb85d373" 
#記得這三個金鑰都要改成自己的



#orders_controller.rb
def braintree_gateway
  gateway = Braintree::Gateway.new(
    :environment => :sandbox,
    :merchant_id => ENV["braintree_merchant_id"],
    :public_key => ENV["braintree_public_id"],
    :private_key => ENV["braintree_private_id"],
  )
end
```
加入
![](https://i.imgur.com/tDduSnQ.png)

通常不會把真正的金鑰推上去，但會留一份 sample 給接手的人，他才知道怎麼處理

為什麼這個檔案都已經加到 git ignore 檔案裡面還是會被偵測到有改？
因為在它加到這個檔案之前有版控過

gitignore 
```bash
git rm 'config/database.yml' --cache
```

## 15:30 Heroku
https://devcenter.heroku.com/articles/heroku-cli
```bash
curl https://cli-assets.heroku.com/install.sh | sh
```
![](https://i.imgur.com/4yODotf.png)
確定進入專案的資料夾
```bash
heroku create

如果出現invalid
heroku login輸入帳密後再create一次
```
建好之後可以在 dashboard 看到剛剛新增的
![](https://i.imgur.com/wZOqDyt.png)
如果heroku要改名字的話，記得終端機也要執行指令去改

```bash
git remote -v
git remote rm '要刪除的節點名稱'
git remote add '命名的節點名稱' https://git.heroku.com/pure-castle-81375.git
#copy  會噴喔
```
![](https://i.imgur.com/6d8KDoG.png)

```bash
git push '命名的節點名稱' master

＃git 把 master 推到  heroku上的 '命名的節點名稱' 這個遠端節點
```
![](https://i.imgur.com/pidTl4V.png)
## ==每次push之前都記得要commit到最新==

## 修正Detected sqlite3問題 16:04

新增專案的時候同時指定資料庫
```bash
#建立專案的時候就指定資料庫
rails new Aaa -d postgresql
#copy  會噴喔
```
因為專案一開始new的時候沒寫-d postgresql，只好手動設定。
```
gem 'pg', '~> 0.18.4'
```
```yml
#database.yml
#改database

default: &default
  adapter: postgresql
  encoding: unicode
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>

development:
  <<: *default
  database: ivoting_development

test:
  <<: *default
  database: ivoting_test

production:
  <<: *default
  database: ivoting_production
  # username: ivoting
  # password: <%= ENV['AAAA_DATABASE_PASSWORD'] %>
```
rails db:migrate後會噴錯，因為sqlite是檔案型資料庫，會直接幫我們建立資料庫
```
wuyongerde-MacBook-Pro:ivoting louis$ rails db:migrate
rails aborted!
ActiveRecord::NoDatabaseError: FATAL:  database "ivoting_development" does not exist
```
用postresql時，要自己手動建立資料庫
```
rails db:create
rails db:migrate
```
再自己產生假資料，因為換資料庫，之前的資料不會過來
```bash
rails db:seed          #產生候選人
rails product:generate #產生product
```

轉好資料庫之後，再重新 push 一次 heroku

最後會出現網址
![](https://i.imgur.com/QnC11EQ.png)
但網址是這個
![](https://i.imgur.com/bVy0YlJ.png)
還是會出錯，因為在安裝的過程中，heroku沒有做rails db:migrate
![](https://i.imgur.com/uhPHTtt.png)
Faker跟FactoryBot是寫在Gemfile的test裡面，要把它搬到外面來
bundle後再commit一次，再推上heroku一次
```bash
heroku run rails db:migrate
heroku run rails db:seed          #產生候選人
heroku run rails product:generate #產生product
```
==如果出現錯誤訊息==
![](https://i.imgur.com/m5YyUTg.png)
```bash
rake assets:precompile
```


可以用 figaro 指令直接把環境變數設定好
```bash
 figaro heroku:set -e production
```

