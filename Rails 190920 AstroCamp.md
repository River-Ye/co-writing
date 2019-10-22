---
title: Rails 190920 AstroCamp
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: Ruby, Rails 
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}
 # Rails 190920
 
https://www.accupass.com/event/1908260559443278663890
記得報名 008JS 的活動，我靠你們達成業績了！


- Why BasicObject
  - 不需要Kernal  裡的方法，節省記憶體。
  - 繼承BasicObject，定義出自己的繼承體系，而不用擔心太多方法名稱衝突問題。 from [BasicObject]("https://openhome.cc/Gossip/Ruby/BasicObject.html")

可以用 undef 把繼承的方法取消掉
```ruby
class Cat
  undef :display
end
```

所有的類別包括 Class 本身都是 Class 生出來的
```ruby
class Cat
end

Cat = Class.new
```

- 為什麼 Ruby 的常數可以修改？
    - 如果常數不能被修改，類別將無法被擴充。為了維持openclass的設計
    - [Open Class]("https://railsbook.tw/chapters/08-ruby-basic-4.html")

```ruby
a = [1,2,3,4,5]
b = a
a[0] = "x"

p b #["x",2,3,4,5]
```
為什麼會有這樣的結果？
因為上面的意思是 a 標籤指向 [1,2,3,4,5] 這個記憶體空間
b 標籤也指向同一個地方
所以如果改了其中一個東西，連 b 的內容也會改 (因為是改這個記憶體位置中的內容)

```ruby
Cat = [1,2,3,4,5]
Cat[0] = 'x'
```
基於上面的理由，這樣子的內容不會發生錯誤
```ruby
Cat = 1
Cat = 2
```
但這樣子就不被允許，因為記憶體空間從 1 的位置改成指向 2 的位置


![](https://i.imgur.com/D5rqPz0.png)

```ruby
#class Cat
#end
# Cat = Class.new
Cat = []#^^^等同於
```


```ruby
a = [1, 2, 3, 4, 5]
b = a.dup
a[0] = 'x'
p a # ["x", 2, 3, 4, 5]
p b # [1, 2, 3, 4, 5]

# 因為使用dup複製，所以會形成另一個不同的object
```
因為傳進來的方法是 pass by reference，所以傳進來的是這個記憶體位置，會直接改到內容


![](https://i.imgur.com/gVEfYgz.png)
```ruby
# pass by reference
# pass by value

def kill(studnets)
  studnets = studnets.dup # pass by value <<加上這句
  a[1] = nil # pass by reference
end

a = [1,2,3,4,5]
kill(a)
```

![](https://www.mathwarehouse.com/programming/images/pass-by-reference-vs-pass-by-value-animation.gif)

ancestors：找尋方法的路徑
```ruby
 module Flyable
 end
 
 class Animal
 end
 
 class Cat < Animal
 end
include Flyable
  p Cat.ancestors
# 印出 [Cat, Flyable, Animal, Object, Kernel, BasicObject]
```

動態定義方法：需要的時候再定義就好
```ruby
  1.upto(10) { |i|
  
  define_method "x#{i}" do |message|
    puts "xxx#{i} - #{message}"
  end
  
  }
  
  x1('kk') # => xxx1 - kk
  x2('yy') # => xxx2 - yy
  x3('zz') # => xxx3 - zz
```

限定 Rails
![](https://i.imgur.com/h6FdOwH.png)
![](https://i.imgur.com/aqzriMw.png)


```ruby
class Cat
  class << self
    def a
    end
    def b
    end
    def c
    end
  end
end

```

# 13:51 rails nested form(巢狀表單)
一個候選人有多個簽名檔
```bash
  rails g model Signature candidate:references content active:boolean
```

```ruby
#migration_file
class CreateSignatures < ActiveRecord::Migration[5.2]
  def change
    create_table :signatures do |t|
      t.references :candidate, foreign_key: true
      t.string :content
      t.boolean :active, default: false  #加上預設值false

      t.timestamps
    end
  end
end

```
```ruby
#candidate.rb
has_one :signature #加入
accepts_nested_attributes_for :signature #查文件

#_form.html.erb
<hr>
<%= form.input :signature, label: '簽名檔' %> #加入
#但這樣寫會出錯

#改成

#simple_form寫法(寫在block裡面)
<%= form.simple_fields_for :signature do |signature_form| %>
  <%= signature_form.input :content, label: '簽名檔' %>
<%end%>

#一般form寫法
<% fields_for :signature do |signature_form| %>
  <%= signature_form.label :content, '簽名檔' %>
  <%= signature_form.text_field :content %>
<%end%>
```

這裏的 :signature 是虛擬欄位has_one給他的

```ruby
#candidates_controller.rb
def edit
    @candidate.build_signature if @candidate.signature.empty?
end

def candidate_params
# candidate[signature_attributes][content]
    params.require(:candidate).permit(:name,
                                       :age, 
                                       :policy, 
                                       :party, 
                                       :degree,
                                       signature_attributes: [:id, :content, :active])
end
```
14:50
product增加編輯欄位，新增庫存：動態model
```htmlmixed
#products_controller.rb
  before_action :find_product, only: [:show, :add_to_cart, :edit, :update]
    
  def edit
  end

  def update
    if @product.update(product_params)
      redirect_to products_path, notice: '商品更新完成'
    else
      render :edit
    end
  end
  
#index.html.erb
#加入

<%= link_to "編輯", edit_product_path(product), class: 'btn btn-info btn-sm' %>  
  
#新增_form.html.erb
<%= simple_form_for(product) do |form| %>
  <%= form.input :name %>
  <%= form.input :price %>
  <%= form.input :description %>
  <%= form.input :is_available %>
  <%= form.submit '更新', class: 'btn btn-lg btn-info'%>
<% end %>  

#edit.html.erb
<h1>更新商品</h1>
<%= render 'form', product: @product %>

```
```bash
rails g model Sku product:references quantity:integer spec
```
```htmlmixed
#product.rb
class Product < ApplicationRecord
  has_many :skus
  accepts_nested_attributes_for :skus
end

#products_controller.rb
def edit
    @product.skus.build if @product.skus.empty?
end


#_form.html.erb
  <h2>庫存</h2>
  <%= form.simple_fields_for :skus do |sku_form| %>
    <div class="row">
      <div class="col-8">
        <%= sku_form.input :spec, label: false, placeholder: '規格：XL、 L、XS' %>
      </div>
      <div class="col-4">
        <%= sku_form.input :quantity, label: false, placeholder: '數量' %>  
      </div>
    </div>
  <%end%>
```

```ruby
#products controller
  def edit
    @product.skus.build if @product.skus.empty?
  end
  
  def product_params
    params.require(:product).permit(:name, :price, :description, :is_available,
    skus_attributes: [:id, :spec, :quantity])
  end
```
15:22 [cocoon套件](https://github.com/nathanvda/cocoon):按下新增表單會再跳出form可以填寫

```htmlmixed
#_form.html.erb
 <h2>
  庫存 <%= link_to "新增庫存", '#', class: 'btn btn-sm btn-danger' %>
 </h2>
```
[jquery_rails](https://github.com/rails/jquery-rails)
```
gem 'cocoon', '~> 1.2', '>= 1.2.9'
gem 'jquery-rails', '~> 4.1', '>= 4.1.1'
```
```javascript
// application.js
// 貼上
// 
//= require jquery
//= require cocoon
//
// jquery 需要在 cocoon 前 require 進來，所以要擺在 cocoon 上面
```

```htmlmixed
#product.rb
accepts_nested_attributes_for :skus, allow_destroy: true

#product_controller.rb
  private
  def product_params
    params.require(:product).permit(:name, :price, :description, :is_available,
    skus_attributes: [:id, :spec, :quantity, :_destroy])
  end
```
```htmlmixed
#_form.html.erb

#改掉link_to
<%= simple_form_for(product) do |form| %>
  <%= form.input :name %>
  <%= form.input :price %>
  <%= form.input :description %>
  <%= form.input :is_available %>
  <hr>
  <h2>
    庫存
  </h2>
  <%= form.simple_fields_for :skus do |sku_form| %>
    <%= render 'sku_fields', f: sku_form %>
  <% end %>
  <div class="links">
    <%= link_to_add_association "新增庫存", form, :skus, class: 'btn btn-sm btn-danger' %>
  </div>
  <hr>
  <%= form.submit class: 'btn btn-success' %>
<% end %>
```
查文件
![](https://i.imgur.com/QW5ht9P.png)
```htmlmixed
#views>products>建立一個檔案_sku_fields.html.erb
<div class="nested-fields">
  <div class="row">
    <div class="col-8">
      <%= f.input :spec, label: false, placeholder: '規格：XL、L、大、中、小' %>
    </div>
    <div class="col-3">
      <%= f.input :quantity, label: false, placeholder: '數量' %>
    </div>
    <div class="col-1">
      <%= link_to_remove_association "X", f, class: 'btn btn-sm btn-danger' %>
    </div>
  </div>
</div>
```

# Rails Admin 16:08 (後台)
```bash
rails g model Candidate name party age:integer policy:text
```
https://github.com/sferik/rails_admin
```
gem 'rails_admin', '~> 2.0'
bundle install
rails g rails_admin:install
```
localhost:3000/admin
![](https://i.imgur.com/1HUILbM.png) 
可以自己改名字，才不會那麼容易被猜到
![](https://i.imgur.com/HFksVFj.png)