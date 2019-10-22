---
title: Rails 190830 AstroCamp
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: Rails, Ruby 
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}

用迴圈建立多筆Owner資料
10.times { |n| Owner.create(name: "o#{n}"),(email:"o#{n}@cc.cc")}
## C
==create 和 create! 的差別==
create  只會顯示false
create! 會噴錯誤例外訊息（Exception）跟錯誤(error)

## R
==find(1) 跟 find_by(id: 1) 這兩種寫法有什麼不同?==
find(1) 只能找:id是1的資料
find_by(id: 1) id:可以改成要找的東西(不限id:)
例如: find_by(name: 'kk')或find_by(name: 'kk', email: 'ddd')


``` irb
>> Candidate.find_by(id: 9487)
  Candidate Load (0.1ms)  SELECT  "candidates".* FROM "candidates" WHERE "candidates"."id" = ? LIMIT ?  [["id", 9487], ["LIMIT", 1]]
=> nil

>> Candidate.find(9487)
  Candidate Load (0.2ms)  SELECT  "candidates".* FROM "candidates" WHERE "candidates"."id" = ? LIMIT ?  [["id", 9487], ["LIMIT", 1]]
ActiveRecord::RecordNotFound: Couldn't find Candidate with 'id'=9487
  ...[略]...
  

```
Ruby  找不到方法的時候 11:00
```ruby
class Cat
    def method_missing
    end
end

```

==終端機&GIT==

```bash

Chesterde-MacBook-Pro:20190830 chester$ cd -
/Users/chester/Desktop/5xruby/ruby

Chesterde-MacBook-Pro:ruby chester$ cd -
/Users/chester/Desktop/5xruby/ruby/20190830

// git checkout -
```

>11:30 用 10.times 建立 Owner 資料

- all
- where => 找出來的資料"類似" Array
    - 可以用 Hash(哈希)的方式設定多個查詢條件
    - 也可以用字串 <= 解析成SQL語法
- select => 只找出特定的欄位
- order
- limit

11:34 
where 找多筆資料
find_by 只會找單筆資料
![](https://i.imgur.com/ViBiwMv.png)

![](https://i.imgur.com/rQd1LgH.png)
```
Product.where("price < 10").where(name: 'kk')
找出價錢少於10 and Name = 'kk'  的產品
```
## U

- save
- update
- update_attributes
- update_all  ==全部更新！！==
- increment
- decrement
- toggle

p1.update(price: p1.price+1) <= 直接寫入資料庫

p1.increment(:price, num) <= 修改欄位的值，但未寫入資料庫
p1.save

p1.increment!(:price, 2) <=修改欄位的值並同時寫入資料庫，可以修改1或其他值

## D

- delete:不會走Callback流程
- destroy:資料刪除後救不回來
- destroy_all

>destroy 跟 delete 的差別，在於 destroy 方法在執行的時候，會執行完整的回呼（Callback，在稍後的章節會介紹），但 delete 方法僅直接執行 SQL 的 delete from ... 語法，不會觸發任何回呼。
>>實務上會建立一個紀錄刪除的欄位，並不會真正刪除資料。[color=#5c52ce]
>>

## scope(做出一個範圍) <= 賦予查詢條件有意義的名稱

```ruby
#product.rb
    def self.cheap
        where("price <= 10")
    end
    
    或
    
    scope :cheap, -> {where('price <= 10')}
    (block物件化，lamda or proc，此例的->是lambda)
  ```
```bash  
#terminal
Product.cheap
```
![](https://i.imgur.com/FXqZ7DD.png)

```ruby
#product.rb
    #多行程式碼時使用def寫
    def self.cheaper_than(n)
        where("price <= #{n}")
    end
    
    或(兩種寫法一樣)
    
    #少行程式碼直接scope
    scope :cheaper_than, -> (n){where('price <= #{n}')}
    (block物件化，lamda or proc，此例的->是lambda)
```
```bash
#terminal
Product.cheaper_than(n)
```    

13:40 View
```ruby=
    def hello(message, &cc)
        puts message
        cc.call
    end
    
    hello do 
        puts "hi"
    end

```
## default_scope
> 通常用於假刪除或軟刪除[color=#5c52ce]
```ruby   
    default_scope { where order: :desc }
```
```bash
   Product.all.unscope(:order).order(:price)
```

## 自訂驗證器 14:15

需要另外設定 autoload path

```ruby
    validate :do_somthing
    private
    def 
    
    end
```

### before.save  and before.create

- before.save   都會做，只要資料有變動（save, update）
- before.create 只有新增

### 加密演算法
SHA  1    =>git
SHA  256  =>bitcoin



## 用 devise gem 建立多對多資料庫

![](https://i.imgur.com/NhJlstq.png)

>commmend :spring stop
spring <= 幫助建立程式的快取



用 devise 建立登入登出按鈕跟頁面
``` erb
<ul class="navbar-nav">
<% if user_signed_in? %>
  <li class="nav-item">
    <%=link_to "登出", destroy_user_session_path,class: "nav-link",method:"delete" %>
  </li>
<%else %>
  <li class="nav-item">
    <%=link_to "註冊", new_user_registration_path,class: "nav-link" %>
  </li>
  <li class="nav-item">
    <%=link_to "登入", new_user_session_path,class: "nav-link" %>
  </li>
<% end %>
</ul>
``` 
做出沒登入不能投票的功能
``` ruby
def vote
  if user_signed_in?
  v = @candidate.vote
    VoteLog.create(candidate_id: @candidate.id, ip_address: request.remote_ip)
    redirect_to root_path, notice:'投票成功!'
  else
    redirect_to root_path, notice:'請先登入會員!'
  end
end
``` 
再改寫一次投票功能
``` ruby
current_user.vote_logs.create(candidate: @candidate,ip_address: request.remote_ip)
``` 

timecode 16:25 改善又臭又長的controller