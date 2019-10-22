---
title: Rails 190906 AstroCamp
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: Rails,Ruby
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}
# Rails 190906

覺得自己是單細胞生物...
覺得自己是個廢物...(對不起，我汙辱了廢物 ...)
單細胞這條路你們是不孤單的，因為我也是...
生まれて、すみません......（太宰治名言）
覺得三葉蟲都比我厲害......
你沒有看過安西飛人
教練我好想打球
john cena~~ (自帶音樂


CodeReview
1. 假刪除

```ruby
add_column :candidates, :deleted_at, :datetime
add_index :candidates, :deleted_at  #當作快取
```
```ruby
#candidate.rb
# 複寫方法，方便程式碼管理與使用

default_scope { where(deleted_at: nil)}

def destroy
  update(delete_at: Time.rb)
end
```
2.  只能投三票
![](https://i.imgur.com/89JAKWw.png)
exists? 判斷有沒有存在
```ruby
#vote_log.rb
validate :validate_vote

  private
  def validate_vote
    if VoteLog.exists?(candidate: candidate, user: user)
      errors[:候選人] << "已經投過了!"
    elsif user.vote_logs.count >= 3
      errors[:投票] << "不能超過 3 筆!"
    end
  end
```

```ruby
#candidate_controller.rb
def vote
    if user_signed_in?

      log = current_user.vote_logs.new(candidate: @candidate,
                                       ip_address: request.remote_ip)

      if log.save
        message = "投票成功"
      else
        message = log.errors.full_messages.first
      end

      redirect_to root_path, notice: message
    else
      redirect_to root_path, notice: '請先登入會員!'
    end
 end
```
3. 查詢log
```ruby
  def log
    @logs = @candidate.vote_logs.includes(:user)
    #include解決n+1問題，SQL語法會用IN
  end
```
```htmlmixed=
#log.html.erb
<h1>投票紀錄</h1>

<ul>
    <% @logs.each do |log| %>
      <li><%= log.user.email %></li>
    <% end %>
</ul>
```

==11:05 購物車開始(安裝套件)==
貼在 group :development, :test do
gem 'factory_bot_rails'
gem 'rspec-rails'
https://github.com/rspec/rspec-rails
```
rails generate rspec:install
```
minitest vs rspec
test vs spec
```
rails g 
可查詢可以建立哪些檔案
```
```
rails g rspec:model Cart
```


寫測試：3A原則


# PORO
Plain Old Ruby Object
AutoLoad Path


## Cart 基本功能
```ruby
#cart_spec.rb
require 'rails_helper'


RSpec.describe Cart, type: :model do
  describe "基本功能" do
    it "可以把商品丟到購物車裡，然後購物車裡就有東西了。"do
      cart = Cart.new
      expect(cart.empty?).to be true
      
      cart.add_item(1)
      expect(cart.empty?).to be false
     end
  end

  describe "進階功能" do
    it "加相同種類的商品，購買項目(CartItem)不會增加，但商品的數量會改變。" do
      cart = Cart.new
      3.times { cart.add_item(1)}
      2.times { cart.add_item(2)}
      2.times { cart.add_item(1)}

      expect(cart.items.count).to be 2
    end
  end
end


```
Model下建立一個cart.rb

```ruby
#app>model>cart.rb
#PORO
class Cart
  def initialize
    @items = []
  end

  def empty?
    @items.empty? #->此empty?是ruby的內建方法，只是剛好跟外面的empty?同名
  end

  def add_item(product_id)
    @items << product_id
  end
end

#rspec 執行會得到7，因為不管是不是同商品，都全部加入陣列裡了
```
Model  只是一個資料來源；
資料可以來自資料表或是由網路上的使用者所提供
購物車的Model沒有表格，下圖第二列。
![](https://i.imgur.com/ad6kukv.png)
![](https://i.imgur.com/bTfiPA0.png)
此find方法是ruby的方法，跟rails的ORM無關，只是方法名稱相同
(用法像map，後面接一個block)

```ruby=
#cart.rb
class Cart
  attr_reader :items

  def initialize
    @items = []
  end

  def empty?
    @items.empty?
  end

  def add_item(product_id)
    found_item = @items.find {|i| i.product_id == product_id}
    #這裡的每個i 都是CartItem.new建立出來的物件
    if found_item
      found_item.increment(1)
    else
      @items << CartItem.new(product_id)
    end
    # @items << product_id
  end
end

#@items是一長串資料
#開始修正錯誤: 
#1. 第14行的i.product_id的product_id會找不到，==product_id是參數傳進去的
#2. 第17行 undefine increment
#3. 第19行product_id會找不到
```

在Model下再建立一個cart_item.rb
```ruby
#Model>app>card_item.rb
class CartItem 
  attr_reader :product_id, :quantity

  def initialize(product_id, quantity = 1)
    @product_id = product_id
    @quantity = quantity
  end

  def increment(n = 1)
    @quantity += n
  end
end
```
#cart_spec.rb在加入以下程式碼也會測試成功！
```ruby
expect(cart.items.first.quantity).to be 5
```
14:20
.to be_a，為 type matcher，檢驗物件的類別，類似 ruby 的 .kind_of。
https://relishapp.com/rspec/rspec-expectations/v/3-8/docs/built-in-matchers/type-matchers
```ruby
#cart_spec.rb
#上略
#新增
    it "商品可以放到購物車裡，也可以再拿出來。"do
      cart = Cart.new

      p1 = Product.create(name:'aa', price: 100)
      p2 = Product.create(name:'bb', price: 50)

      3.times { cart.add_item(p1.id) }
      2.times { cart.add_item(p2.id) }

      expect(cart.items.first.product).to be_a Product
                  #陣列裡的第一個               #是一種商品
      expect(cart.items.first.product_id).to be p1.id
                                             #是商品p1 
    end
    
    #cart.items.class       得到Cart
    #cart.items.first.class 得到CartItem
    #Cart = [CartItem, CartItem, ....]
    
```
放入購物車，所以需要用到資料庫儲存
```
rails g model Product name price:integer description:text is_available:boolean
```
```ruby
#cart_item.rb
#上略(poro寫)
#新增(用Model的ORM寫)
  def product
    #回傳一個商品
    # Product.find(product_id)
    Product.find_by(id: @product_id)
  end
```
```ruby
# 這樣也能動是因為上面有set attr_reader :product_id
  def product
    Product.find_by(id: product_id)
  end
```
rspec下去會有pending，記得把product_spec.rb的 pending刪掉
![](https://i.imgur.com/02h62tY.png)

14:58下一段測試

```ruby
  it "每個 Cart Item 都可以計算它自己的金額（小計）。" do
      cart = Cart.new

      p1 = Product.create(name: 'aa', price: 100)
      p2 = Product.create(name: 'bb', price: 50)

      3.times {cart.add_item(p1.id)}
      2.times {cart.add_item(p2.id)}

      expect(cart.items.first.total_price). to be 300
      expect(cart.items.last.total_price). to be 100
    end
```

這裡的 product 會找到 CartItem 的 product 方法找到這個商品，然後去乘以數量
![](https://i.imgur.com/ezd9NVc.png)

```ruby
  def total_price
    product.price * quantity
  end
```
相關的測試，應該要去相關所屬的地方測，故給他一個cart_item_spec.rb檔
```bash
rails g rspec:model CartItem
```
```ruby
#cart_item_spec.rb
require 'rails_helper'

RSpec.describe CartItem, type: :model do
  it "每個Cart Item都可以計算它自己的金額(小計)。" do
      cart = Cart.new

      p1 = Product.create(name:'aa', price: 100)
      p2 = Product.create(name:'bb', price: 50)

      3.times { cart.add_item(p1.id) }
      2.times { cart.add_item(p2.id) }

      expect(cart.items.first.total_price).to be 300
      expect(cart.items.last.total_price).to be 100 
    end
end

```
MAC 好不習慣RRRRR
咁～一整天共筆就這句ＸＤ！！！？
一時寫code一時爽，一直寫code一直爽
```ruby
#cart_spec.rb
#上略
#新增
it "可以計算整台購物車的消費總額。" do
      cart = Cart.new

      p1 = Product.create(name:'aa', price: 100)
      p2 = Product.create(name:'bb', price: 50)

      3.times { cart.add_item(p1.id) }
      2.times { cart.add_item(p2.id) }

      expect(cart.total_price).to be 400
    end
```
```ruby
#cart.rb
#上略
#新增
  def total_price
    #這裏的total_price是cart_item.rb的方法
    #方法一(超爛)
    #@items[0].total_price + @items[1].total_price

    #方法二(爛)
    # total = 0
    # @items.each do |i|
    #   total += i.total_price
    # end
    # total

    #方法三(較好)
    @items.reduce(0){|sum, i|sum + i.total_price}
    
    #方法四(很有事的方法)(超有事)
    #@items.reduce([]){|sum, i|sum << i.total_price}.sum
    #
    #有人願意提供map 的寫法嗎？(87膩ＸＤ！？)
    #Well done
    #@items.map{|item|item.total_price}.sum
  end
```
Q: 這樣測試做出來的資料不是會變成很多重複的？
A: 不會，因為rspec測試的時候他的資料會寫到 db>migrate 裡面的 test.sqlite，不會存到真的資料庫

15:53 折扣方案
難在要時間暫停（下單時）
https://github.com/travisjeffery/timecop
```
gem 'timecop', '~> 0.9.1'
```
>License 分類
>- MIT << free for all
>- BSD << free for all
>- GPL << 污染性(你用了的話，開發套件的人有權跟你要原始碼)
>- Beer<< free for all
>

```ruby
#cart.rb
#上略
#新增
it "特別日期全面打9折" do
      # 2/10 打9折
      t = Time.local(2008, 2, 10, 10, 5, 0)
      Timecop.travel(t)
      
      cart = Cart.new

      p1 = Product.create(name:'aa', price: 100)
      p2 = Product.create(name:'bb', price: 50)

      3.times { cart.add_item(p1.id) }
      2.times { cart.add_item(p2.id) }

      expect(cart.total_price).to eq 360
    end
    #eq 比對內容物 be 比對Object_id
```
```ruby
#cart.rb
#上略
#新增
    def total_price
      total = @items.reduce(0){|sum, i|sum + i.total_price}
      if Time.now.day == 10 && Time.now.month == 2
        total = total * 0.9
      end
      total
    end
    
    #比較好的寫法
    
    def total_price
      total = @items.reduce(0){|sum, i|sum + i.total_price}
      if super_good_day?
        total = total * 0.9
      end
      total
    end

  private
   def super_good_day?
     Time.now.day == 10 && Time.now.month == 2
   end
```

16:39 整理程式碼

```ruby
#card_spec.rb
#寫在裡面
  RSpec.describe Cart, type: :model do

  before do
    @cart = Cart.new     #要把所有cart改成@cart(較麻煩)
  end
  
  #或

  let(:cart) { Cart.new }  #let是rspec提供的方法，可把 cart= Cart.new 全刪掉(較方便)

##下略
```
```ruby
expect(cart.empty?).to be true
#可寫成
expect(cart.empty?).to be_truthy
#或
expect(cart).to be_empty

#參考就好，炫技用
```

[Travis CI 教學](https://ithelp.ithome.com.tw/articles/10187405)

[Mockflow](https://www.mockflow.com/)


![](https://i.imgur.com/m4S97OL.png)
