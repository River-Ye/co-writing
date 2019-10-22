---
title: Rails 198027 AstroCamp
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags:  Rails, Ruby
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}

good morning
KD 說早安:)
摸寧

# Active Record




Active Record ＝ 把資料做成物件（是一種ORM框架）

物件 = 欄位 + 基本操作 + 商業邏輯

Model = ActiveRecord.new

![](https://i.imgur.com/tqWEKY7.png)



ORM = Object Relational Mapping
物件關聯對映（英語：Object Relational Mapping，簡稱ORM，或O/RM，或O/R mapping），是一種程式設計技術，用於實現物件導向程式語言裡不同類型系統的資料之間的轉換。從效果上說，它其實是建立了一個可在程式語言裡使用的「虛擬物件資料庫」。
## CoC 
慣例
![](https://i.imgur.com/RvpWJa5.png)
每個資料表產生的時候會有一個id欄位(預設的primary key)


pluralize
singularize
camelize
underscope
```bash
$ rails g model vote => Vote / votes
$ rails g model Votelog => Votelog / votelogs
$ rails g model VoteLog => VoteLog / vote_logs
```
model輸入有誤，還未migrate前，可用
```bash
$rails d model .... 來刪除
```
![](https://i.imgur.com/EWKvFS6.png)
每間店都有店長，每個店長都有個人資料，如果我想要店長資訊的話，不需要每次都要撈他的個人資料出來，只要用id編號就可以知道是哪位店長。

## 一對一關聯
==建立關聯性==
![](https://i.imgur.com/amV7fX8.png)

建立一筆資料'o1'
![](https://i.imgur.com/TCeRV9g.png)
Owner.first = Owner.find_by(id: 1)

建立一個物件owner(物件化)
![](https://i.imgur.com/DeTxvNg.png)
如果還沒有store 的資料，使用 owner.store 他還是會試著去抓，這就是 has_one owner 的方法
![](https://i.imgur.com/NCYFTjT.png)
記得先寫 o1 就是 Owner.first


## ==破壞慣例：==
情境：建立 Owner Model 時，命名成  Ownner 
    建立 Store Model,用 owner:references
```ruby
class Ownner < ApplicationRecord
    #
  has_one :store, foreign_key: 'owner_id'
  
end
```
![](https://i.imgur.com/0bAb033.png)
```
o1.store = s1 
將store1 指給owner1
[["title", "S1"], ["owner_id", 1]
```

![](https://i.imgur.com/whHrG6u.png)

store 讀    reader
store= 寫    setter

.create_store => 直接寫入資料庫
.build_store => 建立，但==未寫入==資料庫

後面的 store 是跟著 hasone 後面的store產生
如果後面是book，就會變成create_book、build_book
build之後要save才會存入，create不用save，直接寫入

在voting專案裡面投票的功能可以根據不同觀點改寫
``` ruby
#=>候選人視角
@candidate.vote_logs.create(ip_address: request.remote_ip)
#=>選票視角
VoteLog.create(candidate_id: @candidate.id, ip_address: request.remote_ip)
``` 


## ==多對多關聯==
需要有第三方表格儲存資訊

Create WareHouse's migration
```bash
rails g WareHouse sotre:references product:references
rails g WareHouse sotre_id:integer product_id:integer
```
```ruby
#store.rb
class Store < ApplicationRecord
  has_many :ware_houses
  has_many :products, through: :ware_houses #透過第三方
end

#product.rb
class Product < ApplicationRecord
  # belongs_to :store
  has_many :ware_houses
  has_many :stores, through: :ware_houses #透過第三方
end
```

```ruby
#store.rb
has_and_belongs_to_many :products, join_table: :ware_houses
#product.rb
has_and_belongs_to_many :stores, join_table: :ware_houses
```
:::warning
Model 和  table 不是一對一
:::
:::success
使用HABTM時，可以不需要 Model？ 視情況 :
如果是只存放id，可以不需要有 Model，
只要有table存放資料就可以。
table: products_stores
:::

:::danger
分工合作
讓資料庫去挑選資料

:::











