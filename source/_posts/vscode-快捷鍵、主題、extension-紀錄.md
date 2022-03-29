---
title: vscode 快捷鍵、主題、extension 記錄
date: 2022-01-10 15:23:55
categories: Front-end
---

## 快捷鍵
* `ctrl + D` > 選擇當前文字
* `alt + 滑鼠點擊` > 一次操作多個地方
* `shift + alt + 滑鼠拖曳` > 一次操作多個地方
* `shft + alt + ↓` > 複製一行
* `ctrl + shft + K` > 刪除一行
* `ctrl + X` > 剪下一行
* `li*3` > 一次建立3個`li`
* `ctrl + shft + R` > Refactor ，將程式碼打包成 function 


## 主題
* _Night owl_ : 熬夜仔深色系主題，focus 的地方很清楚，用起來蠻舒服的，也是我目前在使用的主題。
* _Ayu Mirage_ : Mosh 推薦的主題，背景色相較 Night owl 淡了一點，可是整體配色很和諧。

## Extension
***colorize***
能夠將 RGB、HSL 代表的顏色，其顏色直接顯示在 code 裡面。

***material icon theme***
讓 file name 前面，顯示檔案類型對應的 icon。

***eslint***
透過它能夠直接在 vscode 中檢查程式碼是否有符合 eslint 規則。

使用 npm 下載 eslint:
* 下載 eslint: `npm install eslint --save-dev`
* 在 `package.json` 的 script 中加入以下指令: (備註省略)
```json
  "scripts": {
    // 初始化 eslint
    "eslintInit": "eslint --init",
    // 使用 eslint 檢查程式碼
    //  --ext 的意思是指定哪些檔案的意思
    "lint-check": "eslint --ext .js,.jsx src/component/",
    // 使用自動修補程式碼
    "lint-fix": "eslint --fix --ext .js,.jsx src/component/"
  },
```

***prettier***
提供能夠讓程式碼更好看的功能。

存檔後自動使用 prettier: 
* 於 vscode 中 file > preferences > [search: formatonsave]，然後勾起來
* 於 vscode 中按 `ctrl + p`，然後輸入: `>format document with`，將預設改為 prettier

***prettier eslint***
參考: [VSCode Prettier 整合 ESLint 自動排版](https://wcc723.github.io/development/2021/04/11/vscode-eslint-prettier/)
如果要結合 eslint 以及 prettier，先從 vscode 下載 `prettier eslint`，並將 `format document with` 設定預設改成 _prettier eslint_，再透過 npm 下載 `eslint`, `prettier` 以及 `prettier eslint` 。
```cmd
npm install --save-dev eslint prettier prettier-eslint
```

***Discord Presence***
讓 discord 顯示正在 coding 的 rich presence

***Live Server***
Launch a development local Server with live reload feature for static & dynamic pages

***Git Graph***
讓我們能直接在 vscode 裡面就看到分支圖