---
title: eslint prettier 配置
date: 2022-04-11 20:57:30
tags:
---

1. `npm install -D eslint prettier eslint-config-prettier`
2. 在 `package.json` 裡增加 eslint --init
```json
"scripts": {
  "eslint--init": "eslint --init",
},
```
3. `npm run eslint--init`
4. 在 `.eslintrc.js` 增加:
```js
  extends: [
    "prettier"
  ],
```