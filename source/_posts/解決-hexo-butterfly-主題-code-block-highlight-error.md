---
title: 解決 hexo butterfly 主題 code-block highlight error
date: 2021-06-10 01:06:27
---

我將之前的markdown文章轉移至 hexo 後，發現雖然設定好了 code block 的語言，可是整個程式碼區塊還是只有白色，爬文後找到解決方法。

根目錄/_config.yml : 
```yml
highlight:
  enable: true
  line_number: true
  auto_detect: true
  tab_replace: ''
  wrap: true
  hljs: false
```

themes/butterfly/_config.yml : 
```yml
highlight_theme: mac #  darker / pale night / light / ocean / mac / mac light / false
highlight_copy: true # copy button
highlight_lang: true # show the code language
highlight_shrink: false # true: shrink the code blocks / false: expand the code blocks | none: expand code blocks and hide the button
highlight_height_limit: false # unit: px
code_word_wrap: false
```