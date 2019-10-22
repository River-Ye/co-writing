---
title: Rails 190923 AstroCamp
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: Rails, Ruby
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}
# Rails 190923

# Concern


專案資料夾，若 concerns 是空的，可以知道是初級工程師

早期只有 model 資料夾有

```ruby=
  module Profileable
    extend ActiveSupport::Concern # ::Concern 可能是模組或類別 
    #如果要有include do end及 module ClassMethods的話才要加第2行
      
    included do       
      #範例
      has_many :order_items
      belongs_to :user
      validates :recipient, :phone, :address, presence: true
    end
    #最後再你要有這功能的module裡include Profileable來用

    module ClassMethods #寫在這裡的方法就是類別方法
    end
  end
```

```ruby=
# model/cart.rb、order.rb
  def total_price_with_tax
    total_price * 1.05
  end
```
若有好幾個檔案都需要total_price_with_tax方法，可用concern的方式簡化。
(說穿了concern就是一個模組，有需要的時候就inclunde進來，跟partial render的概念有點像)

開一個檔案models>concern>taxable.rb
```ruby
#taxable.rb
module Taxable
  extend ActiveSupport::Concern

  included do   #記得要加d
  end

  def total_price_with_tax
    (total_price * 1.05).floor #無條件捨去
  end

  module ClassMethods 
  end
end

#model/cart.rb、order.rb
#加入
include Taxable
```

![](https://i.imgur.com/WkSG8Aj.png)
![](https://i.imgur.com/zCQpGLN.png)


[gem papertrail](https://github.com/paper-trail-gem/paper_trail)
[gem acts as paranoid](https://github.com/ActsAsParanoid/acts_as_paranoid)
11:14 controller也加concern

開一個檔案controller>concern>current_cart.rb

```ruby
#current_cart.rb
module CurrentCart
  extend ActiveSupport::Concern

  included do 
    helper_method :current_cart
  end

  module ClassMethods 
  end
  
  private
  def current_cart
    @cart = @cart || Cart.from_hash(session[:cart9527])
  end
end

#application_controller.rb
include CurrentCart
```

## Service Object 11:40

SRP(Single Resposibility Principle):一個東西只要專心做一件事情就好。

==為什麼要寫Service Object?==
A: 因為可以簡化流程及方便測試，目前專案因為太小所以感受不到，但大一點的專案其實很好用(正常工程師都在用)。

app下開新資料夾services，再開檔案braintree_payment.rb
```ruby
#braintree_payment.rb
class BraintreePayment
  
  def initialize(order: nil, nonce: '')   # keyword arguments

    @order, @nonce = order, nonce         #Parallel Assignment
  end
  
  def run
      result = braintree_gateway.transaction.sale(
        amount: @order.total_price,
        # payment_method_nonce: params[:payment_method_nonce],  #因為是polo，只有ruby的方法，故會讀不到rails的params
        payment_method_nonce: @nonce,
        options: {
          submit_for_settlement: true
        }
      )
  end
  
  private
  def braintree_gateway
    Braintree::Gateway.new(
      environment: :sandbox,
      merchant_id: ENV["braintree_merchant_id"],
      public_key: ENV["braintree_public_key"],
      private_key: ENV["braintree_private_key"]
    )
  end
end
```
```ruby
#orders_controller.rb
#改成
  def transaction
    if @order.may_pay?
      # result = braintree_gateway.transaction.sale(
      #   amount: @order.total_price,
      #   payment_method_nonce: params[:payment_method_nonce],
      #   options: {
      #     submit_for_settlement: true
      #   }
      # )
      result = BraintreePayment.new(order: @order, nonce: params[:payment_method_nonce]).run
#下略      
      
#private內的braintree_gateway可以拿掉

#def braintree_gateway
  #   Braintree::Gateway.new(
  #     environment: :sandbox,
  #     merchant_id: ENV["braintree_merchant_id"],
  #     public_key: ENV["braintree_public_key"],
  #     private_key: ENV["braintree_private_key"]
  #   )
  # end
```
可是這樣把braintree_gateway方法拿走的話
```ruby    
def payment
  @token = braintree_gateway.client_token.generate
end

#會壞掉，因為沒有braintree_gateway
```
所以寫個類別方法
(如果是clone下來的檔案，要自己新增config>application.yml檔並把key都輸入進去)
```ruby
#braintree_payment.rb
class BraintreePayment
  
  def initialize(order: nil, nonce: '')   # keyword arguments

    @order, @nonce = order, nonce         #Parallel Assignment
  end
  
  def self.generate_token
    braintree_gateway.client_token.generate
  end
  
  def run
      result = braintree_gateway.transaction.sale(
        amount: @order.total_price,
        # payment_method_nonce: params[:payment_method_nonce],  #因為是polo，只有ruby的方法，故會讀不到rails的params
        payment_method_nonce: @nonce,
        options: {
          submit_for_settlement: true
        }
      )
  end
  
  private
  def braintree_gateway
    Braintree::Gateway.new(
      environment: :sandbox,
      merchant_id: ENV["braintree_merchant_id"],
      public_key: ENV["braintree_public_key"],
      private_key: ENV["braintree_private_key"]
    )
  end
end

#orders_controller.rb

def payment
    # @token = braintree_gateway.client_token.generate
    @token = BraintreePayment.new.generate_token
end
```

https://www.toptal.com/ruby-on-rails/rails-service-objects-tutorial
[Skinny Models, Skinny Controllers, Fat Services
](https://medium.com/marmolabs/skinny-models-skinny-controllers-fat-services-e04cfe2d6ae)

13:50-14:40 講解
權限控管 Gem: cancancan / pundit
```
gem "pundit"
```
```bash
rails g pundit:install
#建立application_policy.rb
```
```ruby
#application_controller.rb
class ApplicationController < ActionController::Base
  include Pundit
end
```
想在user的table建立一個使用者
```bash
rails g migration add_role_to_user role
```
預設值給 default: 'user'

```ruby
#policies>#candidate_policy.rb
class PostPolicy
  attr_reader :user, :candidate

  def initialize(user, candidate)
    @user = user
    @candidate = candidate
  end

  def new?
    user.role == 'admin'
  end

  def update?
    # user.admin? or not candidate.published?
  end
end
```
這會找 candidate policy 裡面跟 new 方法相關的設定
```ruby
#candidates_controller.rb
  def new
    @candidate = Candidate.new
    authorize @candidate
  end
```
```ruby
#application_controller.rb
rescue_from Pundit::NotAuthorizedError, with: :not_authorize
private

def not_authorize
    redirect_to root_path, notcie: "權限不足"
end
```
```ruby
#candidate_controller.rb
before_action :authenticate_user!, only: [:new, :create, :edit, :update, :destroy]
```

```erb
<%= link_to '新增候選人', new_candidate_path, class: 'btn btn-primary btn-lg' if user_signed_in? && policy(@candidates).new? %>
```

14:45 rails self reference

範例：
```ruby
class Follow < ApplicationRecord
  belongs_to :follower, class_name: 'User'
  belongs_to :followee, class_name: 'User'
end
```

14:50 做簡易聊天室
```bash
rails g model User nickname email
rails g model Chatroom title description:text
rails g model Session user:references chatroom:references
```
```ruby
class User < ApplicationRecord
  has_many :sessions
  has_many :chatrooms, through: :sessions
end

class Chatroom < ApplicationRecord
  has_many :sessions
  has_many :users, through: :sessions
end

class Session < ApplicationRecord
  belongs_to :user
  belongs_to :chatroom
end
```

```
 rails g migration add_adim_to_chatroom adim:integer
```
```ruby
class AddAdimToChatroom < ActiveRecord::Migration[5.2]
  def change
    add_column :chatrooms, :adim_id, :integer
  end
end
```
```ruby
class User < ApplicationRecord
  has_many :sessions
  has_many :chatrooms, through: :sessions

  def join_room(room)
    chatrooms << room
  end

  #u1.create_room('aa')
  def create_room(title)
    room = Chatroom.create(title: title, admin: self)
    join_room(room)
  end
end


class Chatroom < ApplicationRecord
  validate :title, presence: true

  has_many :sessions
  has_many :users, through: :sessions

  belongs_to :admin, class_name: 'User'
end
```
gem:uuidtools

## 15:26重構 Refacory
### Before:
```ruby
class Movie
  REGULAR     = 0     # 普通片
  NEW_RELEASE = 1     # 新片
  CHILDRENS   = 2     # 兒童片

  attr_reader :title
  attr_accessor :price_code

  def initialize(title, price_code)
    @title, @price_code = title, price_code
  end
end
```
```ruby
class Rental
  attr_reader :movie, :days_rented

  def initialize(movie, days_rented)
    @movie, @days_rented = movie, days_rented
  end
end
```
```ruby
class Customer
  attr_reader :name

  def initialize(name)
    @name    = name  # 姓名
    @rentals = []    # 租借紀錄
  end

  def add_rental(arg)
    @rentals << arg
  end

  def statement
    total_amount = 0            # 總消費金額
    frequent_renter_points = 0  # 常客積點
    result = "Rental Record for #{@name}\n"

    @rentals.each do |element|
      this_amount = 0
      # determine amounts for each line
      case element.movie.price_code
      when Movie::REGULAR
        this_amount += 2
        this_amount += (element.days_rented - 2) * 1.5 if element.days_rented > 2
      when Movie::NEW_RELEASE
        this_amount += element.days_rented * 3
      when Movie::CHILDRENS
        this_amount += 1.5
        this_amount += (element.days_rented - 3) * 1.5 if element.days_rented > 3
      end

      # 累加常客積點
      frequent_renter_points += 1
      # 如果是新片而且租超過 1 天，另外加 1 點
      if element.movie.price_code == Movie::NEW_RELEASE && element.days_rented > 1
        frequent_renter_points += 1
      end

      # 顯示此筆租借資料
      result += "\t" + element.movie.title + "\t" + this_amount.to_s + "\n"
      total_amount += this_amount
    end

    # 結尾列印
    result += "Amount owed is #{total_amount}\n"
    result += "You earned #{frequent_renter_points} frequent renter points"
    result
  end
end
```

```ruby
require './lib/customer'
require './lib/movie'
require './lib/rental'

client = Customer.new('eddie')

movie1 = Movie.new('ruby', Movie::NEW_RELEASE)
rental1 = Rental.new(movie1, 3)
client.add_rental rental1

movie2 = Movie.new('php', Movie::REGULAR)
rental2 = Rental.new(movie2, 7)
client.add_rental rental2

puts client.statement
```

開始重構
```ruby
#customer.rb
#把上面剪下來
private

def amount_for(element)  #加上參數element
  this_amout = 0    #加上這行
  case element.movie.price_code
      when Movie::REGULAR
        this_amount += 2
        this_amount += (element.days_rented - 2) * 1.5 if element.days_rented > 2
      when Movie::NEW_RELEASE
        this_amount += element.days_rented * 3
      when Movie::CHILDRENS
        this_amount += 1.5
        this_amount += (element.days_rented - 3) * 1.5 if element.days_rented > 3
  end
end

#上面
@rentals.each do |element|
  this_amount = amount_for(element)
#下略...  
```

因為each do |element|的名字命名不好，所以把名字都改成|rental|

後面筆記沒做了...詳情請見龍哥的github
[龍哥的refactoring_demo](https://github.com/kaochenlong/code_refactoring_demo)

16:00 投票搭配js.erb (ajax)
16:24 rack mini profiler

就這樣結束課程惹....
