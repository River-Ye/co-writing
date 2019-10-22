---
title: JS 190911 AstroCamp
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: Javascript
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}

新的一天嗎?
得意的一天
心情點播：[獵人X早安 ( HUNTER X HUNTER)](https://www.youtube.com/watch?v=_bbMY6infq4)
獵人的早安好聽（我愛酷拉皮卡）

![](https://i.imgur.com/KaTfjpQ.png)


==JS物件導向==
某一種資料有幾種特定的操作方式，所以把這些行為跟資料綁在一起，稱之為物件。

物件上的屬性就是資料
物件上的方法就是行為

##我們可以用 object 做到很像 ruby 中 class 的事情
```javascript
let cat = {
 talk: function() { return 'meow'}
}
let c1 = Object.assign({}, cat)
c1.talk
let c2 = Object.assign({}, cat)
c2.talk
```
==唯一問題：object 很浪費記憶體==

initialize  constructor(建構函式)

```javascript
function Person(name){
  this.name = name
  
}

let p = new Person('bob')
console.log(p)

#[object Object] {
  name: "bob"
}
```
如果我們可 p 一個 greeting 方法
```javascript
function Person(name) {
  this.name = name
}
let p = new Person('bob')
console.log(p)

p.greeting = function() {
  console.log('hello')
}
p.greeting()
//[object Object] {
//  name: "bob"
//}
//"hello"
```
可是你如果新作一個 p2 他不能用 p 的 greeting

```javascript
let p2 = new Person('john')
p2.greeting()
//error
```
所以應該放在原型上
```javascript
//建構函式習慣大寫開頭
function Person(name) {
 this.name = name
}
// 把共用的函示綁在原型鍊上
Person.prototype.greeting = function() {
 return `Hello, my name is ${this.name}`
}
let p = new Person('bob')
let r = p.greeting()
console.log(r)
```

## this 是什麼？
那如果我們把這個方法拔出來呢？
```javascript
function Person(name) {
  this.name = name
}
Person.prototype.greeting = function() {
  console.log("Hello I'm " + this.name )
}

let ash = new Person('ash')
ash.greeting()
//"Hello I'm ash"

let grtFn = ash.greeting
// 把 greeting 拔下來，讓他自己變成一個一等公民

grtFn()
//"Hello I'm JS Bin Output "
```
為什麼這裡會找不到 this 勒？如果直接呼叫 grtFn() 他的 context 就變成環境，他會去找這個環境有沒有 name ，如果有就會把他印出來

```javascript
let ash = new Person('ash')
ash.greeting()

let grtFn = ash.greeting
grtFn()

grtFn.call({name: 'cat', age: 100})

let dogGrt =grtFn.bind({name: 'dog', age: 1})
// 直接定義 self 是什麼，把屬性放在 call 裡面
dogGrt()
```
## call 跟 apply 的差別
```javascript
  function foo(x,y) {
    consloe.log(this.number + x + y)
  }
  
  foo(1,2) // NaN
  var number = 10
  foo(1,2) // 13
  
  foo.call({number:999}, 1, 2) // 1002
  foo.apply({number:999},[1, 2]) // 1002
  
  let complexNumber = [5,6,7,8] // get frome ajax
  foo.apply({number:999},complexNumber) // 1002

  
```
起床囉！

map 
```javascript=
let add1 = i => i + 1
add1(10) // 11
[1,2,3,4,5].map(add1) //[2,3,4,5,6]

[1,2,3,4,5].map(function(i) { return `${i+1}`)  
// 可以寫成箭頭函式
[1,2,3,4,5].map(i => `${i+1}`)
// ['2','3','4','5','6']
```
filter
```javascript=

```
reduce

```ruby
ary = [*1..10]

result = 0
ary.each {|i| result = result + i * 10}
ary.reduce(0){ |accu,i| accu + i * 10 }

```
用 reduce 做字串的加總
```javascript=
ary = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

result = 
  ary.reduce('') { |accu, i| "#{accu}#{i}" }
p result
```
百分之 99 的情況 reduce 裡面放的都會是 identity
identity:   0  1 [] {}

實際上用這種 loop 他會從 1 做到 10，不過要得到正確的值，我們並不用按照順序來，可以先做 1 再做 7 再做 10， erlang 裡面有個方法叫做 pmap 他就會看你的電腦有幾核，把它分配給每個核心做完之後再拿回來


```javascript

Array.prototype.inspect = function(i) {
  console.log(this);
  return this;
};

let goodData = { a: 1, b: 2, c: 3 };
let badData = { a: null, b: 2, c: 3, d: null };

function nullKeys(obj, keys = Object.keys(obj)) {
  let result = Object.entries(obj)
    .filter(([k, v]) => keys.includes(k))
    .filter(([k, v]) => v === null)
    .map(([k, v]) => k);
  return result.length === 0 ? "all good" : result;
}

let r1 = nullKeys(goodData);
console.log(r1);
let r2 = nullKeys(badData);
console.log(r2);
let r3 = nullKeys(badData, ["b", "c"]);
console.log(r3);
```


```ruby
good_Data = { a: 1, b: 2, c: 3 }
bad_Data = { a: null, b: 2, c: 3, d: null }

def  null_keys(hsh , keys = hsh.keys)
 result = hsh.select { |k,v| v.nil? }
             .select { |k,v| key.incould? (k) } 
             .map{ |k,v| k }
 result.length == 0 ? 'all good' :result       
end



p null_keys(good_data)
p null_keys(bad_data)
p null_keys(badData,[:b, :c])
```

心情點播： [周杰倫 擱淺](https://youtu.be/YJfHuATJYsQ)

```javascript
//統計大於等於 18 歲的人的姓氏個數

Array.prototype.inspect = function() {
  console.log(this);
  return this;
};

let students = [
  { name: "John Doe", age: 24 },
  { name: "Mary Lee", age: 17 },
  { name: "Bill Doe", age: 2 },
  { name: "Ash Lee", age: 38 },
  { name: "Ryu Doe", age: 18 }
];

let result = students
  .filter(({ age }) => age >= 18)
  .map(({ name }) => name.split(" ")[1])
  .inspect()
  .reduce((accu, name) => {
    if (accu[name]) {
      accu[name] = accu[name] + 1;
    } else {
      accu[name] = 1;
    }
    return accu;
  }, {});

console.log(result);    //{ Doe: 2, Lee: 1 }
```

```ruby
# 統計大於等於 18 歲的人的姓氏個數
students = [
  {name: 'John Doe', age: 24},
  {name: 'Mary Lee', age: 17},
  {name: 'Bill Doe', age: 2},
  {name: 'Ash Lee', age: 38},
  {name: 'Ryu Doe', age: 18}
]

# 方法1
students.select{ |x| x[:age] >= 18 }.map{ |x| x[:name].split(' ')[1] }.reduce({}) { |acc, index|
  if acc[index]
    acc[index] += 1
  else
    acc[index] = 1
  end
  acc
}

# 方法2
students.select{ |x| x[:age] >= 18 }.map{ |x| x[:name].split(' ')[1] }.reduce({}) { |acc, index|
  if acc.has_key? (index)
    acc[index] += 1
  else
    acc[index] = 1
  end
  acc
}

# 方法3
students.select{ |x| x[:age] >= 18 }.map{ |x| x[:name].split(' ')[1] }.reduce({}) { |acc, index|
  if acc[index] == nil
    acc[index] = 1
  else
    acc[index] += 1
  end
  acc
}


```

```javascript
let add = function(x) {
  return function(y) {
    return x + y;
  };
};

let add2 = add(2);
let result = add2(10);
console.log(result);    //12
```

```javascript
let add = function(x) {
  return function(y) {
    return function(z) {
      return x + y + z;
    };
  };
};

//上述程式碼可精簡如下
//let add => x => y => z => x + y + z;

let result = add(2)(10)(100);
console.log(result);    //112
```

curry function
```js
let add = x => y => z => x+y+z
let result = add(2)(10)(999)
conslole.log(result)
```


https://ruby-doc.org/core-1.9.3/Proc.html#method-i-curry 

underscope > lodash > ramda