---
title: hexo 備份
date: 2021-07-11 18:05:21
---

本文參考 : 
[Hexo備份至GitHub](https://www.dazhuanlan.com/2019/09/24/5d89fc809d6bf/)
[30 天精通 Git 版本控管 (25)：使用 GitHub 遠端儲存庫 - 觀念篇](https://ithelp.ithome.com.tw/articles/10140055)
---------------------------------
## 步驟

```bash
git init
# 建立新分支 'hexo' 並切換至 'hexo' 分支  
git checkout -b hexo
git add .
git commit -m 'init'
git remote add origin https://github.com/aleetsaiya/aleetsaiya.github.io
git push -u origin hexo
```

這邊 `git push -u origin hexo` 設定 `-u` 的話，我們之後要 `push` 或是 `pull` 時就不用加後面的 `origin hexo` 了，只要打 : 
```javascript
git pull
git push
```

更多詳細的內容可以至參考文章看。