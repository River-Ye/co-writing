---
title: Rails 190910 AstroCamp
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags:  Rails, Ruby 
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}
#  Rails 190910
今天終於早起，但是起來後恍神一下人就在五倍惹...
我今天約莫六點第一次起床，再一次起床又快十點了
我今天七點起床，然後就沒有然後惹...
我今天七點第一次起床，再醒來就已經九點零七分了ＸＤ
揪團參加鐵人賽(誤
揪揪揪
各種揪
louis太強惹XD

---

先在購物車連結做出購物車的列表
```htmlembedded=
<!-- show.html.erb -->
<h1>購物車</h1>
總價<%= current_cart.total_price %>

<table>
  <thead>
    <tr>
      <td>商品名稱</td>
      <td>數量</td>
      <td>單價</td>
      <td>小計</td>
    </tr>
  </thead>
  <tbody>
    <% current_cart.items.each do |item| %>
    <tr>
      <td> <%= item.product.name%> </td>
      <td> <%= item.quantity%> </td>
      <td> <%= item.product.price %> </td>
      <td> <%= item.total_price%> </td>
    </tr>
    <% end%>
  </tbody>
</table>
```
做出結帳 / 繼續選購 / 清空購物車按鈕
```htmlmixed=24
<!-- 上略 -->
<tfoot>
    <tr>
      <td>總價：</td>
      <td><%=current_cart.total_price%></td>
    </tr>
  </tfoot>
</table>  
    <%= link_to "繼續選購", products_path, class:'btn btn-lg btn-primary' %>
    <%= link_to "結帳", '#', class:'btn btn-lg btn-danger' %>
```
```htmlmixed=
<h1>
購物車
<%= link_to "清空購物車", cart_path,class:'btn btn-xs btn-danger', method:'delete',data: {confirm:'確定要清空嗎？'} %>
</h1>
<!-- 下略 -->
```
在 carts controller 做出 destroy 方法

```ruby

  def destroy
    session[:cart9527] = nil
    redirect_to products_path, notice: "購物車已清空！"
  end
```
接著做出如果購物車沒東西，購物車連結就不會出現的效果
```erb
<% unless current_cart.empty? %>
<li class="nav-item">
<%= link_to "購物車(#{current_cart.items.count})", cart_path, class: 'nav-link' %>
</li>
<% end %>

or(精簡)

<li class="nav-item">
<%= link_to "購物車(#{current_cart.items.count})", cart_path, class: 'nav-link' unless current_cart.empty?  %>
</li>

unless可寫成if not
```
為什麼unless 前面不用加“,”?
因為上面那串其實是長這樣：
```ruby
<%= link_to(*args) unless current_cart.empty? %>
```

11:12結帳路徑

```erb
#routes.rb加入結帳路徑
get '/checkout', to: 'carts#checkout'

or

resource :cart, only: [:show, :destroy] do
  collection do
    get :checkout #/cart/checkout
  end
end

#carts_controller.rb
def checkout
end

#app/views/carts/checkout.html.erb
<h1>結帳</h1>
```

==partial render整理一下==
```erb
#views/carts/_cart.html.erb
<table class='table'>
  <thead class="thead-dark">
    <tr>
      <th>商品名稱</th>
      <th>數量</th>
      <th>單價</th>
      <th>小計</th>
    </tr>
  </thead>
  <tbody>
    <% current_cart.items.each do |item|%>
    <tr>
      <td><%=item.product.name%></td>
      <td><%=item.quantity%></td>
      <td><%=item.product.price%></td>
      <td><%=item.total_price%></td>
    </tr>
    <%end%>
  </tbody>
  <tfoot>
    <tr>
      <td colspan='3'> 總計:</td>
      <td><%= current_cart.total_price %></td>
    </tr>
  </tfoot>
</table>  
    <%= link_to "繼續選購", products_path, class:'btn btn-lg btn-primary' %>
    <%= link_to "結帳", checkout_path, class:'btn btn-lg btn-danger' %>
```
```erb
#checkout.html.erb
<h1>結帳</h1>

<%= render 'cart' %>

#show.html.erb
<h1>
購物車
<%= link_to "清空購物車", cart_path,class:'btn btn-xs btn-danger', method:'delete',data: {confirm:'確定要清空嗎？'} %>
</h1>
<%= render 'cart' %>
```
11:46 按下結帳，強迫登入會員

因為會員驗證可能會很多次，故用before_action的方式
```ruby
#carts_controller.rb
class CartsController < ApplicationController
  before_action :check_login, only: [:checkout]
  
  def show
  end

  def destroy
    session[:cart9527] = nil
    redirect_to products_path, notice:'購物車已經空'
  end

  def checkout
  end

  private
  def check_login
    redirect_to new_user_session_path, notice:'請先登入會員' unless user_signed_in?
  end
end
```

## ==設計訂單==
接著要做出訂單的 model
一個 user 有很多 Order 訂單有很多 OrderItem
```bash
#Order的部分
rails g model Order recipient phone address note:text status

#OrderItem的部分
rails g model OrderItem product:references quantity:integer order:references

rails db:migrate
```
```ruby
#order.rb
has_many :order_items
```
![](https://i.imgur.com/blWKLYD.png)
==寫測試==
```ruby
#order_spec.rb
#require 'rails_helper'
RSpec.describe Order, type: :model do
  it "訂單需要有使用者" do

  end

  it "收件人姓名、電話、地址需必填" do
    o1 = Order.new
    o2 = Order.new(recipient:'123', phone: '0910222222')
    o3 = Order.new(recipient:'456', phone: '0910222222', address:'地球')

    expect(o1.valid?).to be false   #valid是rails內建的方法
    expect(o2.valid?).to be false
    expect(o3.valid?).to be true
  end
end
```
```ruby
#order.rb
#加入
validates :recipient, :phone, :address, presence: true
```
在寫測試應該要分類清楚
```ruby
require 'rails_helper'

RSpec.describe Order, type: :model do
  it "訂單需要有使用者" do

  end

  describe "驗證收件人相關資料" do
    context "有效資料" do
      it "填寫完整資料" do
        o3 = Order.new(recipient:'456', phone: '0910222222', address:'地球')
        expect(o3.valid?).to be true
      end
    end

    context "無效資料" do
      it "未完整填寫資料" do
        o1 = Order.new
        o2 = Order.new(recipient:'123', phone: '0910222222')

        expect(o1.valid?).to be false   #valid是rails內建的方法
        expect(o2.valid?).to be false
      end
    end
  end

  # it "收件人姓名、電話、地址需必填" do
    
  # end
end

```

Q: 為什麼不用 .to raise_error 
A: 因為一般存資料只會 rollback 不會出錯，除非用 save! 所以使用 valid? 就可以不用存資料就可以知道會不會出錯

valid? 是 model 內建方法確認有沒有通過驗證

==再優化一下並用FactoryBot產出==
```ruby
#orders.rb
FactoryBot.define do
  factory :order do
    recipient { Faker::Name.name }
    phone { Faker::PhoneNumber.cell_phone } #查文件
    address { Faker::Address.full_address}  #查文件
    note { Faker::Lorem.sentences }
    status { "MyString" }

    trait :invalid do
      phone { nil } 
      address { nil }      
    end
  end
end

```
```ruby=
#order_spec.rb
RSpec.describe Order, type: :model do
 
  it "訂單要有使用者" do    
  end

  describe "驗證收件人相關資料" do
    
    context "有效資料" do
      it "填寫完整資料"   do
        # o3 = Order.new(recipient:'456', phone: '0910222222', address:'地球')
        # expect(o3.valid?).to be true
        order = build(:order)
        expect(order.valid?).to be true
        # expect(order).to be_valid
        
      end 
    end
    
    context "無效資料" do
      it "未填寫完整資料"   do
      # o1 = Order.new
        # o2 = Order.new(recipient:'123', phone: '0910222222')

        # expect(o1.valid?).to be false   #valid是rails內建的方法
        # expect(o2.valid?).to be false
        order = build(:order, :invalid)
        expect(order.valid?).to be false
        # expect(order).not_to be_valid
      end     
    end
    
  end
end
```
![](https://i.imgur.com/RAhHQXW.png)
![](https://i.imgur.com/FiLwbfM.png)
補充：


![](https://i.imgur.com/kgA7pla.png)

```bash
rails g migration add_ref_to_user 
```
![](https://i.imgur.com/94mHC7Q.png)

或偷懶版
```bash
rails g migration add_ref_to_order user:references
```
記得 rails db:migrate
```ruby
#order.rb 加入
belongs_to :user

#user.rb 加入
has_many :orders
```
建立關連後，跑測試就失敗了，因為User跟Order建立了關係，可是User沒有人。
![](https://i.imgur.com/QGlqw3O.png)

## ==用FactoryBot產出User==
```ruby
#在factory資料夾開
#users.rb
FactoryBot.define do
  factory :user do
    email { Faker::Internet.email }
    password {'123456'}
  end
end
```
![](https://i.imgur.com/1xRnr3x.png)


```ruby
#orders.rb
FactoryBot.define do
  factory :order do
    recipient { Faker::Name.name }
    phone { Faker::PhoneNumber.cell_phone } 
    address { Faker::Address.full_address}  
    note { Faker::Lorem.sentences }
    status { "MyString" }
    user { create(:user) } #多這行利用剛剛建立的 user factory

    trait :invalid do
      phone { nil } 
      address { nil }      
    end
  end
end

```
![](https://i.imgur.com/AJ7OKjy.png)

### 測試訂單需有使用者 13:58

```ruby
  describe "訂單使用者" do 
   #it "有使用者"do    #可以不用寫，下面已經有驗證了
   #end
    it "沒有使用者" do 
      order = build(:order, user:nil)
      expect(order).not_to be_valid
    end
  end
```
讓測試看起來更清楚
```
#.rspec
--require spec_helper
--format documentation  << Add this line
```
![](https://i.imgur.com/TFbRzxw.png)

'specify' & 'it' 其實是一樣的東西，但通常 specify 用在濃縮的時候

'specify' & 'it' (參考)
```ruby
# 簡化後的it "..." do...end
specify { expect(build(:order)).not_to be_valid }
```
```bash=
#    
$ RAILS_ENV=test rails db:redo
```

==結帳頁面加入：收件人、電話、地址、==
透過使用者角度create order
```erb
#carts_controller.rb
  def checkout
    @order = current_user.orders.build()
  end
  
#checkout.html.erb
<h1>結帳</h1>

<%= render 'cart'%>

<%= simple_form_for(@order) do |form| %>
  <%= form.input :recipient%>
  <%= form.input :phone%>
  <%= form.input :address%>
  <%= form.input :note%>

  <%= form.submit '建立訂單'%>
<% end %>
```

這裏會報錯，因為submit找不到路徑
>報錯的原因是，rails 看到你塞了 @order ， 它試著去找orders_path，但是發現找不到（目前只有Model Order，沒有route，也沒有Controller ）
>
![](https://i.imgur.com/OtD9mwa.png)


解法有幾種： 1.直接給他一個路徑
```
<%= simple_form_for(@order, url: "/") do |form| %>
```
但這樣有點奇怪
應該做一個正常路徑給他
```ruby
#routes.rb
resources :orders, only: [:create]
```

建立 orders_controller 14:57
```ruby
class OrdersController < ApplicationController

  def create
    #  建立訂單
    
  end

end
```
在Model的oders下的status加預設值
```bash
rails g migration add_default_status
```

```ruby
change_column_default(:orders, :status, 'pending')
```
![](https://i.imgur.com/iDlzvJP.png)

```ruby
#orders_controller.rb
  #建立訂單    
  def create
    
    @order = current_user.orders.build(order_params)
    
    current_cart.items.each do |item|
      @order.order_items << OrderItem.new(product: item.product, quantity: item.quantity)
    end
    #把購物車裡的東西拿出來，一條一條塞入order_items
    
    if @order.save
      # /orders/2/payment
      session[:cart9527] = nil #訂單成立後購物車要清空
      redirect_to  payment_order_path(@order), notice: '訂單已成立'
    else
      render 'carts/checkout'
    end
  end
 
  private
  def  order_params
    params.require(:order).permit(:recipient,:phone,:address,:note)
  end
```
```ruby
#checkout.html.erb
#改成
<%= render 'carts/cart'%>
#因為要連到其他 controller 的 view 一定要用絕對路徑
```
```ruby
#routes.rb
# /orders/2/payment
#新增路徑
  resources :orders, only: [:create] do
    member do
      get :payment
    end
  end
```
點下“建立訂單”後會出現no action 'payment'錯誤訊息
建立資料夾 views>orders
```erb
#建立pay.html.erb
<h1>付款</h1>
```
![](https://i.imgur.com/sdxaTQ6.png)


```erb
#orders_controller.rb
current_cart.items.each do |item|
  @order.order_items << OrderItem.new(prodcut: item.product, quantity: item.quantity)
end
```

## ==加"我的訂單"在登出旁邊 15:37==
```ruby
#routes.rb
#把index跟show打開
resources :orders, only: [:create, :index, :show] do
    member do
      get :payment
    end
end
```
```erb=
#_naver.html.erb
#上略
<% if user_signed_in? %>
    <li class="nav-item">
      #加入第6行 
      <%= link_to "我的訂單", orders_path, class: 'nav-link' %>
    </li>
    <li class="nav-item">
      <%= link_to "登出", destroy_user_session_path, class: 'nav-link', method: 'delete' %>
    </li>
#下略        
```
```erb
#建立index.html.erb
<h1>我的訂單</h1>
```
```ruby
 def index
    @orders = Order.where(user: current_cart)
    #或從使用者角度建立
    @orders = current_user.orders
 end
```
如果此時用無痕模式去localhost:3000/orders，資料就被看光了，所以要index要驗證，create也是，一個一個寫太麻煩，所以可以加入：
![](https://i.imgur.com/vESt86K.png)

```ruby
#orders_controller.rb
#加入
before_action :authenticate_user!
```

```htmlmixed=
#views>order>
#index.html.erb

<h1>我的訂單</h1>

<table class='table'>
<thead>
  <tr>
    <th>訂單編號</th>
    <th>日期</th>
    <th>狀態</th>
    <th>詳細內容</th>
  </tr>
</thead>
  <tbody>
    <% @orders.each do |order|%>
    <tr>
      <td><%= order.id%></td>
      <td><%= order.created_at%></td>
      <td><%= order.status%></td>
      <td><%= link_to "詳細內容", order, class:'btn btn-success btn-sm' %><td>
      #第21行的path完整寫法：order_path(@orders) 
      # 可以縮寫的只有 show update 和destroy
    </tr>
    <%end%>
  </tbody>
</table>
```
按照時間排序 order(created_at: :desc) 
```ruby
#orders_controller.rb
  def index
    # @orders = Order.where(user: current_cart)
    @orders = current_user.orders.order(created_at: :desc)  
    #最後的order是排序的order，跟我們訂單的order無關，只是名字剛好相同
  end
#下略
```
rails routes多的時候，把要看的部分區分開來可用以下指令：
```bash
rails routes | grep 'show'
#只會挑出show方法的route
```

## ==修正時間格式 16:00==
修正成亞洲時區(查文件)

![](https://i.imgur.com/CVclheo.png)

![](https://i.imgur.com/5krOsZ3.png)
```ruby
#config/application.rb
#加入
config.time_zone = 'Taipei'
```
要重開rails s才會生效
”詳細內容“龍哥說就交給我們自己寫了

# AASM
## ==訂單狀態 16:16==

狀態機：
透過動作去控制狀態

```bash
gem 'aasm', '~> 4.11'
```
用平常每天大家都會遇到的「門」當例子，狀態機就是「已經關上的門，只能打開不能關；已經打開的門，只能關不能打開」（廢話）；或是開車為例：「在開車打檔的時候，應該是要 1 檔、2 檔、3 檔依續往上打，而不應該是在 4 檔突然打 P 檔」。

在狀態機的模型下，狀態理論上只能順著我們設計的路線走，所以不會有「待處理」的訂單突然就變成「已到貨」狀態的情況發生。

https://github.com/aasm/aasm

```ruby
class Order < ApplicationRecord
  has_many :order_items
  validates :recipient, :phone, :address, presence: true
  belongs_to :user

  include AASM
  aasm(column: 'status') do 
    state :pending, initial: true
    state :paid, :delivered, :refunded, :cancelled
    
    event :pay do
      transitions from: :pending, to: :paid
    end

    event :cancel do
      transitions from: :pending, to: :cancelled
    end

    event :deliver do
      transitions from: :paid, to: :delivered 
    end

    event :refund do
      transitions from: [:delivered, :paid], to: :refunded
    end
  end

end

```
aasm 有送一些方法
1.在狀態後面加問號
```bahs
#rails console
# 狀態查詢 (用名詞)
o.pending? => true
o.paid? => false
```

```bash
#改變狀態 (用動詞)
o.pay! =>true
o.paid =>true
```
![](https://i.imgur.com/Wo8ahqs.png)

![](https://i.imgur.com/LMzjBM8.png)
2.在動作前面加 may 後面加問號
![](https://i.imgur.com/JeTuDkU.png)
```ruby
aasm(column: 'status',no_direct_assignment: true) do 
```
加了 no_direct_assignment: true 就不能直接改值

另外可以用 after 做一些其他事情
```ruby
    event :pay do
      transitions from: :pending, to: :paid

      after do
        puts "發送簡訊"
      end

    end
```

![](https://i.imgur.com/ljS0yeV.png)

做出不同狀態，可以跑出不同的按鈕
``` erb
<%= link_to "退貨", "#", class: 'btn btn-info btn-sm' if order.may_refund? %>
<%= link_to "付款", '#', class: 'btn btn-danger btn-sm' if order.may_pay? %>
```

![](https://i.imgur.com/5iMzLtU.png)

