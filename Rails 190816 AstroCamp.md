---
title: Rails 190816 AstroCamp
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: Rails, Gem, Ruby
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}

morning
G'ning ning
摸尼
# ==Time code==
15:05 namespace (網址階層切割) 時間點可能有點落差 
15:23 8個路徑不夠用怎麼辦？做擴充
15:31 試著寫票選系統(開新專案)
15:49

# ==Route and Model View Controller==
![](https://i.imgur.com/s9LYDFU.png)


## ==Route==
:::danger
Do not mess with  樓下阿桑!!
Nobody can mess with 樓下阿桑!
Nobody.
:::

>Route 在做的事：
>=>指出正確的PATH
讓Rails能找到對應的Controller 和 View ，並將透過Model取得的資料(從資料庫)呈現在使用者面前？
>Other opinion?[color=#c7ce06]

### MVC架構：由Route開始，到Controller結束。
1. 瀏覽器依據資源(resources)的設定，經由Route去找到Controller中，對應的action(method)。
2. 視需要透過Model向資料庫取得資料後，處理完再與對應的View(.html.erb)做結合。
3. View處理完後再回傳給Controller，Controller再將結果轉交給瀏覽器。



## ==建立Rails專案==
進入終端機，到你想要建立的路徑後輸入
rails new 名稱
![](https://i.imgur.com/iNyMPnZ.png)

gemfile >> bundle install
如果新增一個套件在Gemfile要用 bundle install 來安裝到自己電腦
![](https://i.imgur.com/sTiNCsD.png)
Bundle是為了解決版本相依問題，它會去看gemfile這個檔案
它會去看每個軟體的相依性，把它存在gemfile.lock這個文件裡面

            
>[Difference between bundle and gem install?](https://stackoverflow.com/questions/6162047/difference-between-bundle-and-gem-install)

``` =ruby
gem 'rails', '~> 5.2.3'
            major, minor, patch
```
                    
~> 代表只會下載patch的版本

## ==純手工打造過程：==

![](https://i.imgur.com/4qZTqrF.png)
如果有人連到about.php，請去pages#about(去pages的controller的方法about)
1.在controller資料夾內開一個檔案叫pages_controller.rb

已經生出pages_controller.rb
![](https://i.imgur.com/NWqUlzK.png)

2.並在pages_controller.rb檔內繼承ApplicationController

想自動生成的話，可以在終端機
$ rails g controller pages 

檔案名稱與類別名稱必須相同，只是case的方式不同
![](https://i.imgur.com/UHN7Qb7.png)


3.新增一個方法about
``` =ruby
class PagesController < ApplicationController
  def about
    render html: 'hello, world'
  end
  def 
  
    # render html: '首頁'，可用類似sinatra的erb方式呼叫
  end
end
#localhost:3000/可印出hello, world
```
4.用類似erb方式呼叫：
  在views檔案新增一個檔案叫pages後，在裡面新增檔案index.html.erb
  ![](https://i.imgur.com/D0HpAN8.png)
## ==透過指令產生 rails g controller==
自動化過程：
在終端機輸入 rails generate controller Hello
產出名叫Hello的controller(完成手工做的1~2步驟)
(可縮寫generate成 g) 

![](https://i.imgur.com/RtS2M0Y.png)
![](https://i.imgur.com/wjN0DaY.png)

另一種方法：
同時生出controller也在route生出路徑
views也有網頁html.erb
==終端機==
![](https://i.imgur.com/0tmlXY2.png)
==controller==
![](https://i.imgur.com/aNaG3WL.png)
==views==
![](https://i.imgur.com/gc0pHz4.png)
==route==
![](https://i.imgur.com/zlOSE7y.png)
 
 route裡另一種寫首頁方法：根目錄
![](https://i.imgur.com/RChy5Iq.png)
==行為/習慣==
![](https://i.imgur.com/y4mY8Cy.png)



``` == 
Rails.application.routes.draw do
  get '/', to: 'pages#index' #可以寫成 #root 'pages#index'  首頁

  get '/about', to: 'pages#about'
  get '/about', to: 'pages#index'    #當路徑相同時這行等於沒用，會抓最上面路徑(about)
end

```

# ==RESTful的網址設計(面試常考喔^_< 啾咪~)==
>導入 REST 的設計，可讓網址變得更直觀，而且也幫開發人員訂了一套網址設計的慣例。
>>將網址視為一種資源(不同的看法，可以用“>”做出區隔，還可以上色。)[color=#c7ce06]
>>>(或是譯作“將網址作為一組來源的標準化”？？)
>>>>產生8條路徑，7個方法
>

![](https://i.imgur.com/llejAnn.png)


```ruby
Rails.application.routes.draw do
  resources :products
  #resources 要用複數
  #慣例上，後面接的symbol 也會使用複數
end
```

```irb
rails routes #查看所有的path
```
![](https://i.imgur.com/ZPKA5fM.png)

![](https://i.imgur.com/1SPa4SX.png)


![](https://i.imgur.com/LalRHKn.png)


```ruby
#index.html.rb
<%= link_to 'Go, CATS!', cats_path %>

#心情點播：趁早 - 張宇
```

[More about link_to](https://api.rubyonrails.org/v5.2.3/classes/ActionView/Helpers/UrlHelper.html#method-i-link_to)

![](https://i.imgur.com/4QdBlG1.png)

![](https://i.imgur.com/Rrx20xv.png)
![](https://i.imgur.com/AkbFWII.png)
![](https://i.imgur.com/yVZhBLa.png)

==後台管理Admin==

```ruby=
resources :articles, only: [:index, :show]

namespace :admin do
  resources :articles
end
```

![](https://i.imgur.com/p1CSKfG.png)

==八條不夠用了怎麼辦？！==

![](https://i.imgur.com/BHBgCTY.png)
```ruby
resources :cats do
    #/cats/2/paly  需要針對特定對象（有ID）
    member do
      get :play
      get :sleep
    end
    #/cats/say_hello 同時對所有的對象
    collection do
      get :say_hello
    end
  end
```
![](https://i.imgur.com/7idsQQV.png)
![](https://i.imgur.com/vT7hANS.png)

# ==投票系統==
先想路徑


Step by Step
1. 建立新專案：rails new vote
1. 設定路由：routes.rb
    ```ruby
    resources :candidates
    ```
![](https://i.imgur.com/VRfxsxf.png)

1. 新增Controller
    ```ruby
    #candidates_controller.rb
    class CandidatesController < ApplicationController
    end
    ```
1. 建立對應的action，在views 底下建立candidates資料夾，建立對應的xxx.html.erb(index & new)
    ```ruby
    #candidates_controller.rb
    def index
    end
    def new
    end
    ```
1. 建立Model: 
    1. rails g model Candiate name party age:integer policy
    2. 會產生Model:candidates.rb 和 migration檔
    3. 確認migratetoin檔後，執行rails db:migarate 
4.  運用rails 提供的 form_for 來建立表單，達到自動生成CSRF Token 的目的
    ```htmlembeded
        <%= form_for %>
    ```
1. 設定資料驗證
    ```ruby
    #candidate.rb
    validates :name, prensence: true
    validates :age, 
    ```
1. 有了資料驗證，那應該要有顯示錯誤訊息
    ```htmlembeded
    <!--new.html.erb -->
    <% if @candidate.errors.any? %>
      <ul>
        <% @candidate.errors.full_messages.each do |message|%>
        <li><%= message %></li>
        <% end %>
      </ul>
    <% end %>
    ```
1.  只有new 還不夠 ，need action create
    ```ruby
    #create
    #Strong Params
    clean_params = params.require(:candidate).permit(:name,:party,:age,:policy)
    @candidate = Candidate.new(clean_params)

    if @candidate.save
      redirect_to candidates_path, notcie: "Add Success!"
    else
      render :new
    end 
    ```
    1. Strong Params:
    >It provides an interface for protecting attributes from end-user assignment. [color=#25f95e]
    >
1.  修改 index.html.erb 新增 action show & show.html.erb
    ```htmlembeded
    <%= link_to candidate.name, candidate_path(candidate.id)%>
    ```
    ```ruby
    def show
        @candidate = Candidate.find_by(candidate.id)
    end
    ```

1. 增加編輯的功能：
    1. 增加編輯的連結：index.html.erb
    ```htmlembeded
    <%= link_to "Edit", edit_candidate_path(candidate.id)%>
    ```
    2. 新增action edit 和 edit.html.erb
    ```ruby
    def edit
        @candidate = Candidate.find_by(candidate.id)
    end
    ```
1. 只有edit還不夠，need action update
```ruby
#更新資料也需要Strong Parameters
clean_params = params.require(:candidate).permit(:name,:party,:age,:policy)
#要知道要更新哪一筆資料
@candidate = Candidate.find_by(id: params[:id])

if @candidate.update(clean_params)
  redirect_to candidates_path, notcie: "Update Success!"
else
  render :edit
end 
```
11. 增加刪除的功能：
    1. 增加刪除的連結：index.html.erb
    ```htmlembeded
    <%= link_to "Delete", candidate_path(candidate.id), method: 'delete', data: {confirm: "R U SURE"}%>
    ```
    2. 新增action destroy
    ```ruby
    def destroy
        @candidate = Candidate.find_by(candidate.id)
        @candidate.destroy
        redirect_to candidates_path, notcie: "Delete Success!"
        
    end
    ```
 1. 增加額外的功能（vote）
    1. 先幫Model 新增欄位：
    ```irb
    rails g migration add_degree_to_candidate
    ```
    2. 編輯新增的migration檔
    ```ruby
    add_column
    ```

    
