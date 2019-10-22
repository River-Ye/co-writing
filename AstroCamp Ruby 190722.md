---
title: AstroCamp Ruby 190722
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: Ruby
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}
###### 20190722   tags: `Ruby`


# ==Numeric(數字)==

```=Ruby
puts 3.55.round    #四捨五入
puts 3.55.floor    #無條件捨去
puts 3.55.ceil     #無條件進位
puts 3.55.to_i     #轉整數
```

==一些在運算上會出現意料之外的結果的運算==
```=Ruby
(0.3 / 0.1).floor  #=> 2 (!)
(2.1 / 0.7).ceil  #=> 4 (!)
(0.3 / 0.1).to_i  #=> 2 (!)
```
## 整數的除法
```=Ruby
puts 3/2   
puts 3/2.0   
puts 3.0/2       
```

### ==數字也是物件==

## 範圍

```=ruby
(1..10).to_a
(1...10).to_a    #少用
('a'..'z')
```

Ｑ：計算1到100的總和
```=ruby
 total = 0

 (1..100).to_a.each do |x|
   total = total + x
     end

 puts total
```

better way 
```=ruby
p (1..100).to_a.sum
```
```=ruby
p (1..100).reduce { |sum, x| sum + x }
```



# [Array](https://apidock.com/ruby/Array/)



## 為什麼要使用陣列？

跑迴圈或是同時傳入大量參數時。
>Ruby 陣列的特色：
>允許同時存放不同的資料型態
---
產生陣列的方法

```
list = %w(kk mm bb)
```

使用陣列
```=ruby
heroes = ['孫悟空', '魯魯夫', '宇智波佐助', '⼀拳超人', '流川楓','黑崎⼀護', '劍心'];
puts heroes[0] # 印出 孫悟空
puts heroes[1] # 印出 魯魯
puts heroes[-1] # 印出 劍⼼
puts heroes[-2] # 印出 ⿊黑崎一護

puts heroes.first # 印出 孫悟空
puts heroes.last # 印出 劍⼼
puts heroes.second # 印出 nothing

puts heroes.length # 印出 7
heroes << '漩渦鳴⼈人' # 印出 8
heroes.push('布羅利')
puts heroes.length # 印出 9
```
---
 ### ==針對陣列常用的方法：==
 
 ## ==map==
 >對陣列內元素做同樣的處理後，產生新的陣列。 [name=邱宏毅]
 
 ```=ruby
list = [1, 2, 3, 4, 5]
new_list = []
list.each do |x|
    new_list << x * 2
end

p new_list

p list.map { |x| x * 2 }

＃ list不寫的話
p (1..5).map { |x| x * 2 }
```

## ==select==
>挑選符合條件的元素並成為新的集合 [name=Patrick Liu] 
 ```ruby
p (1..10).select { |x| x < 5} #取1234
p (1..10).select { |x| x % 2 == 0} #取偶數
 ```
Q.選出奇數
 ```=ruby
p (1..10).select { |x| x % 2 == 1} #取奇數
 p (1..10).select {|x| x % 2 != 0 }
 p (1..10).select {|x| x.odd? }
 
 #外鄉人的寫法
 
 new_list = []
 
 (1..10).each do |x|
     if x%2 == 1 
         new_list << x
     end
 end
 
 p new_list
 ```
 
## ==reduce==
 對各元素運算歸納成一個結果
 ```=ruby
 p (1..100).reduce {|sum, x| sum + x}
 ```
 ---
# ==[Hash](ruby-doc.org/core-2.5.1/Hash.html)==

What is Hash?

Key 和 Value 的組合

```=ruby
profile = {name: 'kk', age: 18} #新式寫法

p profile

#profile = {:name => "kk", :age => 18} ＃舊式寫法
```


Q. a = [1, 2, 3,1,2,1,3,1,2,3,4,5,6]

```=ruby

b = Hash.new()

p b["nothing"]

c = Hash.new(12)

p c["nothing"]

a = [1, 2, 3,1,2,1,3,1,2,3,4,5,6]

num = Hash.new(0)

a.each do |x|

  num[x]= num[x] + 1

end

p num


#nil
#12
#{1=>4, 2=>3, 3=>3, 4=>1, 5=>1, 6=>1}
```

# [Symbol](ruby-doc.org/core-2.5.1/Symbol.html)

>符號：有名字的物件(==是一個值，不是變數==)
>具有唯一性的特徵，所以不能修改。[name=邱宏毅][color=#00cc00]

```
1       數字物件
"aa"    字串物件
:hello  符號物件
```
```
#irb

age =18

age.class => Integer
:age.class => Symbol
```
### ==字串的內容可以改變，符號不行==

```=ruby
p "hello".object_id #70143757662660
p "hello".object_id #70143757662560
p "hello".object_id #70143757662460

p :hello.object_id #1057308
p :hello.object_id #1057308
p :hello.object_id #1057308
```

### 字串轉符號

```=ruby
p "name".to_sym # 印出 :name
p :name.to_s # 印出 "name"
```

### ==字串與符號的使用時機==
不可變時，優先使用符號。

[Ｄuck Typing](https://zh.wikipedia.org/zh-tw/%E9%B8%AD%E5%AD%90%E7%B1%BB%E5%9E%8B)

>Q.常數，變數和符號的差別？(開放作答)
>>常數跟變數在Ruby裡沒有差別，只差在==命名方式不同==
>>變數可以給值，符號本身就是一個值 [name=唐睿聰]

### ==return 回傳值==
>回傳 ＝ 交回控制權[name=邱宏毅][color=#00cc00]

```
#控制權的交換

def method_name #控制權2
    return ...  #控制權3
end

method_name()   #控制權1 ##控制權4
```
>puts 本身沒有回傳值(=> nil)
>p 卻有回傳值
>return 可適時省略，會自動回傳最後一行的執行結果

### ==問號與驚嘆號==
可以是命名的一部份 但只能放在最後面
>問號通常會回傳真假值
>驚嘆號通常表示要注意！
>>method_name! 預期回傳True/False
>>method_name? 有副作用[color=#00cc00]
```=ruby
def is_adult?(age)
    if age >= 18
      return true
    else
      return false
    end
end

p is_adult?(20)  #true

或是

def is_adult?(age)
    age >= 18
end

p is_adult?(20) #true
```

Q. 函數（function）跟方法（method）有什麼不同？
>function沒有對象 method有對象
>>method有作用對象(物件導向的設計)
>>[color=#00cc00]
>>



## ==模組化==
有個 main.rb 專門require或load 其他rb檔案
>方便分工
>require只會載入一次 load每次執行就載入一次

# ==Block==
>無法單純存活 必須依附在method之後
>>只是一個程式碼區塊[color=#00cc00]

```=ruby
#do..end 與 { } 兩種寫法

5.times { |i|
 puts i
}

5.times do |i|
 puts i
end
```

Q. { } 與 do..end 的差別？！
```=ruby
list = [1,2,3,4,5]
p list.map{ |x| x*2 }
p list.map do |x| x*2 end

###
[2, 4, 6, 8, 10]
#<Enumerator: [1, 2, 3, 4, 5]:map>
```

但有 yield，block會強致執行一次
```=ruby
def say_hello
    puts "hi"
end
say_hello {
    puts "here"
}

puts "there" #hi there

def say_hello
    puts "hi1"
    yield
    puts "hi2"
end
say_hello {
    puts "here"
}

puts "there" #hi1 here hi2 there
```

# ==[Code Examples](https://repl.it/@Chris_Chiu/breed-crumbs)==