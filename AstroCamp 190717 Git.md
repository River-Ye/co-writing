---
title: AstroCamp 190717 Git
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: Git
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}

# Git


檢視目前設定
`git config --list`

```
git config --global user.name "yourname"
git config --global user.email "youremail"
```

---

初始化
`git init`

檢視git 目前狀態
`git status`

將檔案加入追蹤（）
`git add`

Working 
Staging
Repository
---


查詢 commit 紀錄
`git log`

`git log --oneline`

Ｖim操作
[image:6DB121F6-E9F3-4281-AFA8-BC320720CC02-25905-0003E15381749A76/E27CE085-2BED-48BC-AE8B-F0E7F30D2DFF.png]


---

刪除檔案
`rm file_name`

刪除目錄
`rm -rf direction_name`

回到最近上一次的版本
`git checkout .`


`git blame`

::codility::  : 會側錄你寫CODE的過程


---

查看現有branch
`git branch`

建立新的 branch
`git branch branchname`

HEAD : what you are looking at …::What brand we focus on::
`*`

切換至其他 branch
`git checkout branchname`

---

合併分支

`git checkout master`

`git merge cat`

=> master (被HEAD指定的branch)會往前進

刪除branch
`git branch -d branchname`

狀況題：尚未合併的commit，但分支(branch)已刪除時，
		該如何找回commit ID ?

觀看開發者全部 git 的操作記錄
`git reflog`

圖像化顯示（在終端機）
`git log  --oneline --all --graph`

---

另一種合併分支的方式：rebase

Merge or Rebase ?

過程（歷史紀錄）是否有需要保留？
不需要	>	rebase	(會影響歷史紀錄的順序)
需要	>	merge

---
回到上一步

`git reset --mixed`   預設自帶

[image:8B23A451-D439-4622-B0FA-FDEA408F02DF-25905-0004047085BA16D1/F8F0D783-C9F3-413E-AF7B-A301C33179E1.png]




^ and ~ (使用代名詞)



---


其他git 簡碼

`gcb` = `git checkout branch `







