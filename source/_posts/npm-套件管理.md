---
title: npm 套件管理
date: 2021-07-05 16:31:46
categories: Front-end
---

## 參考 : 
+ [NPM Install 到底做了些什麼？node_modules 檔案結構 + 特性入門](https://ithelp.ithome.com.tw/articles/10191783)
+ [從零開始: 使用NPM套件](https://medium.com/html-test/%E5%BE%9E%E9%9B%B6%E9%96%8B%E5%A7%8B-%E4%BD%BF%E7%94%A8npm%E5%A5%97%E4%BB%B6-317beefdf182)
+ [解開npm安裝包npm install命令的疑惑,區分--save和--save-dev](https://ppfocus.com/0/di8924d63.html)

----------------------------------------------

## 介紹
當我們在 python 中使用 pip 下載一個套件時，那些套件會存在我們的電腦上，所以之後只要在任何一個 python 檔中寫 `import` ... 就可以直接使用那個套件了。而 npm 相較於 pip，他是把每個一個專案要使用的 lib 都裝在每個專案的 `node_modules` 底下，所以會出現"同一個套件下載很多次的情形"，而這樣也方便我們對不同專案使用不同的套件版本。

## 指令
1. `npm init` 會建立 package.json，紀錄我們要使用的套件。
2. `npm install`  如果`install`後面沒加套件名稱的話， npm 會幫你把 package.json 上有寫然後你目前 node_modules 還沒下載的套件安裝下來。
3. `npm install bootstrap` 如果後面有加套件名稱 (像是 bootstrap) 的話，npm 會把你指定的套件安裝到 node_modules ，然後在 package.json 裡面記錄你剛剛安裝的套件。

因為 node_modules 檔案很大所以通常來說不會放上去 github repository，所以如果有一天有一個人從不同台電腦 clone 我們的 repository 的話 (不會包含 node_modules) ，這時他可以輸入 `npm install`，就可以將紀錄在 package.json 上需要用到的套件下載下來到自己的 node_modules 裡。

_於 `npm install`後面加上參數 -g 的話不會將目標下載到 `node_modules` 裡面，而是會下載到我們自己的磁碟裡，`npm config get prefix`可以查看 global 下載到的位置_ 

### scripts
在 package.json 檔案裡面，`scripts`欄位擺的的是平常我們會在 cmd 上打的指令，像是底下這段 scripts

```json
"scripts": {
"compile:sass": "node-sass sass/main.scss /css/style.css"
},
```

在 cmd 上輸入 `npm rum compile:sass` 的話，就等同於在 cmd 上輸入 `node-sass sass/main.scss /css/style.css`，意思是把 sass 資料夾裡面的 main.scss 編譯後存到 css 資料夾內的 style.css。

_於 node-sass 後面加上參數 -w 的話會於 cmd 持續追蹤是否有更新 main.scss_ 