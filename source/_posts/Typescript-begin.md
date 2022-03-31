---
title: Typescript-start
date: 2022-03-31 09:14:34
tags:
---

[布魯斯的 TypeScript 入門攻略｜輕鬆打造實時聊天室](https://hiskio.com/courses/628/about) 課程筆記

## Setup
+ Step.1 Install: `npm install -g typescript`
+ Step.2 Init: `tsc --init`
+ Step.3 Setting: 設定 `ts.config`
+ Step.4 Compile: `tsc` 
 
## Setting ts.config
```json
// 我們寫的 index.ts 的資料夾
"rootDir": "./src"
// 設定輸出資料夾
"outDir": "./dist"
// 允許我們使用 js 檔
"allowJS": true
// 開啟這個選項能夠讓我們在 browser 的 console 中直接連結到是哪行 ts 檔輸出的，方便 debug
"sourceMap": true
```