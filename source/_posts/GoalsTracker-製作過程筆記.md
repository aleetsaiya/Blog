---
title: GoalsTracker 製作過程筆記
date: 2022-06-05 02:04:29
tags:
---

_2022/06/05_

GoalsTracker 是我在 2022/06/01 的時候想到的一款 APP，這個 APP 主要的功能是能夠幫助使用者快速的建立一個目標以及透過社群達到跟人分享、堅持的效益。

從開始製作到現在過了 4 天，目前整個 APP 大致的雛形已經有了，只是在一些功能上應該還需要一點時間來去完成，接下來紀錄我從開始製作到現在遇到的一些問題，不管是解決了還是沒解決:

1. 如果要製作一個讓使用者的資料能夠存在資料庫的 APP，**首先要先讓他們登錄**，整個 APP 的操作流程要先想好再開始寫程式
2. 圖片上傳到 Firebase Storage 之前的壓縮方式
3. 如何使用 Redux 建立一個 `async action`
4. input 使用到的 `state` 跟 Redux `store` 裡面紀錄的 `state` 可以分開來使用，不用每次 input value change 都做一次 `dispatch`   
------------------------------

_2022/06/13_

先來解決一下之前遇到的問題:

1. 圖片壓縮的部分我是直接不壓縮了，相對的改成限制使用者上傳的圖檔大小
```js
 const uploadImage = async (e: React.ChangeEvent<HTMLInputElement>) => {
    const file = e.target.files[0];
    // 檔案超過 2MB 的話就不能上傳
    // file size 單位: x 位元組 (bytes)
    if (file.size / (1024 * 1024) > 2) {
      toast.warning("Image size need to less than 2MB");
      return;
    }
 }
```
2. 想要在 action 使用 async request 的方法，可以使用 [redux toolkit](https://redux-toolkit.js.org/) 提供的 [createThunk](https://redux-toolkit.js.org/api/createAsyncThunk) 來幫助我們達成

接下來紀錄現在遇到的問題:

1. 一直在想 database 分成 `users` 跟 `goals` 到底是好還是不好，會不會其實都合在 `users` 裡面反而會比較好寫跟管理 (目前自己想應該不會有答案)
2. 之前想說因為 [react-router](https://reactrouter.com/) 只是負責前端部分的 routes，後端沒有設定的話使用者直接對網址發出請求就會顯示 404 error，這樣就無法達成每個使用者有自己的分頁 (dynamic routes)，而這個問題在 [stack overflow](https://stackoverflow.com/questions/48826489/react-production-router-404-after-deep-refresh-firebase) 這則回應解決了。
3. 目前主要需要解決的是建立 goals 的人要如何追蹤這個 goals 有幾個人參與了以及分享 goals 的方式，就看之後怎麼解決