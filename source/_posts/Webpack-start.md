---
title: Webpack start
date: 2022-03-31 10:59:32
tags:
---
[布魯斯前端 Webpack 快速入門](https://www.youtube.com/watch?v=uP6KTupfyIw) 筆記

> Webpack 能夠幫我們將多種不同的檔案打包

## Basic Setup
1. `npm install webpack --save-dev`
2. 建立 `webpack.config.js`
```js
// 調用 path module in node.js
const path = require("path");

module.exports = {
  entry: "./src/index.js",
  output: {
    // path.resolve 能夠從左到右將相對路徑轉成絕對路徑
    // __dirname 為當前資料夾位置
    path: path.resolve(__dirname, "dist"),
    filename: "main.bundle.js",
  },
  // mode 可以為 development | production  
  mode: "development",
};
```
3. 在 `package.json` 內增加 scripts 用來讓 webpack compile 檔案
```json
"scripts": {
  // compile
  "build": "webpack",
  "watch": "webpack --watch",
},
``` 

More about `module`: [visit](https://ithelp.ithome.com.tw/articles/10185008)

## Dev Server
如果要建立一個 local server 的話，webpack 有提供方法來幫助我們建立

1. 在 `webpack.config.js` 內加入: 
```js
module.exports {
  // ...
  // ...
  devServer: {
    // 設定 static html location
    static: {
      directory: path.join(__dirname, "dist"),
    },
    compress: false,
    port: 9000,
  },
}
```

2. 在 `package.json` 中加入 script
```json
"scripts": {
  // create a local server   
  "dev": "webpack server"
}
```

## CSS Loader
> Loader 能夠幫助我們處理非 Javascript 的檔案，像是 css、scss、圖片、字體 ...。

在沒有加入 CSS Loader 的情況下，Webpack 預設只能讀的懂 Javascript，所以在我們 `import` css 檔案後程式會報錯。這時就要加入 CSS Loader

Docs: [Webpack css-loader](https://webpack.js.org/loaders/css-loader/#root)

1. `npm install --save-dev css-loader style-loader`
2. 在 `webpack.config.js` 內加入:
```js
module.exports {
  // ...
  // ...
  module: {
    rules: [
      {
        // 用正則表達式跟 webpack 說看到的所有 css 檔案都要用底下的 loaders 解析
        test: /\.css$/i,
        use: ["style-loader", "css-loader"],
      },
    ],
  },
}
```

## Plugin
### html-webpack-plugin
如果要避免 cache ，希望每次 bundle 出來的 js 檔都要有不一樣的名字，可以使用這個 plugin。它能夠幫我們自動生成 html 檔，以及透過設定 `[hash]` 將 js 檔的名稱增加亂數並自動在 html head 中嵌入

1. `npm install --save-dev html-webpack-plugin`
2. 在 `webpackage.config.js` 內加入:
```js
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports {
  plugins: [
    output: {
      path: path.resolve(__dirname, "dist"),
      // 增加 [hash] 讓我們執行 npm run build 的時候會產生新的亂數檔
      filename: "main.[hash].bundle.js",
    },    
    new HtmlWebpackPlugin({
      // 這邊加入 template 是為了讓產生出來的 dist/index.html 會依照 src/index.html 的內容樣本來產生，不然不會有內容
      template: "./src/index.html",
    }),
  ],
}
```

### MiniCssExtractPlugin
如果我們使用上述引入 css，會發現 css 都在 `<head/>` 裡面，因此可以使用這個 plugin 來幫助我們建立一個額外的 css 檔

> Note that if you import CSS from your webpack entrypoint or import styles in the initial chunk, mini-css-extract-plugin will not load this CSS into the page. Please use html-webpack-plugin for automatic generation link tags or create index.html file with link tag.

docs: [Webpack MiniCssExtractPlugin](https://webpack.js.org/plugins/mini-css-extract-plugin/)

1. `npm install --save-dev mini-css-extract-plugin`
2. 在 `webpackage.config.js` 內加入:
```js
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: [MiniCssExtractPlugin.loader, "css-loader"],
      },
    ],
  },
  plugins: [new MiniCssExtractPlugin({
    // 增加 [hash] 讓 bundle 出來的檔案有亂數，如果沒有要產生亂數的話不用傳入參數也可以
    filename: "main.[hash].css",
  })],
}
```