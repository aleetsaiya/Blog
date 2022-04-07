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

--------------------

## Styled-Component with Typescript

### Setup:
1. `npm install --save styled-components`
2. `npm install --save @types/styled-components` (如果要用 Typescript)
3. Create styled-component 
> 可以在 vscode 下載 [vscode-styled-components](https://marketplace.visualstudio.com/items?itemName=styled-components.vscode-styled-components) 讓我們能在他定義寫法下有 highlight

```js
import React from 'react'
import './App.css'
import styled from 'styled-components'

// 定義 styled-component 的 props
type ButtonProps = {
  colorStatus: boolean
}

// 建立 styled-component
const Button = styled.button<ButtonProps>`
  background: transparent;
  border-radius: 3px;
  border: 2px solid palevioletred;
  color: ${(props) => (props.colorStatus === false ? 'black' : 'red')};
  margin: 0 1em;
  padding: 0.25em 1em;
`

const App: React.FC = () => {
  return (
    <>
      <Button colorStatus={false}>Click</Button>
    </>
  )
}

export default App
```

----------
## 自己建立 Hook
[React docs](https://zh-hant.reactjs.org/docs/hooks-custom.html)

要用 `use` 開頭來命名 hook，不然就無法起到 hook 的檢查機制。

課堂中示範自己建立一個 HOC (High Order Function) 的 Hook:
```js
import React, { useState } from 'react'

// 定義如何計算出 score
function getCurrentSocre () {
  return 100
}

function getScoreByBoardName (boardName: string) {
  if (boardName === 'boardA') return 200
  else if (boardName === 'boardB') return 1000
  return 0
}

// 自定義 HOC hook (High Order Function)
function useScore (boardName: string) {
  const [socre, setScore] = useState(
    getCurrentSocre() + getScoreByBoardName(boardName)
  )
  // 回傳 score 以及 setScore 並定義他們的 type
  return [socre, setScore] as [
    number,
    React.Dispatch<React.SetStateAction<number>>
  ]
}

const ScoreBoardA: React.FC = () => {
  const [score, setScore] = useScore('boardA')
  // 使用回傳回來的 setSocre 對 score 進行更新
  const handlePlus = () => {
    setScore(score + 10)
  }
  return (
    <>
      <p>Total Score: {score}</p>
      <button onClick={handlePlus}>Plus</button>
    </>
  )
}

const ScoreBoardB: React.FC = () => {
  // eslint-disable-next-line
  const [score, setScore] = useScore('boardB')
  return <p>Total Score: {score}</p>
}
```
-------
## 使用 UseEffect 結合 API Request
> 範例中使用 json placeholder 來模擬與後端 fetch 資料的情形

使用 `useEffect`，在 ComponentDidMount 的時候獲取 data:
```js
// 定義 API request 回傳的資料型態
type Comment = {
  postId: number
  id: number
  name: string
  email: string
  body: string
}

const App: React.FC = () => {
    // 在接收到 data 的時候定義接收到的資料型態
  useEffect(() => {
    fetch('https://jsonplaceholder.typicode.com/comments?postId=1')
      .then((res) => res.json())
      .then((data: Comment[]) => console.log(data))
  }, [])
}
```
將 `fetch` 拿到 `useEffect` 外，自己寫一個 async function 並透過斷言 (assertion) 來去指定型態
```js
type Comment = {
  postId: number
  id: number
  name: string
  email: string
  body: string
}

const App: React.FC = () => {
  // 定義 async function 
  async function fetchData () {
    const res = await fetch(
      'https://jsonplaceholder.typicode.com/comments?postId=1'
    )
    // Use assertion to define type of data
    const data = (await res.json()) as Comment
    return data
  }

  useEffect(() => {
    fetchData()
  }, [])
}
```

使用 `useEffect` 結合更換 id (頁數) 獲得不同 data、控制錯誤處理以及 loading status:
```js
type Comment = {
  postId: number
  id: number
  name: string
  email: string
  body: string
}

const App: React.FC = () => {
  const [postId, setPostId] = useState(1)
  // 設定 error types 為 null 或是 Error
  const [error, setError] = useState<Error | null>(null)
  const [loading, setLoading] = useState(false)

  // 定義 async function 獲得 data 
  async function fetchData (id: number) {
    // 開始 fetch 時，設 loading 為 true
    setLoading(true)
    try {
      const res = await fetch(
        `https://jsonplaceholder.typicode.com/comments?postId=${id}`
      )
      const data = (await res.json()) as Comment
      console.log(id, data)
    } catch (error) {
      setError(error as Error)
    }
    // 結束時設為 false
    setLoading(false)
  }

  // 當 postId 變動的時候，useEffect 會觸發並 fetchData
  useEffect(() => {
    fetchData(postId)
  }, [postId])
  )
}
```

-------------

## 將處理 API 相關的程序自定義成一個 Hook
將與 API 相關的程序從原本的 Component 提取出來，可以讓 Component 更能夠專注在他的工作上面。

完整程式碼:
```js
import React, { useState, useEffect } from 'react'

// 定義從 API Request 回傳回來的 data type
type Comment = {
  postId: number
  id: number
  name: string
  email: string
  body: string
}

// 自定義一個 Hook
function useFetchApi () {
  // postId:   要抓 API 裡面的哪一塊資料 (page)
  // error:    當 error 發生時會從原本的 null 變成 type of Error
  // loading:  處理 loading status
  // data:     從 api request 獲得的 data

  const [postId, setPostId] = useState(1)
  const [error, setError] = useState<Error | null>(null)
  const [loading, setLoading] = useState(false)
  const [data, setData] = useState<Comment[]>([])

  async function fetchData (id: number) {
    setLoading(true)
    try {
      const res = await fetch(
        `https://jsonplaceholder.typicode.com/comments?postId=${id}`
      )
      const resData = (await res.json()) as Comment[]
      setData(resData)
    } catch (error) {
      setError(error as Error)
    }
    setLoading(false)
  }

  // 當 postId 發生變動時，重新 fetchData
  useEffect(() => {
    fetchData(postId)
  }, [postId])

  return [data, loading, error, setPostId] as const
}

const App: React.FC = () => {
  // data, loading, error 因為都是 state，會隨著我們呼叫 setPostId 而改變
  const [data, loading, error, setPostId] = useFetchApi()

  function handleClick (id: number) {
    setPostId(id)
  }

  return (
    <>
      <h1>Fetch API</h1>
      <button onClick={() => handleClick(1)}>Id 1 data</button>
      <button onClick={() => handleClick(2)}>Id 2 data</button>

      {error === null
        ? (
        <p style={{ color: 'green' }}>資料獲取成功</p>
          )
        : (
        <p style={{ color: 'red' }}>資料獲取失敗</p>
          )}
      {loading ? <p>loading...</p> : null}
      <p>結果:</p>
      {data.length > 0 && data.map((item) => <p key={item.id}>{item.email}</p>)}
    </>
  )
}

export default App

```

----------
## useRef

`useRef` 與 `useState` 的其中一個差別在: `useRef` 在更新其值的時候不會觸發畫面的重新渲染，而 `useState` 在更新其值得時候會使畫面重新渲染。

> React會確保 useRef 回傳出來的這個物件不會因為 React 元件更新而被重新創造。所以當我們的 component re-render 時，ref.current 的值不會被改變

```js
const App: React.FC = () => {
  const [hidden, setHidden] = useState(true)
  const sumRef = useRef(0)

  function click () {
    sumRef.current = sumRef.current + 1
    if (sumRef.current >= 5) {
      setHidden(false)
    }
  }

  return (
    <>
      <h1>Ref</h1>
      <button onClick={click}>+1</button>
      {!hidden && <div>被隱藏的區塊</div>}
    </>
  )
}
```

使用 `useRef` 來達成 `document.querySelector` 的效果:

```js
const App: React.FC = () => {
  const h1Ref = useRef<HTMLHeadingElement>(null)

  useEffect(() => {
    // 相較於使用: const h1 = document.querySelector('h1')
    const h1 = h1Ref.current
    console.log(h1) // log: <h1>My page title</h1>
  }, [])

  return <h1 ref={h1Ref}>My page title</h1>
}
```