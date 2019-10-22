---
title: Webpack and Webpacker 190905 AstroCamp
description: 整理筆記
robots: noindex, nofollow
lang: zh-tw
dir: ltr
breaks: true
tags: Javascript, Ruby, Rails 
disqus: hackmd
---
{%hackmd BkVfcTxlQ %}

早起的蟲兒被鳥吃
早起的小菜被蟲吃

babel
https://babeljs.io

package.json
mkdir myProject
cd myProject/

$npm init 
之後都按enter
生出package.json
![](https://i.imgur.com/gMBsiCV.png)
![](https://i.imgur.com/HRCjxa9.png)

![](https://i.imgur.com/laivCnJ.png)
```bash
# 
which yarn
# reshim
asdf reshim nodejs
```
```bash
# npm
package-lock.json
# yarn
yarn.lock
```

Babel 要支援哪些可以去這邊查 =>presents => @babel/preset-env
![](https://i.imgur.com/MmPlGS1.png)

https://github.com/browserslist/browserslist

https://xkcd.com/
有中文版頁面 https://xkcd.tw
https://classicprogrammerpaintings.com/
寫code寫累了可以看

![](https://i.imgur.com/BQXkSnQ.png)


14:10 webpacker
https://github.com/rails/webpacker
![](https://i.imgur.com/FTXj3Cy.png)
生出下面的資料夾
![](https://i.imgur.com/7nyDjnF.png)
![](https://i.imgur.com/aK11mkS.png)
![](https://i.imgur.com/nr0OhK0.png)
 ![](https://i.imgur.com/TFb991i.png)

mac linux()
lsof -i :3000
kill -9 -上面指令的PID號碼    



下午，刺激的來了喔！！
https://github.com/rails/webpacker
1. 安裝webpacker
```ruby
#Gemfile
gem 'webpacker', '~> 4.x'
```
記得 bundle
bundle exec rails webpacker:install
yarn upgrade

把<%= javascript_pack_tag 'application' %>貼到app/views/layouts/application.html.erb

1. view下開一個叫spa的資料夾，把昨天的spa.html檔丟進去，改名為index.html（只留ul和div）
2. 把昨天的.js檔複製到app/javascript/packs/application.js檔裡
3. 在routes建立路徑：get '/spa', to: 'spa#index'
4. 建立spa_controller
5. 把昨天html的CDN貼到application.html.erb（/head上方），把defer刪除
6. http://localhost:3000/spa 應該可以動



暫時借放
yarn add jquery lodash moment

在package.json會新增jquery lodash moment

```javascript
// config/webpack/environment.js
const { environment } = require('@rails/webpacker')
const webpack = require('webpack')

environment.plugins.append('Provide', new webpack.ProvidePlugin({
  $: 'jquery',
  jQuery: 'jquery',
  'window.jQuery': 'jquery',
  _: 'lodash',
  moment: 'moment'
}))

// environment.plugins.append('CssUrlRelative', new CssUrlRelativePlugin())

// environment.loaders.get('sass').use.splice(-1, 0, {
//   loader: 'resolve-url-loader'
// })

module.exports = environment
```