---
title: Rails 190909 AstroCamp
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: Rails,Ruby
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}
# Rails 190909
每天都睡9小時以上是不是很奢侈？
好羨慕...
羨慕・゜・(PДq｡)・゜・
裝睡的人叫不醒，覺醒的人不用睡?
求今天只睡5.5小時的人的心理陰影面積
我只睡了4.5小時......
我以後還是努力點好了..（跪

[Spring Stop](https://ihower.tw/rails/rails-recipes-back-end.html)

```
p1 = FactoryBot.build(:product)
```
![](https://i.imgur.com/VRwuTa0.png)
![](https://i.imgur.com/3tQF3cZ.png)

查文件，到rails_helper.rb貼上第36行那段
https://github.com/thoughtbot/factory_bot/blob/master/GETTING_STARTED.md
![](https://i.imgur.com/s41rxma.png)


![](https://i.imgur.com/wkD3L57.png)

```ruby
原本要打FactoryBot.create()
貼上那段到rails_helper.rb之後，就可以少寫FactoryBot,可直接create
     # p1 = Product.create(name: 'aa', price: 100)
     # p2 = Product.create(name: 'bb', price: 50)

      p1 = create(:product, price: 100)
      p2 = create(:product, price: 50)
```
![](https://i.imgur.com/ad3L4zp.png)

trait：特徵
在create東西的時候，可以直接加入特徵。
```ruby
#只是舉例
p1 = create(:product, :cheap)
p2 = create(:product, :cheap)
```
![](https://i.imgur.com/mEQ45Wr.png)

比對字串時，應該要用 ==eq== ，be 是比對物件ID

```ruby
context "進階功能" do
    it "可以將購物車內容轉換成Hash並存到Session裡。" do
      p1 = create(:product)
      p2 = create(:product)

      3.times { cart.add_item(p1.id) }
      2.times { cart.add_item(p2.id) }
      
      expect(cart.serialize).to eq cart_hash
      #cart.serialize = cart.to_h
    end
  end

  private
  def cart_hash
    cart_hash = {
        "items" => [
          {"product_id" => 1, "quantity" => 3},
          {"product_id" => 2, "quantity" => 2}
        ]
      }
  end
```

```ruby
#cart.rb     先做出樣版，再進一步做修改
def serialize
    {
      "items" => [
        { "product_id" => 1, "qunttity" => 3 },
        { "product_id" => 2, "qunttity" => 2 }
      ]
    }
end

#改用each迴圈印出真正的方法
   def serialize
    # item_list = [
    #       {"product_id" => 1, "quantity" => 3},
    #       {"product_id" => 1, "quantity" => 3}]

    item_list = []
    @items.each do |i|
      item_list << {"product_id" => i.product_id, "quantity" => i.quantity}
    end

    { "items" => item_list }
  end
  

```
在外面定義一個東西再來改他是不好的做法
```ruby
#cart.rb
  def serialize
  #改用ruby風味的方法
    # item_list = []
    # @items.each do |i|
    #   item_list << { "product_id" => i.product_id, "quantity" => i.quantity}
    # end

    item_list = @items.map{ |i| {"product_id" =>i.product_id,
                                 "quantity" => i.quantity} }

    { "items" => item_list }
  end

```
11:52 "還原購物車內容"
```ruby
#cart_spec.rb
it"也可以存放在Session的內容(Hash格式)，還原成購物車的內容。)"do
      cart = Cart.from_hash(cart_hash)

      expect(cart.items.count).to be 2
      expect(cart.items.first.quantity).to be 3
end
```
```ruby
#cart.rb
def from_hash(hash)
end
```
還是會噴錯
![](https://i.imgur.com/DAsH0bc.png)
因為Cart是類別，故要用類別方法。

```ruby
#cart.rb
def self.from_hash(hash)
end
```
接著想辦法把 items 做出來
```ruby
#cart.rb
  def self.from_hash(hash = nil)
    # hash = {
    #     "items" => [
    #       {"product_id" => 1, "quantity" => 3},
    #       {"product_id" => 2, "quantity" => 2}
    #     ]
    #   }
    if hash && hash["items"]   #如果是hash，且此hash帶有“items"的key的格式
      list = []
      hash["items"].each do |i|
        list << CartItem.new(i["product_id"],i["quantity"])
      end
      Cart.new(list) 
    else
      Cart.new #如果他給我的格式不對或是nil的時候，就給他一台空車
    end
  end
```
![](https://i.imgur.com/aJ3scKL.png)

```ruby
#cart.rb  12:24
#改成
def initialize(items = [])
    @items = items
end
#原來的initialize沒給參數，但Cart.new(list)有給一個參數
```
優化程式碼
```ruby
def self.from_hash(hash = nil)

    if hash && hash["items"]
      list = hash["items"].map{|i| CartItem.new(i["product_id"],i["quantity"])}  
      Cart.new(list) 
    else
      Cart.new
    end
end
```
超噁心優化
```ruby
def self.from_hash(hash)
    
    if hash && hash["items"]     
      new hash["items"].map{|i| 
          CartItem.new(i["product_id"], i["quantity"])} 
          #在同一類別下，可把Cart.new寫成new  
          #因為結合律問題，不能用do...end，需改成{}
      new(list)
    else
      new
    end
  end
```

# 13:48 rails product:generate

產生多筆資料(用db:seed也可以)
![](https://i.imgur.com/6bHimDd.png)
```rake
#開一個資料夾
#lib>task>generate_product.rake 
namespace :product do
  desc '產生10筆樣本商品'
  task :generate => :environment do
    puts "generating products..."
    10.times { FactoryBot.create(:product) }
    puts "done"
  end
end
```
```
rails product:generate
```

建立 products
先把商品頁面做出來
```
rails g controller Products
```
```ruby
#routes.rb
#加入
 resources :products do
    member do
      put :add_to_cart
    end
 end
```
```htmlmixed=
<!-- app>views>products>index.html.erb -->
<h1>商品列表</h1>

<ul>
  <% @products.each do |product|%>
    <li>商品名稱：<%= link_to product.name, product%>｜價錢：<%=product.price%>元
    <%= link_to "加入購物車", add_to_cart_product_path(product.id), class: 'btn btn-sm btn-danger', method: 'put'%></li>
  <%end%>
</ul>
```
```ruby
#products_controller.rb
class ProductsController < ApplicationController
  before_action :find_product, only: [:add_to_cart, :show]
  def index
    @products = Product.all
  end

  def show
  end

  def add_to_cart
    cart = Cart.new
    cart.add_item(@product.id)

    redirect_to products_path, notice: '已加入購物車'
  end

  private
  def find_product
    @product = Product.find(params[:id])
  end
end

```
補充：find_product內要用find方法，會跳rescue
(find_by:找不到方法時會回傳nil，find:找不到方法時會跳rescue)
### 捕捉錯誤訊息
![](https://i.imgur.com/a74Zi1p.png)

![](https://i.imgur.com/FRfOIa5.png)
==＊放在public資料夾下的html檔案，是不用經過路徑對照表的。==



```ruby
private
  def find_product
    begin
      @product = Product.find(params[:id])  
    rescue  ActiveRecord::RecordNotFound
      render :file => 'public/404.html', 
             :status => :not_found,
             :layout => false
    end
  end
```

我們還可以把這個抓錯方法往上拉
讓 candidate 跟 product 這兩個 controller 都能用
使用 rescue_from 這個方法



```ruby
#product_controller.rb
private
  def find_product
      @product = Product.find(params[:id])  
  end
  
#application_controller.rb
class ApplicationController < ActionController::Base
  rescue_from ActiveRecord::RecordNotFound, with: :record_not_found

  private
  def record_not_found
    render file: "#{Rails.root}/public/404.html", 
           status: :not_found,
           layout: false
  end
end
#就不會只針對product去rescue，而是個網站(candidate, product..)
```

# 15:15 回來購物車

因為  stateless 的緣故，12,13 行 的狀態無法被傳遞至下一個頁面，
所以要把 cart 存入   session

```ruby＝=12
def add_to_cart
    cart = Cart.new
    cart.add_item(@product.id)
    
    session[:cart9527] = cart.serialize 
    #session就像號碼牌
    redirect_to products_path, notice: " 已加入購物車 ！"
end
```

來看看我們能拿到什麼

```erb
#index.html.erb
<h1>商品列表</h1>
<%=session[:cart9527] %> 
<%= Cart.from_hash(session[:cart9527]) %>
```
![](https://i.imgur.com/4IFvlEd.png)

那為什麼現在重複點加入購物車數量不會變？ 因為 add_to_cart 這個方法寫錯了，每次都會做一台新的購物車
所以應該要改成從 session 抓下來
```ruby
  def add_to_cart
   #cart = Cart.new 每次來都給一台新車
    cart = Cart.from_hash(session[:cart9527])
    cart.add_item(@product.id)

    session[:cart9527] = cart.serialize

    redirect_to products_path,notice:"已加入購物車"
  end
```
15:39 優化程式碼
```ruby=
#private下加
def current_cart
    Cart.from_hash(session[:cart9527])
end


def add_to_cart
current_cart.add_item(@product.id)
session[:cart9527] = current_cart.serialize

redirect_to products_path, notice:"已加入購物車"
end
```
這樣寫只是替換名字，結果去點“加入購物車”後，數量就不會增加了？
因為第3行，帶入第8行時，確實會加入數量沒錯，但在第9行時，又再度帶入了舊的第3行程式碼而不是第8行的。


==rails memorization==
```ruby
#private改成
def current_cart
    @cart = @cart || Cart.from_hash(session[:cart9527])
end
```
# ==ViewHelper==
## ==View拿不到controller裡的方法==

![](https://i.imgur.com/OAkR1KJ.png)

```htmlmixed=
#_navbar.html.erb
#加入
<%= link_to "購物車(#{current_cart.items.count})", "#", class: 'nav-link' %>
```
![](https://i.imgur.com/1ONJ9F8.png)
解決方法一：view_helper.rb
```ruby=
#app/helpers/products_helper.rb
module ProductsHelper
  def current_cart
    @cart = @cart || Cart.from_hash(session[:cart9527])
  end
end
```
==view_helper寫的方法只能給view用，controller拿不到==
helper的目的是要寫一段ruby code 協助整理資訊
如果要controller拿到helper裡的方法的話，可在controller加入
```ruby
  #products_controller.rb
  include ProductsHelper
```
接著來實作看看怎麼讓 candidates 頁面中，超過 0 票的選票前面多個星星
```ruby
#helpers/products_helper.rb
module ProductsHelper
  def star_mark(candidate)
    if candidate.vote_logs_count > 0
      "* #{candidate.vote_logs_count}票"
    else
      "#{candidate.vote_logs_count}票"
    end
  end
end
```
然後 view 裡面就變成這樣
```erb
<%= link_to star_mark(candidate), log_candidate_path(candidate) %>
```


解決方法二：
方法寫在 controller 輸出給 helper 用
但是這樣做的話，只有在特定controller可以用，如果要全部的controller都可用的話，可貼在最上層的controller：
```ruby
#application_controller.rb
helper_method :current_cart
#方法寫在這
  def current_cart
    @cart = @cart || Cart.from_hash(session[:cart9527])
  end
```

方法一及方法二都是在讓views拿到controller裡面的方法，至於要選擇哪種方法的判斷標準：
Contorller 用得多就寫在 Contorller
view 用得多就寫在 view

# ==購物車path建立==
```ruby
#get '/cart', to: 'cart#index'
resources :cart 
cart不用s，因為購物車只有一台，但也不該帶id，/cart/5很怪吧~

故應該要寫成
resource :cart, only: [:show, :destroy] 
resource不加s
```


