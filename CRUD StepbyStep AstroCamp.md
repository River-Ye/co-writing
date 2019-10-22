---
title: CRUD StepbyStep AstroCamp
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: Rails, Project, Gem, 
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}

# Step by Step
## ==專案-路由==
### 1. 建立新專案：
```bash
#terminal
rails new vote
```
### 2. 設定路由：
```ruby
#routes.rb
resources :candidates
root 'candidates#index'
```
![](https://i.imgur.com/VRfxsxf.png)
## ==Controller and Views==
### 1. 新增Controller
    ```ruby
    #candidates_controller.rb
    class CandidatesController < ApplicationController
    end
    ```
### 1. 建立對應的action，在views 底下建立candidates資料夾，建立對應的xxx.html.erb(index & new)
```ruby

```

## ==Model==
 接下來，為了能將資料寫入資料庫，需要用Rails 的指令來操作：
### 1. 建立Model: 
 
```bash
#terminal
rails g model Candiate name party age:integer policy
```
### 2. 會產生Model:candidates.rb 和 migration檔
### 3. 確認migratetoin檔後:
```bash
#terminal
rails db:migarate 
```
## CRUD-==C== 

### form_for & CSRF Token
0. 從controller 開始
    ```ruby
    def new
        @candidate = Candidate.new
    end
    ```
    
1.  運用rails 提供的 form_for 來建立表單，達到自動生成CSRF Token 的目的
    ```htmlembeded
    <!--     new.html.erb -->
        <%= form_for @candidate do |candidate| %>
                    .......
        <% end %>
    ```
### validates
1. 設定資料驗證
    ```ruby
    #candidate.rb
    validates :name, prensence: true
    validates :age, numericality: {greater_than: 39}
    ```
### errors messages
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
### create with new
1.  只有new 還不夠 ，need action create
    ```ruby
    #def create
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
## CRUD-==R==
### show
1.  修改 index.html.erb  & action index，
 新增 action show & show.html.erb
    ```htmlembeded
    <%= link_to candidate.name, candidate_path(candidate.id)%>
    ```
    ```ruby
    def index
        @candidate = Candidate.all
    end
    
    def show
        @candidate = Candidate.find_by(id: params[:id])
    end
    ```
## CRUD-==U==
### edit
1. 增加編輯的功能：
    1. 增加編輯的連結：index.html.erb
    ```htmlembeded
    <%= link_to "Edit", edit_candidate_path(candidate.id)%>
    ```
    2. 新增action edit 和 edit.html.erb
    ```ruby
    def edit
        @candidate = Candidate.find_by(id: params[:id])
    end
    ```
### update
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
## CRUD-==D==
### destroy
11. 增加刪除的功能：
    1. 增加刪除的連結：index.html.erb
    ```htmlembeded
    <%= link_to "Delete", candidate_path(candidate.id), method: 'delete', data: {confirm: "R U SURE"}%>
    ```
    2. 新增action destroy
    ```ruby
    def destroy
        @candidate = Candidate.find_by(id: params[:id])
        @candidate.destroy
        redirect_to candidates_path, notcie: "Delete Success!"
        
    end
    ```
    
## Rebuild(DRY)(可略過)
==DRY = Don't repeat yourself==

### Controller
```ruby
before_action :find_candidate, only: [:show, :edit, :update, :destroy, :vote]

            ......
    
private 
def find_candidate
    @candidate = Candidate.find_by(id: params[:id])
end
# Stonger parameters
def candidate_params
    clean_params = params.require(:candidate).permit(:name, :age, :policy, :party, :degree)
end

```

### Views(new.html.erb & edit.html.erb)
- _form.html.erb
```htmlembedded=
<% if candidate.errors.any? %>
    <% candidate.errors.full_messages.each do |message| %>
    <% end %>
<% end %>

<%= form_for(candidate) do |form| %>
  
              ...
  
<% end %>
```
:::info
將重複的部份抽出來，並讓_form.html.erb變成被餵食的對象，
1,2,6行原本的 @candidate 修改為 candidate
:::
- new.html.erb & edit.html.erb
```htmlembedded
<%= render 'form', candidate: @candidate %>
```

- 將插入的navbar，用相同的手法抽離出來
## ==新增欄位==(可省略)
### 從終端機下指令：
1. 先幫Model 新增欄位：
    ```bash
    # terminal
    rails g migration add_degree_to_candidate
    ```
 2. 編輯新增的migration檔
    ```ruby
    add_column  :candidates, :degree, :string
    ```
3. 別忘了...
    ```bash
    # terminal
    rails db:migrate
    ```


>model 新增欄位後，記得==順手==去修改 view 和 controller [color= #0f0]
>

## ==Vote (增加額外的功能)==
### 1. 新增路徑 vote
```ruby
resources :candidates do 
    member do
      # put :vote, to: 'candidates#vote'
      put :vote #put /candidates/2/vote
    end
    
  end
```
### 2.新增欄位 vote
```bash
# terminal
rails g  migration add_vote_to_candidates
```
```ruby
    add_column  :candidates, :vote, :integer, default: 0
```
```bash
# terminal
rails db:migrate
```


### 3. 更新index.html.erb 與 show.html.erb
```htmlembedded=
<!-- index.html.erb -->
    <%= link_to '懇請賜票！！', vote_candidate_path(candidate.id), class: 'btn btn-md btn-warning', method:'put',data: { confirm: "R U SURE？", abd: 1234}%>
    (<%= candidate.vote%>票)
```
```htmlembedded=
<!-- show.html.erb -->
<li>得票數：<%=@candidate.vote%></li> 
```
### 4. action vote
```ruby
  #before_action 裡記得...

  def vote
    # v = @candidate.vote || 0
    # 沒有設定default: 0  時的處理方式
    v = @candidate.vote
    @candidate.update({vote: v+1})
    redirect_to root_path, notice:'投票成功!'
  end
```
### 做排序
```ruby
  def index
    @candidates = Candidate.all.order(vote: :desc )
  endp_address
```
## VoteLog (記錄投票IP)
### 建立Model
```irb
rails g model VoteLog candidate_id:integer ip_address: string

rails g model VoteLog  candidate:references ip_address
```
:::danger
rails db:rollback 只對資料庫(migration檔)有作用；
對Model 檔沒有
:::
```bash
# terminal
rails db:migrate
```
### 增加candidate資料表關聯
```ruby
#candidate.rb

has_many :vote_logs, dependent: :destroy
```
### 修改index.html.erb
```htmlembedded
<!-- index.html.erb -->
<li>得票數：<%=@candidate.vote_logs.count%></li> 
```

### 修改action vote

```ruby
#def vote
#兩段式，同def create 的寫法
vv = VotLog.new(candidate_id: @candidate.id, ip_address: request.remote_ip)
    vv.save

VoteLog.create(candidate_id: @candidate.id, ip_address: request.remote_ip)
VoteLog.create(candidate: @candidate, ip_address: request.remote_ip)
```

## ==N+1問題 => Counter Cache==

### 1. 在 Candidate Model 開一個 integer 型態的欄位，名稱叫做 vote_logs_count
```bash
rails g migration add_count_to_candidate vote_logs_count:integer
```
記得要進migration檔裡加default: 0
```bash
rails db:migrate
```
### 2. 在 VoteLog Model 的 belongs_to 後面加上 counter_cache: true 參數：
```ruby
class VoteLog < ApplicationRecord
    belongs_to :candidate, counter_cache: true
end
```
### 3. 修改index.html.erb
```htmlembedded=
    <%= candidate.vote_logs_count %>
```

### 4. 之前的投票紀錄不見了？！
 應該單獨被執行，另外寫一個rakefile

```ruby
#lib/reset_counter.rake
namespace :db do
  desc 'Reset DB Counter Cache'
  task :reset_counter => :environment do 
    Candidate.all.each do |c|
      Candidate.reset_counters(c.id, :vote_logs)
    end
  end
end
```
    
## Use Gem to  build

### 1. kaminari
https://github.com/kaminari/kaminari

- Gem.file => bundle
- Controllers
    
    
    ```ruby
    @candidates = Candidate.all.page(params[:page]).per(5)
    ```
- Views
    ```htmlembedded
    <%= paginate @candidates %>
    ```
### 2. bootstrap4-kaminari-views
https://github.com/KamilDzierbicki/bootstrap4-kaminari-views
- Gem.file => bundle
-  Views
    ```htmlembedded
    <<%= paginate @candidates, theme: 'twitter-bootstrap-4' %>
    ```
### 3. simple_form
https://github.com/plataformatec/simple_form
- Gem.file => bundle
- Views 
    ```htmlembedded
    <%= simple_form_for(candidate) do |form| %> 
          <div class="box">
            <%= form.input :name, label: '姓名' %>
            <%= form.input :party, label: '黨派'%>
            <%= form.input :degree, label: '學歷'%>
            <%= form.input :age, label: '年齡'%>
            <%= form.input :policy, label: '政見'%>
            <%= form.button :submit, class: 'btn btn-md btn-success'%>
          </div>
    <% end %>
    ```

    
### 4. faker
https://github.com/faker-ruby/faker
    
- Gem.file => bundle
- seed.rb
    ```ruby
    10.times do |i|
          Candidate.create(
            name: Faker::Name.name, 
            party: Faker::TvShows::GameOfThrones.house, 
            degree: ["junior high", "senier high", "universuty", "graduate","PHD"].sample,
            age: Faker::Number.between(from: 40, to: 65), 
            policy: Faker::TvShows::GameOfThrones.quote )
    end
    ```
    

# 用Devise建立登入系統 與 建立多對多關聯

## 安裝Devise

1. 
    ``` ruby
    #Gemfile.rb
    gem 'devise'
    ```
2. 
    ```bash
    rails generate devise:install
    ```
    :::danger
    In the following command you will replace ==MODEL== with the class name used for the application’s users (it’s frequently User but could also be Admin). This will create a model (if one does not exist) and configure it with the default Devise modules. The generator also configures your config/routes.rb file to point to the Devise controller.
    :::
3. ```bash
    rails generate devise MODEL
    ("Caution!! Don't use MODEL as Model name")
    (rails g migration User user:references)
    ```
4. ```bash
    db:migrate 
## 設定資料庫關聯(多對多)
1. 幫原本的VoteLog增加 user_id 欄位
```bash
    rails g  migration add_userid_to_VoteLog user:references 
```
```bash
    rails db:migrate
```
2.  設定關聯
```ruby
#candidate.rb
validates :name, presence: true
validates :age, numericality: { greater_than: 40}

has_many :vote_logs, dependent: :destroy
has_many :users, through: :vote_logs
```
```ruby
#vote_log.rb
belongs_to :candidate, counter_cache: true
belongs_to :user
```
```ruby
#user.rb
devise :database_authenticatable, :registerable,
       :recoverable, :rememberable, :validatable

has_many :vote_logs
has_many :candidates, through: :vote_logs
```
## 做出註冊/登入/登出的按鈕

```htmlembedded
<!-- _nav.html.erb -->
<ul class="navbar-nav ">
    <% if user_signed_in? %>
      <li class="nav-item">
        <%= link_to "登出", destroy_user_session_path, 
            method: :delete, class: "nav-link" %>
      </li>
    <% else %>
      <li class="nav-item">
        <%= link_to "註冊", new_user_registration_path,
            class: "nav-link" %>
      </li>
      <li class="nav-item">
        <%= link_to "登入", new_user_session_path, 
            class: "nav-link"%>
      </li>
    <% end %>   
</ul>
```

## 改寫controller的內容
### 登入才能投票
### user_signed_in? & current_user
```ruby
#def vote end
if user_signed_in?      
  current_user.vote_logs.create(candidate: @candidate, ip_address: request.remote_ip) 
  redirect_to root_path, notice: "投票成功"
else
  redirect_to root_path, notice: "請先登入會員"
end
```

###  不能重複投票給同一名候選人
```ruby
if user_signed_in?      
      if VoteLog.find_by(user: current_user, candidate: @candidate)
        message = "已投過此候選人"
      else
        current_user.vote_logs.create(candidate: @candidate, ip_address: request.remote_ip)
        message = "投票成功"
      end   
  redirect_to root_path, notice: message
else
  redirect_to root_path, notice: "請先登入會員"
end
```

### 每人只能投三票




### 建立API














