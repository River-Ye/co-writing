---
title: SQL 190902 AstroCamp
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: SQL 
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}

不是在5xruby就是在5xruby的路上，大家早
外面有我帶回來的芒果乾，歡迎享用，大家不要客氣唷：）

- Database & Table
- RDBMS
- 

刪除資料庫（--是註解）
![](https://i.imgur.com/yE4dwuZ.png)

選擇某一個資料庫（兩個都可以）
![](https://i.imgur.com/SfqeHwl.png)

![](https://i.imgur.com/uz8Im7W.png)

在選擇的資料庫建立表單
![](https://i.imgur.com/GaEhX32.png)
PK:主鍵
NN:不可為null (一定要填寫)
AI:資料裡面自動遞增產生流水編號

建立表單
![](https://i.imgur.com/tgffgaS.png)

![](https://i.imgur.com/0SagPbH.png)


SQL 語法 沒有區分大小寫；
但是，為了識別容易，通常會將SQL 內專有的語法 用大寫表示
Differ between CHAR and VARCHAR
DDL DML DQL
## Create
```
INSERT INTO heroes
(name, gender, age, hero_level, hero_rank, description)
VALUES
('埼⽟'
, 'M', 25, 'C', 388,
'無論多強的敵⼈幾乎都是⼀拳擊敗');
```
Hero.where(hero_level:'s').where(gender: 'F')

## Read
```sql
select * from heroes where hero_level = 'S' and gender = 'F';

-- Hero.where(hero_level: 'S', fender: 'F')
-- Hero.where(hero_level: 'S').where(fender: 'F')
```

```sql
select name, gender from heroes where hero_level = 'S' and gender = 'F';
```
```sql
select * from heroes where age is null;
```
```sql
select * from heroes where name like '%背心%';
--'背心'   找名字跟'背心'完全一樣的資料
--'%背心'  找名字是'XX背心'的資料
--'背心%'  找名字是'背心XX'的資料
--'%背心%' 找名字中含有'背心'的資料，不論頭尾
```

```sql
select * from heroes where age >=10 and age <=25;
select * from heroes where age between 10 and 25;
```

```sql
select * from heroes where hero_level = 'S' or hero_level = 'A'
select * from heroes where hero_level in ('S','A')
```
```sql
select * from heroes where hero_level <> 'S'
```
```sql
select * from heroes where hero_level <> 'S' and hero_level <> 'A'
select * from heroes where hero_level not in ('S', 'A');
```
## Update
```sql
update heroes set age =20, hero_rank =100 where id = 25;
-- Hero.find(25).update(age: 20, hero_rank: 100)
```

```sql
update heroes set hero_level = 'B', hero_rank = '101' where id = 35;
select * from heroes where id = 35
```
## Delete
```sql
delete from heroes where hero_level = 'C'
```
# Advenced Query

## 計算總數
```sql    
SELECT COUNT(*)
FROM awesome_db.heroes
WHERE hero_level = 'A';

-- rails寫法
-- Hero.where(hero_level: 'A').count
```

## 加總
```sql
 SELECT SUM(age)
 FROM heroes
 WHERE hero_level = 'A' AND age IS NOT NULL;
```

## 平均
```sql
 SELECT AVG(age)
 FROM heroes
 WHERE hero_level = 'A' AND age IS NOT NULL;
```
## 自訂排序
```sql
SELECT hero_level, SUM(age) 
FROM heroes 
GROUP BY hero_level
ORDER BY FIELD(hero_level,'S','A','B','C');
```

## 排序
```sql
SELECT * 
FROM heroes 
WHERE hero_level = 'S' 
ORDER BY hero_rank
```

## 只取前三名
```sql
SELECT *
FROM awesome_db.heroes
WHERE hero_level = 'S'
ORDER BY hero_rank
LIMIT 3;

-- rails寫法
-- Hero.where(hero_level: 'S').order(:hero_rank).limt(3)
```

# 查詢多個表格
## ER 圖
![](https://i.imgur.com/YRdcTAf.png)

![](https://i.imgur.com/O9n6hW3.png)

設定foreign key:為了確保這筆資料是來自正確的來源
![](https://i.imgur.com/892KXAG.png)

```sql
select * from monsters where kill_by = 
(select id from heroes where name = '埼玉');
```

## 交集 join 
拿兩個表格來交叉查詢
#### inner
![](https://i.imgur.com/o7gPiMq.png)
#### left
![](https://i.imgur.com/qCvLUFp.png)
#### right
![](https://i.imgur.com/j8OOADn.png)
### 反派被誰打倒的
![](https://i.imgur.com/7pc4ioB.png)

```sql
--精簡版（另設代名詞）
select m.name, m.danger_level, h.name
from monsters as m
inner join heroes as h
on monsters.kill_by = heroes.id
--as 也可省略
```

```sql
-- 列出與35戰鬥過的怪物名稱
select m.name
from battle_histories b
left join monsters m 
on b.monster_id = m.id
where hero_id = 35;
```

16:18 API

```
rails g controller api/v2/candidates
```
