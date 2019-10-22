---
title: AstroCamp Ruby 190723
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: Ruby
disqus: hackmd
---

{%hackmd BkVfcTxlQ %}

###### 20190723   tags: `Ruby`

# Method
## Block

不要在Block裡使用Return
```ruby=
def say_hi
    yield 123
    puts "hi"
end

p say_hi{|x|
    puts x
    return 100
}

 #return  交出控制權or結束這回合！！

```
會產生奇怪的斷點
```=ruby



```

```ruby=
def my_select(list)
  result = []
  list.each do |y|
    if yield(y)
      result << y
    end
  end
  return result
end

x = (1..10).to_a
p my_select(x) { |item| item.odd? }

```

>### ==Q.為什麼要使用Block?==
>程式使用起來較有彈性[name=邱宏毅][color=#00cc00]


>## ==Q.Guess what would you get?==
```=ruby
def say
  p yield(123) 
end

say { |x|
  p x

}
```
```=ruby
def say
  puts yield(123) 
end

say { |x|
  p x

}
```
```=ruby
def say
  p yield(123) 
end

say { |x|
  puts x

}
```
```=ruby
def say
  puts yield(123) 
end

say { |x|
  puts x

}
```

>FIFO = First In First Out, Queue
>LIFO = Lasi In First Out, Stack
```ruby=
def hi
    hi()
end
    
hi()  # SystemStackError

```

==Block_given==
block不是參數(只是寄生) 的內建防呆

```ruby=
def say_hello
  if block_given?
    yield
  end
end

say_hello ＃段落呢？？？
```

## ==Proc==

```=ruby
say_hi = Proc.new { puts "hi" }

say_hi.call
###不要用###
#say_hi.()
#say_hi.[]
#say_hi.===
#say_hi.yield
```
```ruby=
young_people = Proc.new { 18 }

case
when young_people
  puts "年輕人"
else
  puts "nothing"
end
#年輕人

young_people = Proc.new { 18 }

case
when young_people.=== #自動執行.===
  puts "年輕人"
else
  puts "nothing"
end

r = 1..10
r.===8    #=> true
r.===11    #=> false
```
>### ==Q.Why Proc?==
>匿名函數的概念[name=邱宏毅][color=#00cc00]


## ==λ lambda==
```ruby=
add_two = lambda { |n| n + 2 }
add_two = -> (n) { n + 2 }

p add_two.call(3)
p add_two[3]
p add_two.(3)
p add_two.===(3)
```

>### ==Q.Proc 和 Lambda 的差別？==
>Lambda 比較像方法，對引數數量上的正確性要求[name=邱宏毅][color=#00cc00]
```ruby=
add_two_proc = Proc.new { |n| n + 2 } 
add_two_lambda = lambda { |n| n + 2 }
p add_two_proc.call(1, 2, 3) # 正常執⾏，印出 3 
p add_two_lambda.call(1, 2, 3) # 發⽣引數個數錯誤
```
# ==OOP==

>### ==Q.在 Ruby裡什麼不是物件？== ###
>開放作答： 尚未被物件化之前的Block

類別的命名規定 = 必須是常數(大寫)
>ex:A辣
>
## Class
```ruby=
class Cat
  def sleep
    puts "zzzz"
  end
end

kitty = Cat.new
kitty.sleep     #zzzz
```
```ruby=
class M60A3
  def 發射!
    puts "boom!"
  end
end

坦克 = M60A3.new
坦克.發射!        #boom!
```

### ==繼承 inheritance==
>繼承，更像是在進行分類

```ruby=
class Animal
  def walk
  end

  def eat
  end
end

class Dog < Animal
end

class Cat < Animal
end

kitty = Cat.new
kitty.eat
來福 = Dog.new
來福.walk

```
### ==Initialize (初始化)==
```ruby=
class Cat 
  def initialize(name)
    puts "born! I am #{name}"
  end
end

kitty = Cat.new("kk") #=> born! I am kk
```
>### new 和 initialize 之間具有先後的關係
```ruby=
class Cat
  def initialize(name)
    puts "我出生了! 我是 #{name}"
  end
end

kitty = Cat.new("kk") #出生了! 我是 kk
```

### ==實體變數(Instance Variable)==
>區域變數是沒有預設值的；
>實體變數的預設值是Nil。
>
>實體變數存在於實體的範圍裡
>實體變數在類別裡可以自由取用

## ==實體方法&類別方法== 
```ruby=
class Cat
  #instance method
  def say
    puts "I'm instance method"
  end
  #class method ，記得加上self
  def self.all
    puts "I'm class method"
  end

end

kitty =  Cat.new#使用kitty 產生實體
kitty.say       #盡在不言中
Cat.all         #盡在不言中
```
>Q.什麼時候需要使用類別方法？為什麼要使用類別方法？
>A.使用上的差異：類別方法更直覺？
>```ruby=
>m = Math.new
>p Math.abs(-10)  # 實體方法
>
>p Math.abs(-10)  # 類別方法
>
>```
>

 ==繼承的進階應用：==
```ruby=
class A
  def abc
    puts "aaa"
  end

end

class B < A
  def abc
    # puts "bbb"
    super(abc) # 呼叫class A 的 abc method
  end
end

kitty = A.new
p kitty.abc  # => "aaa"
```

## ==實體變數與類別變數==

```ruby=
class Cat
  def initialize(name)
    @name = name
  end
#Ruby 並沒有“屬性”的概念，只有方法。
#所以要def 一個 name()，才能正常運作
  def name   # Getter
    @name
  end
#Ruby 並沒有“屬性”的概念，只有方法。
#所以要def 一個 name=()，才能正常運作
  def name=(new_name)  # Setter
    @name = new_name
  end
end

kitty = Cat.new('kitty')
puts kitty.name    # 會印出什麼？
kitty.name = "nancy"
puts kitty.name    # 會印出什麼？
```
### ==外部取用實體變數 - Lazy does matter==
```ruby=
class Cat
    attr_accessor :name  #Getter + Setter
  # attr_reader :name #Getter
  # attr_writer :name #Setter
   
  def initialize(name)
    @name = name
  end

  def say_my_name
    return @name
  end

end

kitty = Cat.new('kitty')

puts kitty.say_my_name
puts kitty.name
puts kitty.name= "patty"
```

## ==Class Variable(類別變數)==
```ruby=
class Cat
  @@counter = 0

  def initialize
    @@counter += 1
  end

  def self.counter  #如果沒有加上self. 會出現錯誤
    @@counter
  end
end

5.times{Cat.new}

p Cat.counter
```

>## ==Q.如果改用實體變數來寫？==
```ruby=
class Cat
    
    def initialize
        @counter = 0
    end
    def go
        @counter += 1
    end
end
kitty = Cat.new
 p kitty.go
 p kitty.go
 p kitty.go

nancy = Cat.new

p nancy.go

```

### ==兩個同名的類別撞在一起不會覆蓋而是融合==
> Open Class
```ruby=

class Cat
 def hello
 end
end
class Cat
 def world
 end
end
kitty= Cat.new

kitty.hello # 會發⽣錯誤？
kitty.world 
```


### ==Open Class 對現存的class 也能作用==
```ruby=
class String
  def bark
    puts %Q("Meow~ saied from #{self})
  end

  #如果是對現存的方法，則會被覆蓋掉
  def length
    100
  end

end

puts "Dog".bark

puts "Dog".length
```

==Monkey Patching==
```ruby=
class String
 def say_hello
 "オッス！オラ#{self}"
 end
end
puts "悟空".say_hello #=> 印出「オッス！オラ悟空」

```
>Other Example:
```ruby=
#open class   改寫原本預設的方法
class Integer 
  def +(n)
    100
  end
end

class Integer
  alias :old_plus :+
  #利用這種寫法，可以有更具彈性的發揮空間
  def +(n)
    puts "hey hey hey"
    self.old_plus(n)
  end
end

puts 1+(2)
puts 2 + 3
```


```ruby=
class Integer
  def day
    self
  end
  alias :days :day 

  def ago
    "#{self} days ago"
  end
end

p 1.day.ago   #1 days ago
p 3.days.ago  #3 days ago
```

paradise?

- [ ]待查 Wait to Study
    - [ ]How to use alias ?
https://www.rubyguides.com/2018/11/ruby-alias-keyword/
    - [ ].send in Ruby ?

## ==Public / Private / Protected==
```ruby=
class Cat
  def hi  
  end

  private
  def gossip
  end
end

kitty = Cat.new
kitty.gossip 

#testwork.rb:13:in `<main>': private method `gossip' called for 
#<Cat:0x00007fb36203b6b0> (NoMethodError)
```


:::info
kitty.say_hello() 在這段程式碼中:
接收者	=>	kitty
訊息	=>	say_hello()
對kitty 這個接收者(receiver)，發送一個say_hello的訊息(message)

private = 不能有明確的接收者
= 在呼叫方法的時候不會有小數點

protected = 不限定是否有明確的接收者
:::

```=ruby
class Cat
  def say
    
    gossip #此時gossip 符合前述，無接收者的要求，可以正常執行
    
    # self.gossip #會出錯
    # bark  #可正常作用
    self.bark #可正常作用
  end

  private  
  def gossip
    puts "You think you know me."
  end

  protected
  def bark
  end
end

kitty = Cat.new

kitty.say

#private is not so private
kitty.send(:gossip) 


# gossip  #private 無法於外部存取
# bark    #protected 無法於外部存取
```
> ==For Fun==
:::success
Try this at Home!!
p self.class.private_methods
:::

