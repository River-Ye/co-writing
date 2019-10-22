---
title: Git 190826 AstroCamp
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: Git 
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}

大家要打起精神來喔 :+1: 
甘巴爹
這禮拜五倍沒開放，有適合讀書的地方推薦嗎？
這間
https://www.tony60533.com/oromo-cafe/
https://vilo92.pixnet.net/blog/post/368195795-%E3%80%90%E5%8F%B0%E5%8C%97%E3%80%91%E8%A5%BF%E9%96%80-sunny-cafe-%E7%87%88%E5%85%89%E7%BE%8E%E6%B0%A3%E6%B0%9B%E4%BD%B3%E2%80%A7%E6%96%B0%E9%96%8B%E5%B9%95
摸寧(｢･ω･)｢

11:49 pull push
12:26 PR 
14:00 Rails 6.0.0
14:15 繼續專案
15:40 投票後+1
16:18 資料庫效能問題

# Git Push

首先：
1. 有一個github帳號
2. 一個有commit 過的 project



```bash
git remote add origin https://github.com/username/bbcc.git
HTTPS / SSH
git  遠端  新增 代名詞              位置
git push -u origin master
            代名詞   分支branch
```
這裡的代名詞代表後面那串網址用什麼代名詞代表
所以 origin 可以代換成dragonball 或者任何想要的名詞
![](https://i.imgur.com/yxRVTFL.png)
HTTPS 跟 SSH 的差異?
HTTPS是加密網站，使用上囉嗦但設定上簡單
SSH則是公鑰跟私鑰，但這設定上麻煩，要key一些指令
https://git-scm.com/book/zh-tw/v1/%E4%BC%BA%E6%9C%8D%E5%99%A8%E4%B8%8A%E7%9A%84-Git-%E7%94%9F%E6%88%90-SSH-%E5%85%AC%E9%96%8B%E9%87%91%E9%91%B0

```bash
#  查看remote 狀況
git remote -v
#  這裡的-u 代表設定上線(upstream)，把自己本地端的分支(master)推到網站上(orgin其實是網址)
git push -u origin master
```
![](https://i.imgur.com/rgXPk0Y.png)

-u 設定一次之後，origin 就已經變成預設值，所以以後只要git push 即可
如果要移除已經設定的代名詞:
```bash
$git remote rm origin
```
## ==如果要複製檔案下來==
![](https://i.imgur.com/DC8A2gM.png)

先到想要拉檔案的資料夾，在打上下面指令
```bash
$git clone github上的網址
```
如果直接在github上面編輯然後存檔，他會自己做一次commit，這時候線上的進度會比本機快一步
![](https://i.imgur.com/QJggxVO.png)


```bash
# 進入已經複製好的專案裡
git pull
#  fetch + merge
```

## ==fetch==
把檔案抓過來，但不會主動合併
![](https://i.imgur.com/OcQeiJp.png)

```bash
# 進入已經複製好的專案裡
git fetch
#  only fetch 
```
情境:
自己的本機上面有新的commit，線上也有一次新的commit，這時候fetch 跟 pull有什麼差異?
![](https://i.imgur.com/KMpgPfx.png)
這邊看到會出現一個origin/master的節點
![](https://i.imgur.com/ys6RfzY.png)
這時候去merge，就會變這樣
![](https://i.imgur.com/NbhKVex.png)

## 和其他人一起工作

先拉再推


:::danger
!!!WARNIG!!!
使用前請先通知隊友
!!!WARNIG!!!
:::

```bash
# 我本來不想用這招的...
git push -f
```

如果想幫忙做別人的專案時=>fork先複製一份到自己的帳號
![](https://i.imgur.com/WQ2Fj3V.png)
改好了之後，再發送pull請求給原作者
![](https://i.imgur.com/pwx2BQW.png)
原作者有三個選項可以選:
1.原本的pull
2.squash & merge =>把它做的改變都變成一個commit然後merge
3.rebase & merge =>變成一條線

# Rails
進到過去的專案，雖然已經升級rails 6了，但進到專案去看rails 版本還是舊的
![](https://i.imgur.com/BHxwZZ8.png)
rails _5.2.3_ new 專案名稱
這樣就會用5.2.3啟動新專案

## rails db:seed
產生練習時的假資料
![](https://i.imgur.com/5bY4zVy.png)
存檔後在終端機輸入
`rails db:seed`
如果在網址打
![](https://i.imgur.com/kloygmk.png)
params就會抓到params[:abc]

先安裝kaminari的gemfile
controller頁面加上.page(params[:page]).per(5)：
```erb
def index
    @candidates = Candidate.all.order(vote: :desc).page(params[:page]).per(5)
end
```
接著在index view的頁面加上:
```erb
<%= paginate @candidates %>
```
雖然可換頁，但不夠美觀，下面再追加bootstrap的gem
## 用 Gem 進行美化 
### kaminari
### bootstrap4-kaminari-views

- theme: 'twitter-bootstrap-4'
```erb
<%= paginate @candidates, theme: 'twitter-bootstrap-4' %>
```

### simple_form的GEM
![](https://i.imgur.com/DI4e1vx.png)
他會判斷資料庫裏面每個欄位是什麼屬性，就把輸入的值轉換過去，所以不用再寫上text , integer等等屬性
輸入的資料不符合規定的話，也會自動跳出警語，本來的any?可以刪掉


## 資料庫的關聯

### has_many & belongs_to
>Rails 裡沒有真正的關聯
>==has_many 和 belongs_to 的作用，等同於 attr_accessor==
>
![](https://i.imgur.com/nKesQjf.png)
![](https://i.imgur.com/ubsweEB.png)


### Foreign Key 的額外設定方式
```ruby
belongs_to :candidate #,foreign_key: 'candidate_id'
belongs_to :candidate, foreign_key: 'c_id'
```

## N+1 問題
![](https://i.imgur.com/ZDOHnYs.png)
10個候選人欄位，資料庫要撈出來必須撈'11'次，效能上這樣的設計其實不好，所以應該要在index頁面寫一個暫存的區域儲存區，雖然會占用讀的空間資源，但以投票系統來說，寫的資源遠大於讀的資源，故在效能上這樣的做法還是有提升。


## Counter Cache

![](https://i.imgur.com/sZyR2Qd.png)
