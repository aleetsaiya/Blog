---
title: React with Typescript Course
date: 2022-04-05 23:25:07
tags:
---

Bruce Typescript + React 課程筆記

## Setup
1. `npx create-react-app my-app --template typescript`
2. `npm install --save-dev eslint prettier prettier-eslint`
3. add eslint init scripts to `package.json`
```json
{
  "scripts": {
    // eslint --init
    "eslint--init": "eslint --init"
  },
}
```
4. `npm run eslint--init`

> npm run eject 可以在我們 create-react-app 後顯示出被隱藏的 webpack 等設定檔

## Create Functional Component with TS

Functional Component 在 Typescript 裡面的 type 為 `React.FunctionComponent`，也可以簡化成 `React.FC`。
```jsx
import React from 'react'

const App: React.FunctionComponent = () => {
  return <h1>Hello World!</h1>
}

export default App
```

--------------------

## Concepts & Tips
+ 放在 `public` folder 底下的資源都會預設放在根目錄底下形成一個靜態資源
+ `<React.Fragment/>` 可以直接用 `<>` `</>` 來替代

--------------------

## React 網站 render 過程
0. 在我們將程式上架到 Server 的時候，Server 會將我們寫的 typescript 構建 (build) 成 `index.html`, `main.js` 以及 `main.css` ...。
1. Client 在瀏覽器上對 server 發出請求 (Http request)
2. 根據 client 送出的請求找到對應的路由 (`/` or `/about` or `/works` ...) ，server 根據路由回傳對應的 `index.html`, `main.js`, `main.css` ...
3. 瀏覽器接收到 server 傳來的 response，解析 html (空白的 html) 並處理 js (包含 react 的內容)，然後開始執行 `ReactDom.render()`
4. `ReactDom.render()` 將 html 渲染到 `<div id="root">`