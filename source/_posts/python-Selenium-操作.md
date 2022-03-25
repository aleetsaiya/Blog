---
title: python Selenium 操作
date: 2021-06-10 07:44:35
---
## 前言
python的selenium可以幫助我們爬取一些難爬的網頁(像是有使用動態載入的網頁)，也可以使用它來對網頁進行自動化的操作。這篇文章紀錄自己常用到的 `webdriver api`。

-------------------
## 下載
+ selenium
```bash
pip install selenium
```
+ webdriver
[ChromeDriver – WebDriver for Chrome](https://chromedriver.chromium.org/)

-----------------
## 操作
### 開始使用 selenium
```python
from selenium import webdriber
driver = webdriver.Chrome('...chromedriver.exe')
```

### 造訪網頁
```python
driver.get('https://aleelive.com')
```

### 搜尋元素
```python
driver.find_element_by_css_selector("input") #搜尋第一個input標籤元素
```

```python
driver.find_element_by_css_selector("input[type='password']")
```

### 獲得 cookies
```python
driver.get_cookies()
```

### 點擊動作
```python
button = driver.find_element_by_css_selector('button')
button.click()
```
### 輸入動作
```python
input_field = driver.find_element_by_css_selector('input')
input_field.send_keys("你想輸入的資訊")
```

### 清除輸入資訊
```python
input_field.clear()
```

### 執行javascript
```python
driver.execute_script("javascript程式碼")
# ex.
driver.execute_script('window.scrollTo(0, document.body.scrollHeight / 5)')
```

### 獲得頁面資訊
```python
page_source = driver.page_source
```
---------------------
基本上比較常用到的 `webdriver api` 就是上面列出來的這些了。