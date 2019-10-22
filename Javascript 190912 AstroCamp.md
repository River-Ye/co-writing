---
title: Javascript 190912 AstroCamp
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: Javascript 
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}



React 是 facebook 出的前端框架
使用任何前端框架之後，後端就變成一個純的API



react 核心： single source of truth
每個事情都要問他，他再決定要給這些元素什麼樣的值或狀態
ex:藍色的commentboxComponent


react  在操作的對象是Virtual DOM ，和Jquery的相性不合

Component Way 

在下面例子中，藍色框框會把知識交給綠色框框請他把這些comment show 出來，但是一旦有人改變 comment ，他就需要把這個知識丟給藍色框框，藍色框框再跟綠色說，現在有一批新的知識，麻煩你重新 show
![](https://i.imgur.com/fEvcdas.png)

在寫 React 的時候，能把知識放在越底下越好，一旦發現做不到，就把知識往上提一層

```htmlmixed=
<body>
  
  <div id='container'> </div>
<script src="https://fb.me/react-15.1.0.js"></script>
<script src="https://fb.me/react-dom-15.1.0.js"></script>
<script src="https://fb.me/react-with-addons-15.1.0.js"></script>

</body>
```
## 開始寫 React 囉！
```javascript=
function App() {
return (<h1>lalala</h1>)
}

ReactDOM.render(<App />, document.getElementById('container'))
```

這裏的 ``<h1>lalala</h1>`` 很像是 html 但其實是 Javascript
React 會幫忙把這段翻譯成原生的語法

要怎麼在畫面中加入邏輯：
1. erb, html 等樣版引擎
2. Angular :在 html 裡面發明自己的 tag
3. React: 把 code 變成 template

## ==泰安開心的做出網頁炸彈==
```javascript=
function App() {
return (<h1 onClick={alertTenTimes}>lalala</h1>)

}

function Comment() {
 return (
   <div>
    <div id= 'user'> aaa</div>
    <p>This is comment</p>
   </div>)
}

function alertTenTimes () {
  [1,2,3,4,5,6,7,8,9,10].map(
  ()=> alert('hohoho!'))
}

ReactDOM.render(<App />, document.getElementById('container'))
```
React 每次只允許產生一個節點，如果要一次產生多個，要用一個父節點包起來
```javascript=

function App() {
  return (
      <div>
      <h1 onClick={alertTenTimes}> lalala </h1>
      <p> hooo </p>
      </div>
  )
}
function alertTenTimes() {
  [1, 2, 3, 4, 5, 6, 7, 8, 9, 10].map( () => {alert('hooo')})
}

ReactDOM.render(<App />, document.getElementById('container') )
```


Add <Comment />

```javascript=

function App() {
  return (
      <div>
      <h1 onClick={alertTenTimes}> lalala </h1>
      <p> hooo </p>
      <Comment />
      </div>
  )
}


function Comment() {
 return (
   <div>
    <div id= 'user'> aaa</div>
    <p>This is comment</p>
   </div>)
}
function alertTenTimes() {
  [1, 2, 3, 4, 5, 6, 7, 8, 9, 10].map( () => {alert('hooo')})
}

ReactDOM.render(<App />, document.getElementById('container') )
```


Add Props

```javascript=

function App() {

let comStr = 'trans from props'
  return (
      <div>
      <h1 onClick={alertTenTimes}> lalala </h1>
      <p> hooo </p>
      <Comment comStr={comStr}/>
      </div>
  )
}


function Comment(props) {
 return (
   <div>
    <div id= 'user'> aaa</div>
    <p>{props.comStr}</p>
   </div>)
}
function alertTenTimes() {
  [1, 2, 3, 4, 5, 6, 7, 8, 9, 10].map( () => {alert('hooo')})
}

ReactDOM.render(<App />, document.getElementById('container') )
```
這裏的 props 是怎麼做的？其實 Comment 後面那堆資訊，React 就是單純的把它轉成Object，因此才可以用 props.bar 的方式把它印出來

---

安裝 webpacker
https://github.com/rails/webpacker
Gemfile
gem 'webpacker', '~> 4.x'

$ bundle
$ bundle exec rails webpacker:install
$ bundle exec rails webpacker:install:react

建 app/views/spa/react.html.erb 這個檔案

把打包好的js檔案加入進去
![](https://i.imgur.com/9FjAcdi.png)

```htmlmixed=
<html>


<head></head>
<body>
  <div id='container'></div>
  <%= javascript_pack_tag 'app' %>
</body>
</html>
```
![](https://i.imgur.com/7yy9VON.png)

```js
import React from 'react'
import ReactDOM from 'react-dom'
import PropTypes from 'prop-types'

let App = () => (<h1>Hello</h1>)

document.addEventListener('DOMContentLoaded', () => {
  ReactDOM.render(
    <App />,
    document.body.appendChild(document.createElement('div')),
  )
})
```

```jsx
import React from 'react'
import ReactDOM from 'react-dom'
import PropTypes from 'prop-types'

let App = () => {
  let times = 10
  return(
    <div>
      <h1>Hello, you clicked {times} times</h1>
      <button id = "btn">Don't Click </button>
    </div>
  )
}
  
document.addEventListener('DOMContentLoaded', () => {
  ReactDOM.render(
    <App />,
    document.body.appendChild(document.createElement('div')),
  )
})
```

```jsx=
let App = () => {
  let times = 10
  return(
    <div>
      <h1>Hello, you clicked {times} times</h1>
      <button id = "btn" onClick={ () => console.log('lalala')}>Don't Click </button>
    </div>
  )
}
  
document.addEventListener('DOMContentLoaded', () => {
  ReactDOM.render(
    <App />,
    document.body.appendChild(document.createElement('div')),
  )
})
```
因為 js 中的 class 已經是關鍵字，所以你要給 class 的時候要改成 className
![](https://i.imgur.com/v1BE16w.png)
相同的還有 colSpan 跟  rowSpan
```jsx
<button id = "btn" className="someBtn" onClick={ () => console.log('lalala')}>Don't Click </button>
```
Chrome 新增擴充功能:
React Developer Tools
^^^^^^^^^^^^^^^^^^^^^^^ 會被 AdBlock 擋掉...OTZ
![](https://i.imgur.com/NoEmuyP.png)

React 文件推薦必讀：
[hooks](https://reactjs.org/docs/hooks-overview.html#state-hook)
[context](https://reactjs.org/docs/context.html)

如果你希望之後的人使用第一眼看到要看到主程式，就要把小函式往下放，但是匿名函式往下放的話會提升，會變成 undefined ，所以要變成具名函式放下面
```jsx
let App = () => {
  let times = 10
  return(
    <div>
      <h1>React Yeah!!!</h1>
      <button id = "btn" className="someBtn" onClick={ () => console.log('lalala')}>Don't Click </button>
      < Comment />
    </div>
  )
}
  
function Comment() {
  return(
    <p> Some Content
    </p>
  )
}
```
## Using Hook
```javascript=
let App  = () => {
  let [times, setTimes] = useState(0)
  return (
    <div>
      <h1>React Yeah! </h1>
      <button id = "btn" onClick={ () => setTimes(times + 1)}>
        Don't Click 
      </button>
      <Comment times={times}/>
    </div>
  )
}

function Comment(props) {
  return (
    <div>
      <p>Hello, you clicked {props.times} times</p>
    </div>
  )
}
```


```javascript=
let App = () => {
  let [times,setTimes]=useState(0)
  return (
    <div>
      <h1>React Yeah!!! </h1>
      <Btn times = {times} setTimes={setTimes}/> 
      <Comment times={times}/>
    </div>
  )
}


function Comment(props) {
  return <p>
    hello,you clicked {props.times} times
  </p>
}
function Btn(){
  return (
    <button id="btn" 
            className= "someBtn" 
            onClick={()=> setTimes(times+1)}>
      Don't click        
    </button> 
  )
}

document.addEventListener('DOMContentLoaded', () => {
  ReactDOM.render(
    <App />,
    document.body.appendChild(document.createElement('div')),
  )
}
)

```
```javascript=
let App = () => {
  let [times, setTimes] = useState(0)
  return (
  <div>
    <h1>ヽ(=^･ω･^=)丿</h1>
    <Btn times={times} setTimes={setTimes} />
    <InputField setTimes={setTimes} />
    <Comment times={times}/>
  </div>
  )
}

  function InputField({setTimes}) {
    let [inputTimes, setInputTimes] = useState(0)
    return(
      <div>
        <input type="text" value={inputTimes} onChange={evt =>{
          console.log(evt.target.value)
          setInputTimes(evt.target.value)
        }}/>
        <button onClick={() => {
          setTimes(inputTimes)
          setInputTimes('')
          }}>Save</button>
      </div>
    )
  }

  function Btn({times, setTimes}) {
    return (
      <button id="btn" 
         className="someBtn" 
         onClick={() => setTimes(times + 1)}>
      Don't Click!
      </button>
    )
  }

function Comment(props) {
  return<p>
     Hello, you have already clicked about {props.times} times!
  </p>
}

document.addEventListener('DOMContentLoaded', () => {
  ReactDOM.render(
    <App />,
    document.body.appendChild(document.createElement('div')),
  )
})
```
## closure
```javascript=
for(var i = 1; i < 6; i++) {
    $('#l' + i).on('click', function() {
    alert(i)
    })
}
```
為什麼不管怎麼點都是 alert (6)?

```javascript=
for(var i = 1; i < 6; i++) {
    $('#l' + i).on('click', (function(i) {
      return function() {
        alert(i)
      }
    })(i))
}
```
or
```javascript=
//curry function
for(var i = 1; i < 6; i++) {
    $('#l' + i).on('click', (i => () => alert(i))(i))
}
```

<!-- Closure Sucks!! -->



