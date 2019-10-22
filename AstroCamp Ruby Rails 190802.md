---
title: AstroCamp Ruby Rails 190802
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: Ruby, Rails, Rspec
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}

# ==Ruby==

## ==模組==
![Uploading file..._b7s3iz8f6]()
![](https://i.imgur.com/IK6K4Us.png)

>Q.模組和繼承的差別
>旗木卡卡西和宇智波佐助
>

```ruby=
kitty.is_a? Cat
kitty.is_a? Bird

class Bird
  def fly!
    puts "I believe I can fly!"
  end
end

class Cat < Bird
  include Flyable
end

p kitty.class

p Cat.superclass /*Cat的上層*/

p Cat.instance_methods.count /*印出方法數量*/

```

```ruby=
class Module

end

class Class < Module

end

/* p Class.superclass class的上層其實就是模組*/
```
![](https://i.imgur.com/rkjFtla.png)

>==模組沒有繼承的功能==
>==模組不能實體化==
>因為class多出 :new :superclass ( :allocate )

```ruby=
/*模組沒有繼承的功能*/
module Flyable < SomeModule

end

/*模組不能實體化*/
module Flyable

end
wing = Flyable.new
```
![](https://i.imgur.com/DZfnzou.png)

>Q.下面程式碼為印出怎樣的結果
```ruby=
module Flyable

  p "Before fly_method"
  def fly
    p "Fly me to the moon"
  end
  p "After fly_method"

end

class Animal

end

class Bird < Animal
  include Flyable
end




bb = Bird.new

bb.fly
```
![](https://i.imgur.com/wBZmxF0.png)

>Diff between include & extend
>
>include =>實體方法
>extend  =>類別方法
>


### namespace (形容詞)

```ruby=

module A
    class Cat
    end
end

module B
    class Cat
    end
end

kitty = A::Cat.new /*A模組裡面的Cat類別*//*前綴A::*/
nancy = B::Cat.new /*B模組裡面的Cat類別*/

```

```ruby=
class A
  module B
    class C
  end
end

cc = A::B::C.new

```
![](https://i.imgur.com/TLGkl4F.png)

### Ruby 裡的冒號

![](https://i.imgur.com/HFHhCVS.png)

## ==TDD Test-Driven-Development 測試驅動開發==

faker
![](https://i.imgur.com/wR6OgGt.png)

# RSpec
![](https://i.imgur.com/xpjsu2W.png)
![](https://i.imgur.com/OhoAjFq.png)

3A原則：https://ithelp.ithome.com.tw/articles/10185338

![](https://i.imgur.com/lTAlSSD.png)
![](https://i.imgur.com/XcqcKsN.png)
繼續測
![](https://i.imgur.com/0iySai4.png)
![](https://i.imgur.com/ObySBRu.png)
![](https://i.imgur.com/di0AhrN.png)

設定沒問題 再測實際要的結果
![](https://i.imgur.com/QD1Lkuw.png)

原始檔案太長很難看
將class內容開新檔案存在同個資料夾
小心目錄寫法
![](https://i.imgur.com/RtEHUfW.png)
![](https://i.imgur.com/b1LnQgJ.png)

繼續測第二條
![](https://i.imgur.com/5pvX4Ig.png)
![](https://i.imgur.com/cQ40Mk8.png)


### ==重構：優化程式碼，但是不影想原本的功能==


# Rake

![](https://i.imgur.com/VUe8adn.png)

![](https://i.imgur.com/XspLgtL.png)





