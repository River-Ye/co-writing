---
title: Rails 190918 AstroCamp
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: Rails, Ruby
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}
# Rails 190918

今日名言：從哪裡跌倒，就在哪裡躺好。

```bash
rails generate devise:views
```
10:38 會員加欄位

```htmlmixed
#_navbar.html.erb
#在登出旁邊加‘個人資料’
<li class="nav-item">
          <%= link_to "個人資料", edit_user_registration_path, class: 'nav-link' %>
        </li>
```

```htmlmixed
<!-- #views>devise>registration>edit.html.erb -->
<div class="form-actions">
    <%= f.button :submit, "Update", class: "btn btn-success btn-lg" %>
</div>
```
把標題改掉
把取消個人資料的部分拿掉
![](https://i.imgur.com/E4Tfqhm.png)

在user做個可以更改名字的欄位
```bash
rails g migration add_name_to_user name
```
記得rails db:migrate
migration 會根據 add ... to ... user 加上以下欄位
```ruby
class AddNameToUser < ActiveRecord::Migration[5.2]
  def change
    add_column :users, :name, :string
  end
end
```

![](https://i.imgur.com/AAkOxyq.png)


```erb
    <div class="row">
      <div class="col">
        <%= f.input :password,
                hint: "leave it blank if you don't want to change it",
                required: false,
                input_html: { autocomplete: "new-password" } %>
      </div>
      <div class="col">
        <%= f.input :password_confirmation,
            required: false,
            input_html: { autocomplete: "new-password" } %>
      </div>
    </div>
```
```htmlembedded=
<!-- #views>devise>registration>edit.html.erb -->
 <div class="row">
    <div class="col-4">
    <%= f.input :name, label: '名字' %>
    </div>
    <div class="col-8">
    <%= f.input :email, required: true, autofocus: true %>
    </div>
  </div>
#       ----中略----
<div class="row">
      <div class="col">
      <%= f.input :password,
                  label:'密碼',
                  hint: "leave it blank if you don't want to change it",
                  required: false,
                  input_html: { autocomplete: "new-password" } %>
        
      </div>
      <div class="col">
      <%= f.input :password_confirmation,
                  label:'密碼確認',
                  required: false,
                  input_html: { autocomplete: "new-password" } %>
        
      </div>
</div>
    <%= f.input :current_password,
                hint: "we need your current password to confirm your changes",
                required: true,
                input_html: { autocomplete: "current-password" } %>
    </div>
    </div>
```
用rails g 看看能產生什麼 看到 devise:controllers
```
rails g devise:controllers users
```
![](https://i.imgur.com/WjQd7gs.png)


```ruby
#routes.rb
devise_for :users, controllers: {
        registrations: 'users/registrations'
      }
```
把 registration controller 裡面的東西解開

```ruby
  #解開束縛！！
 before_action :configure_account_update_params, only: [:update]
  private #第4行
  # If you have extra params to permit, append them to the sanitizer.
  # def configure_sign_up_params
  #   devise_parameter_sanitizer.permit(:sign_up, keys: [:attribute])
  # end

  # If you have extra params to permit, append them to the sanitizer.
  def configure_account_update_params #第49行
    devise_parameter_sanitizer.permit(:account_update, keys: [:name])
  end
```
因為每次更新都要打current_password很麻煩，所以把它拿掉
```ruby
<%= f.input :current_password,
    hint: "we need your current password to confirm your changes",
    required: true,
    input_html: { autocomplete: "current-password" },
    label: "Your current password" %>
```
查文件，關鍵字'devise update user without current password'
```ruby
#registrations_controller.rb
#貼在private底下
private
def update_resource(resource, params)
    resource.update_without_password(params)
end
```
# 11:45 放大頭照 Active Storage
rails內建[Active Storage](https://5xruby.tw/posts/active-storage-review/): 上傳檔案的功能
https://edgeguides.rubyonrails.org/active_storage_overview.html
![](https://i.imgur.com/0ABn8wd.png)
```ruby
#user.rb
class User < ApplicationRecord
  has_one_attached :avatar #加入
end
```
```bash
rails active_storage:install
rails db:migrate
```

[Active Storage 開箱文](https://5xruby.tw/posts/active-storage-review/)

```htmlmixed
<!-- edit.html.erb -->
 <div class="form-inputs">
    <%= f.input :avatar, label: '照片' %>

  <div class="row">
    <div class="col-4"><%= f.input :name, label: '名字' %></div>
    <div class="col-8"><%= f.input :email, required: true, autofocus: true %></div>
  </div>
```

```ruby
#registrations_controller.rb
#記得要permit avatar
  def configure_account_update_params
    devise_parameter_sanitizer.permit(:account_update, keys: [:name, :avatar])  
  end
```
把照片 show 出來
```erb
<!-- edit.html.erb  -->
<%= image_tag current_user.avatar %>
```
這樣寫會出錯，因為在還沒上傳照片時，image_tag會是nil，會噴錯誤訊息
故要寫if else判斷
```erb
<%= image_tag current_user.avatar if current_user.avatar.attached? %>
```
備註：上傳的圖片會存到storage資料夾裡去
在這邊如果只接用 CSS 縮小，檔案不會變小，只是看起來變小，所以應該用 resize

12:10 resize
安裝套件 [mini_magick](https://rubygems.org/gems/mini_magick/versions/4.5.1?locale=zh-TW)

Gemfile
```
gem 'mini_magick', '~> 4.5', '>= 4.5.1'
```
改成
```erb
<%= image_tag current_user.avatar.variant(resize: '400x400') if current_user.avatar.attached? %>
```
```
brew install imagemagick
```

讓照片出現在個人資料旁邊
超連結的變形： 在 link_to 最後加上 do...end，把內容放在中間
```htmlmixed
<!-- _navbar.html.erb -->
<!-- 加在個人資料旁邊 -->
<li class="nav-item">
  <%= link_to edit_user_registration_path, class: 'nav-link' do%>
  <%= image_tag current_user.avatar.variant(resize: '40x40'), class: 'avatar nav-avater' if current_user.avatar.attached? %>個人資料
  <%end%>
</li>
```
因為縮太小的話，會有馬賽克出現，所以縮到一定大小後，再用css調整就好
```htmlmixed
<!-- application.scss -->
.avatar {
  border-radius: 50%;
  border: 2px solid black;
}

.nav-avatar{
  width: 30px;
  height: 30px;
}
```
寫到application_helper.rb簡化程式碼
使用 keyword argument 的方法(方法的引數可以用鍵值對)
```ruby
#application_helper.rb
#module ApplicationHelper
  def avatar_tag(user, class_name: 'avatar', size: '300x300') #這裡的class_name: size: 會用冒號是因為他是hash
    if user.avatar.attached?
      image_tag current_user.avatar.variant(resize: size), class: 'avatar nav-avater' if current_user.avatar.attached?    
    else
      image_tag "#{Rails.root}/public/pictures/default_avatar.jpg", class: class_name    #如果沒有上傳圖就給他預設圖
    end
  end
end
```
_navbar.html.erb裡改成
```htmlmixed
<!-- _navbar.html.erb -->
<li class="nav-item">
  <%= link_to edit_user_registration_path, class: 'nav-link' do%>
  <%= avatar_tag(current_user, size: '100x100', class_name: 'avatat nav-avatar') %>個人資料
  <%end%>
</li>

<!-- edit.html.erb -->
<!--改掉 <%= image_tag current_user.avatar.variant(resize: '400x400') if current_user.avatar.attached? %> -->
<%= avatar_tag(current_user, size: '400x400', class_name: 'avatar nav-avatar') %>
```

如果要傳不定參數的話
一個星號是陣列，兩個星號是 Hash
```ruby
#較舊式寫法 keyword argument
def hello (name ='123', **options)
    p name
    p options[:age]
    p options[:color]
end

hello('kk', age: 18, color: 'black')
# => "kk"
#    18
#    "black"
#
    
#較新式寫法
def hello (name ='123', age:18 ,color:'black') #如果冒號後面沒寫代表沒給預設值
    p name
    p age
    p color
end

hello('kk', age: 18, color: 'black')
# => "kk"
#    18
#    "black"
```

13:54 訂單完成後寄信給使用者
## rails內建的Action Mailer
```bash
rails g mailer Order
```
![](https://i.imgur.com/rZWe3eP.png)

```ruby
#order_mailer.rb
def confirm
    mail to: 'hungyichiu@gmail.com', subject: "訂單#{123}已付款"
end
```
```ruby
#orders_controller.rb
def transaction
#下略
  if result.success?  #手冊內建方法
        @order.pay!
        #寄 email
        OrderMailer.confirm_email #加在這裡
#下略
end
```
龍哥先把OrderMailer放在show，為了方便測試而已，不然這樣寫不太好，
總不會想每次看show都會顯示訂單已完成吧

 - 建立檔案 views/order_mailer/confirm.html.erb

```ruby
#orders_controller.rb
def show
  #寄 email
  OrderMailer.confirm_email.deliver_later
  #OrderMailer是類別，卻可以用實體方法confirm_email做？這是mailer幫忙做的事
end
```
這時候在 log 可以看到
![](https://i.imgur.com/r6Zq6Ej.png)

```ruby
def show
  #寄 email
  OrderMailer.confirm_email(@order).deliver_later
end

#order_mailer.rb
class OrderMailer < ApplicationMailer
  def confirm_email(order)
    mail to:order.user.eamil, subject: "訂單[#{order.slug}]已確認"
  end
end
```
到order的show頁面重新整理後，去確認 log 就可以看到 subject 中間有夾雜訂單號碼
OrderMailer 就跟 controller 很像，views 裡面也會有同樣名字的資料夾，裡面要放跟 action 同樣名字的 html.erb 檔
![](https://i.imgur.com/2mojexR.png)
![](https://i.imgur.com/Yvnfm60.png)
14:27 mailer.html.erb的body裡面放東西
```htmlmixed
<!-- confirm_email.html.erb -->
hi,<%= @order.user.email %>

<!-- order_mailer.rb -->
class OrderMailer < ApplicationMailer
  def confirm_email(order)
    @order = order      #加入這行
    mail(to:order.user.email, subject: "訂單#{order.slug}已確認")
  end
end
```
可以看到 @order 成功傳進來
![](https://i.imgur.com/C90taU0.png)

另一個方法：

```ruby
 def show 
    # OrderMailer.confirm_email(@order).deliver_later
    OrderMailer.with(order: @order).confirm_email.deliver_later 
  end
```

```ruby
#order_preview.rb
class OrderPreview < ActionMailer::Preview
  def confirm_email
    OrderMailer.with(order: @order).confirm_email.deliver_later 
  end
end
```

```ruby
#order_mailer.rb 
def confirm_email  #參數拿掉
    @order = params[:order] 
    mail to: @order.user.email, subject: "訂單[#{@order.slug}]已付款"
  end
```

```ruby=
# order_controller.rb
def show
  # 方法1
  # OrderMailer.confirm_email(@order).deliver_later
    
  # 方法2
  OrderMailer.with(order: @order).confirm_email.deliver_later
end


# ---------------------------------------------------------


# order_mailer.rb
class OrderMailer < ApplicationMailer
  # 方法1
  # def confirm_email(order)
  #   @order = order
  #   mail to: order.user.email , subject: "訂單 [#{order.slug.upcase}] 已付款"
  # end


  # 方法2
def confirm_email
  @order = params[:order]
  mail to: @order.user.email , subject: "訂單 [#{@order.slug.upcase}] 已付款"
end
end
```

14:38 每次都用log看很痛苦

localhost:3000/rails/mailers
![](https://i.imgur.com/aSPDPc2.png)

先全部寫死
```htmlmixed=
<!-- order_mailer.rb -->
class OrderMailer < ApplicationMailer
  def confirm_email
    # @order = order
    # @order = params[:order]
    # mail to:@order.user.email, subject: "訂單#{@order.slug}已確認"
    mail(to: '123@gmail.com', subject: "確認訂單")
  end
end

<!-- order_preview.rb -->
# Preview all emails at http://localhost:3000/rails/mailers/order
class OrderPreview < ActionMailer::Preview
  def confirm_email
    # OrderMailer.with(order: @order).confirm_email.deliver_later 
    OrderMailer.confirm_email
  end
end

<!-- confirm_email.html.erb -->
hi,world
```
![](https://i.imgur.com/frFw5Or.png)

```htmlmixed=
<!-- order_mailer.rb -->
class OrderMailer < ApplicationMailer
  def confirm_email
    @order = params[:order]
    mail to:@order.user.email, subject: "訂單#{@order.slug}已確認"
  end
end

<!-- order_preview.rb -->

class OrderPreview < ActionMailer::Preview
  def confirm_email
    order = FactoryBot.create(:order)
    OrderMailer.with(order: @order).confirm_email
  end
end

<!-- confirm_email.html.erb -->
<h1>hi,你好</h1>

<p>您的訂單編號為：<%= @order.slug.upcase %></p>
```
![](https://i.imgur.com/mjorEwq.png)
因為狀態機關起來了，所以先把狀態關掉
```ruby
#orders.rb
FactoryBot.define do
  factory :order do
    recipient { Faker::Name.name }
    note { Faker::Lorem.sentences }
    phone { Faker::PhoneNumber.cell_phone }
    address { Faker::Address.full_address }
    # status { "pending" }  #關起來
    user { create(:user) }

    trait :invalid do
      phone { nil }
      address { nil }
    end
  end
end
```

15:10 
想把 @order.user.email 縮短成 @order.email
方法ㄧ：
```ruby
#order_mailer.rb
def confirm_email
    @order = params[:order]
    mail to: @order.email, subject: "訂單[#{@order.slug.upcase}]已付款"
end
```  
在 Order model 裡面加上
```ruby
#order.rb
def email
    user.email
end
```
方法二：
key word: rails delegate
要問跟 user 相關的資料就推給 user
```
#order.rb

delegate :email, to: :user
```

15:23 真的把信寄出去
```ruby
#development.rb
#加在最下面
  config.action_mailer.smtp_settings = {
  address: ENV['smtp_address'],
  port: 587,
  domain: '5xruby.tw',
  user_name: ENV['smtp_username'],
  password: ENV['smtp_password'],
  authentication: 'plain',
  enable_starttls_auto: true
}
```

## use [mailgun](https://www.mailgun.com/)

但不應該把重要資訊放這邊，應該放在 figaro 裡面
```yml
#application.yml
smtp_address: 'smtp.mailgun.org'
smtp_username: 'postmaster@sandbox230dd8f72948443988ec23a4339b9745.mailgun.org'
smtp_password: 'c9bb4f1e331916fe7c6edc8bb7377f7b-2b0eef4c-ed9cda11'
```
```htmlmixed
#order_mailer.rb
def confirm_email
    @order = params[:order]
    mail to: @order.email, subject: "訂單[#{@order.slug.upcase}]已付款"
    #寄信給自己，把to後面改成自己的信箱
end

#application_mailer.rb
class ApplicationMailer < ActionMailer::Base
  default from: 'eddie@5xruby.com' #從這裡寄出
  layout 'mailer'
end
```  
再頁面重新整理就可以寄出了
(龍哥的mailgun的domain怪怪的，再自己註冊帳號->overview->Add domain後去找username跟password吧)
![](https://i.imgur.com/CfJwkNa.png)


把寄信這工作包起來晚點做
為了避免使用者以為是網頁壞掉，網頁會先把這個工作存起來，馬上先進到下一個工作，待會在做寄信這種 loading 重的工作

[Active Job](https://edgeguides.rubyonrails.org/active_job_basics.html)背景工作，讓比較複雜的工作在後台先做並先存起來
```
rails g job postman->自己命名
```
[sidekiq有空可看看](https://ihower.tw/rails/actionmailer.html)


```ruby
#postman_job.rb
class PostmanJob < ApplicationJob
  queue_as :default

  def perform(*args)
    OrderMailer.with(order: @order).confirm_email.deliver_later 
  end
end

```
```ruby
#orders_controller.rb
def transaction
  #---略---   
  if result.success?
    @order.pay!
    PostmanJob.set(wait: 10.seconds).perform_later(@order)
  #---略--- 
end
```