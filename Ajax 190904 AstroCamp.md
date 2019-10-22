---
title: Ajax 190904 AstroCamp
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: Javascript, Ajax 
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}

JSON 一種傳遞資料的格式
endcode & decode
JSON.parse(s)
JSON.stringify(obj)



js傳遞的本質是字串

![](https://i.imgur.com/bPkYlAZ.png) 

javascrip 在設定key的時候可以省略“”
但是在用JSON 傳遞資料時，==不能省略！！==

ajax缺點顧慮list每一頁的畫面，seo會變爛
因為ajax只進行局部更新，所以history 會沒有紀錄（上一頁會失效）

這堂課筆記好難做...
+1 ・゜・(PД`q｡)・゜・

![](https://i.imgur.com/8sUrcuW.png)


- web API : 將setTimeout hold 著 3秒後才把他丟進task queue 
- EvnetLoop : 當stack 是空的時候，才把task queue裡的東西丟進stack

```javascript
function main() {
  start()
  wait()
  end()
}

function start(){
  console.log('start')
}

function end(){
  console.log('end')
}

function wait() {
  setTimout(callback, 3000)
}

function callback(){
  console.log('test')
}
main()
```
因為非同步執行沒辦法馬上拿到資料
所以下面的範例中，如果要操作非同步執行拿到的資料不能寫在下面．只能寫在 function 裡面
```javascript
$.get('https://randomuser.me/api/?results=5', function(data) {
$('.result').html(data.title);
});

// can i get `data` here?
```

![](https://i.imgur.com/q2YmMqz.png)

```javascript=
$(() => {
  console.log('lalala')

  $('#fruit-prices').on('click', 'li', function() {
    alert('lalala')
  })
  let baseUrl = 'https://ubin.io/data'
  let queryParam = '/vegetables?item=香蕉'

  $.get(baseUrl + queryParam, function(data) {
    // console.log(data)
    $('#fruit-name').text(data.item)
    let prices = data.prices.map(function(p) {
      let market = p.market
      let price = p.price
      return `<li>${market} / ${price}</li>`
    })
    $('#fruit-prices').append(prices.join(''))
  })

  console.log('end')
})
```
精簡後
```javascript=
$(() => {
  console.log('lalala')

  $('#fruit-prices').on('click', 'li', function() {
    alert('lalala')
  })
  let baseUrl = 'https://ubin.io/data'
  let queryParam = '/vegetables?item=香蕉'

  $.get(baseUrl + queryParam, function(data) {
    // console.log(data)
    data = data || {item: '', prices: []}
    let {item, prices} = data
    $('#fruit-name').text(item)
    let pricesList = prices.map(({market, price}) => `<li>${market} / ${price}</li>`)
    $('#fruit-prices').append(pricesList)
  })

  console.log('end')
})
```

從第三方接收參數時，避免原地將資料打散，
除非能確定資料永遠不會出錯
promise/A+
callable
thenable
rails relentive
.ajax
```javascript=
$(() => {
  console.log('lalala')

  $('#fruit-prices').on('click', 'li', function() {
    alert('lalala')
  })
  let baseUrl = 'https://ubin.io/data'
  let queryParam = '/vegetables?item=香蕉'

  // $.get(baseUrl + queryParam, function(data) {
  //   // console.log(data)
  //   data = data || {item: '', prices: []}
  //   let {item, prices} = data
  //   $('#fruit-name').text(item)
  //   let pricesList = prices.map(({market, price}) => `<li>${market} / ${price}</li>`)
  //   $('#fruit-prices').append(pricesList)
  // })

  $.ajax({url: baseUrl + queryParam})
   .then(function(data) {
     let {item, prices} = data
      $('#fruit-name').text(item)
      let pricesList = prices.map(({market, price}) => `<li>${market} / ${price}</li>`)
      $('#fruit-prices').append(pricesList)
   })
   .catch(function() {

   })
   .done(function() {

   })

  console.log('end')
})
```

泰安最後的demo


```ruby
# frozen_string_literal: true
class Api::V2::CandidatesController < ApplicationController
  before_action :set_headers
  def index
    @candidates = Candidate.all
    # render json: @candidates 
  end
  def show
    @candidate = Candidate.find_by(id: params[:id])
    # render json: @candidate
  end
  
  private

  def set_headers
    headers['Access-Control-Allow-Origin'] = '*'
    headers['Access-Control-Allow-Methods'] = 'POST, PUT, DELETE, GET, OPTIONS'
    headers['Access-Control-Request-Method'] = '*'
    headers['Access-Control-Allow-Headers'] = 'Origin, X-Requested-With, Content-Type, Accept, Authorization'
  end
end

```
```htmlmixed=
<!DOCTYPE html>
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <title></title>
    <meta name="description" content="">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" href="">
  </head>
  <body>

    <ul id='candidates'></ul>
    <div id='desc'></div>

    <script
      src="https://code.jquery.com/jquery-3.4.1.js"
      integrity="sha256-WpOohJOqMqqyKL9FccASB9O0KwACQJpFTUBLTYOVvVU="
      crossorigin="anonymous" defer></script>
    <script src="./application.js" defer></script>
  </body>
</html>
```
```javascript=
let baseUrl = 'http://localhost:3000/api/v2' 

$(document).ready(() => {

  $.ajax({url: baseUrl + '/candidates.json'})
   .then(data => {
     let candidates = data.map((item) => {
       let {id, name} = item
      return `<li class='candidate' data-id='${id}'>${id} : ${name}</li>`
     })
     $('#candidates').append(candidates)
   })

  $('#candidates').on('mouseover', 'li', function() {
    let id = $(this).data('id')
    let url = baseUrl + `/candidates/${id}.json`

    $.ajax({url})
     .then(data => {
       $('#desc').html(`<pre>${JSON.stringify(data)}</pre>`)
     })
  })
})
```