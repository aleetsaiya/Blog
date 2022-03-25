---
title: git reset commit
date: 2022-01-18 13:51:07
---
## 想要去除已提交的 commit 怎麼辦
使用 `git reset` 可以幫助我們清除之前的 commit，而 reset 又有分 `--mixed` ( default ), `--soft` 跟 `--hard`。

`soft` 會保留暫存區的檔案 (`add` 完後的檔案)，我們打的程式碼還在。

`mixed` 會將暫存區的檔案清除，我們打的程式碼也還在。

`hard` 會直接回到指定的版本，中間打的程式碼就都清除了。

```bash
# 將暫存區檔案 (git add) 清除
git reset 
# 回到前一個版本並將程式碼保留在暫存區
git reset --soft HEAD~1 
# 回到前一個版本並清除中間的程式碼
git reset --hard HEAD~1
```

More: [【狀況題】剛才的 Commit 後悔了，想要拆掉重做…](https://gitbook.tw/chapters/using-git/reset-commit)