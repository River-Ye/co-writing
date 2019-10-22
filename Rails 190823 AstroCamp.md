---
title: Rails 190823 AstroCamp
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: Rails ,Ruby
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}
# Rails 190823
安安
摸寧
在他方也給我認真（摩拳擦掌！
Watching U!  

---
13:41 整裡views(partial render)
14:28 新增欄位
16:09 Votelog

為什麼原始碼的方法是 POST 卻能傳到 使用 PATCH 方法的update路徑?
![](https://i.imgur.com/N7K3GnD.png)

因為瀏覽器的預設方法只有POST跟GET，其他方法都是自己加給他的，可以看到原始碼後面有這段

![](https://i.imgur.com/UtdmZC8.png)
可以用data裡面插入 hash 的方式輸入資料
``` html
<%= link_to "刪除", candidate_path(candidate.id),data: { method:'delete',xyz:'abc',confirm:'確定嗎?'} %>
``` 
UJS 讀到 data 這個資料裡面有 confirm的資訊才會做出跳警告窗的效果，如果拿出來會沒用

# ==去除重複:(Rebuild)==
1. 把重複的找 candidate參數抓出來
``` ruby
private
  def find_candidate
    @candidate = Candidate.find_by(id: params[:id])
  end
```
2. 定義在最一開始: 在特定 method 的 action 使用之前要呼叫 find_candidate 方法
``` ruby
before_action :find_candidate, only: [:show, :edit, :update, :destroy]
```
3. 重複的 clean_params 也抓出來
``` ruby
def candidate_params
    params.require(:candidate).permit(:name, :age, :policy, :party)
end
  ```
4. 再把 create 跟 update 中的 clean_params 代換掉
``` ruby
if @candidate.update(candidate_params)
```
5. 把 notice 簡寫得更短
``` ruby
  def update
    if @candidate.update(candidate_params)
      # flash[:notice] = "更新成功"
      redirect_to root_path, notice: "更新成功"
    else
      render :edit
    end
  end
```
6. 把 new 跟 edit 使用 partial render 的方式做出新的_form.html.erb

```htmlembedded
    <%= render 'form'%>
```
7. 把_form檔案裡面的@candidate換成區域變數 candidate，不要讓局部渲染的檔案(_form)去外面抓資料，反之應該讓要渲染的檔案餵資料給這個區域變數
```htmlembedded
    <%= render 'form', candidate: @candidate%>
```
8. 如果每個頁面都要show flash，就把它放在layout裡面
 ![](https://i.imgur.com/zomwh9b.png)

==如果需要加欄位==
使用rails g migration
![](https://i.imgur.com/1FYM5Tp.png)

再去編輯db裡面的檔案，寫好了之後用 rails db:migrate更新資料庫
![](https://i.imgur.com/Pz0vpuO.png)
修正：
```ruby
    add_column  :candidates, :degree, :string
```

>model 新增欄位後，記得==順手==去修改 view 和 controller [color= #0f0]
>


# 實作新的功能
## 新增路徑 vote

```ruby
resources :candidates do 
    member do
      # put :vote, to: 'candidates#vote'
      put :vote #put /candidates/2/vote
    end
    
  end
```

## 新增欄位 vote（幫candidates）
```
add_column :candidates, :vote, :integer ,default: 0
```

```ruby
  def vote
    v = @candidate.vote
    @candidate.update({vote: v+1})
    redirect_to root_path, notice:'投票成功!'
  end
```
```ruby
v = @candidate.vote || 0
# 沒有設定default: 0  時的處理方式
```

通常 patch 使用在更改單筆欄位，put使用在更改全部資料
![](https://i.imgur.com/VCV9ekN.png)


### 做排序
```ruby
  def index
    @candidates = Candidate.all.order(vote: :desc )
  endp_address
```

## VoteLog

```ruby
rails g model VoteLog candidate_id:integer ip_address: string

rails g model VoteLog  candidate:references ip_address
```

```ruby

vv = VotLog.new(candidate_id: @candidate.id, ip_address: request.remote_ip)
    vv.save

    VotLog.create(candidate_id: @candidate.id, ip_address: request.remote_ip)
    VotLog.create(candidate: @candidate, ip_address: request.remote_ip)
```

:::danger
rails db:rollback 只對資料庫(migration檔)有作用；
對Model 檔沒有
:::

---
# Other:

## ActiveRecord::InvalidForeignKey

https://github.com/jenseng/immigrant/issues/31
https://ihower.tw/rails/activerecord-relationships.html
