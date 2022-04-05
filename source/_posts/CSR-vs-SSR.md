---
title: CSR vs SSR
date: 2022-04-05 16:31:35
tags:
---
## CSR (Client-Side Render)
> CSR 的問題在於 SEO 不佳以及造訪網頁的第一個畫面會有較長空白等待時間

用戶從 server 拿到的 `index.html` 其內容是空白的 (還沒渲染)，必須要透過執行 Javascript 將網頁內容渲染到 HTML 上面。如果附帶的 Javascript 還沒執行完，用戶就只能看到空白的頁面 (對使用者體驗以及 SEO 會有負面效果)。

優點:
+ 降低伺服器的運算以及流量的負荷量
+ 透過 Javascript 更新整體頁面，routing 時畫面不需要一片白之後再重新渲染

## SSR (Server-Side Render)
用戶從 server 那邊接收到 `index.html` 之前，HTML 就已經被渲染出來了 (pre-render)。所以使用者比起等待空白頁面，會覺得網站的響應速度會比 SPA 還要快。

缺點: 
+ 伺服器一次需要計算完畫面再將整頁送出，這樣對於伺服器的運算及流量的負荷都比較大
+ Routing 時因為要重新跟 Server 要新的頁面，所以會閃白更新

## Isomorphic JavaScript
將上面兩個方式結合，第一個顯示的畫面由伺服器端計算處理 (SSR)，剩下的畫面由 Javascript 處理 (CSR)。

像是 [Next.js](https://nextjs.org/) 就能處理 SSR。


------------------------------
Ref: 
+ [前端三十｜18. [FE] 為什麼網站要做成 SPA？SSR 的優點是什麼？](https://medium.com/schaoss-blog/%E5%89%8D%E7%AB%AF%E4%B8%89%E5%8D%81-18-fe-%E7%82%BA%E4%BB%80%E9%BA%BC%E7%B6%B2%E7%AB%99%E8%A6%81%E5%81%9A%E6%88%90-spa-ssr-%E7%9A%84%E5%84%AA%E9%BB%9E%E6%98%AF%E4%BB%80%E9%BA%BC-c926145078a4)
+ [初探 Server-Side-Rendering 與 Next.js](https://medium.com/starbugs/%E5%88%9D%E6%8E%A2-server-side-rendering-%E8%88%87-next-js-%E6%8E%A8%E5%9D%91%E8%A8%88%E7%95%AB-d7a9fb48a964)