---
title: 使用requests抓取facebook影片留言
date: 2021-06-10 00:33:20
---
## 前言
大家好，我之前有寫過使用selenium抓取facebook影片留言，只是因為selenium爬完之後感覺有點沒勁，再加上有前輩指導說使用 mbasic.facebook.com進行爬蟲會比較方便，所以這篇文章就誕生啦！雖然其實不是全部使用requests，在登入的步驟還是有用到selenium，但比起之前的版本還是快上好幾倍，如果有興趣的話歡迎各位往下看，馬上來介紹我是如何使用requests抓取facebook影片留言。

## 整體架構
步驟：
+ 登入facebook
+ 送出請求
+ 解析留言
+ 輸出csv檔

其中登入facebook的部分我是使用selenium來完成的，原本其實是打算使用requests.Session()獲得cookies，可是不知道為什麼在測試了幾次之後，原本能成功獲得cookies卻突然不行了，所以最後還是使用selenium，之後找到原因可能會在修改。

## 步驟
### 登入facebook
登入的步驟就是很直觀的使用selenium找到輸入欄位，並輸入自己的帳號密碼送出。
```python
#透過selenium登入獲得cookies
def login(self):
    #driver設定
    driver = webdriver.Chrome(self.driver_location)

    #登入動作
    driver.get('https://www.facebook.com/')
    input_1 = driver.find_element_by_css_selector('#email')
    input_2 = driver.find_element_by_css_selector("input[type='password']")

    input_1.send_keys(self.account)
    input_2.send_keys(self.password)
    driver.find_element_by_css_selector("button[name='login']").click()
    time.sleep(1)
```
### 從selenium獲得cookies
登入完成之後，我們就要把剛剛登入成功的cookies給存下來，使用 driver.get_cookies() 獲得cookies後，再將獲得的cookies放入請求中。
```python
#獲得登入cookies
cookies = driver.get_cookies()

s = requests.Session()

#將cookies放入session中
for cookie in cookies:
    s.cookies.set(cookie['name'], cookie['value'])
```
### 送出請求
因為剛剛已經獲得了cookies，所以現在加上headers後，就來送出請求。
```python
#headers
headers = {
    'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/89.0.4389.114 Safari/537.36',  
}

response = s.get(url, headers=headers)
```
### 解析留言

這個部分真的把我搞得很頭痛，原本想說直接根據留言的class抓取留言內容，可是實測後發現class會隨著不同影片改變(ex. a影片class=”ab”；b影片class=”ae”)。所以後來我是使用正則表示法，根據留言內容的id來判別(留言內容id長度都為15~16，由數字組成)。

使用正則表示法獲得每一則留言的id，再使用BeautifulSoup根據這些id來找出每則留言。(這是我第一次使用正則表示法，所以如果寫得很醜的話請見諒XD)
```python
#獲得每則留言html id
cmid = re.findall('id="\d{15,16}', response.text)
cmid_list = []
for i in cmid:
    ii = i.split('"')
    cmid_list.append(ii[1])

#根據 id存每則留言
soup= BeautifulSoup(response.text, 'html.parser')
comments = []
for id in cmid_list:
    comments.append(soup.find('div', id=id))

#分析留言內容
for comment in comments:
    name = comment.find('h3').text
    #print(name, end=' ')
    msgs = comment.select('div')
    msgs_s = ''
    for msg in msgs:
        if not re.findall('讚 · 傳達心情', msg.text) and not re.findall('已回覆 · \d+ 則回覆', msg.text):
            #print(msg.text)
            msgs_s += msg.text
    dic = {}
    dic['name'] = name
    dic['msg'] = msgs_s
```
### 輸出csv檔
輸出csv檔的話使用pandas DataFrame，將資料輸出到指定資料夾內。
```python
#輸出至目錄底下資料夾
def to_csv(self, dir_name):
    out_dir = './' + dir_name
    out_name = '留言內容.csv'
    df = pandas.DataFrame(data = self.data)
    if not os.path.exists(out_dir):
        os.mkdir(out_dir)
    fullname = os.path.join(out_dir, out_name)    
    df.to_csv(fullname, encoding='utf_8_sig', index=False)
    print('輸出至', dir_name, '資料夾')
    print('輸出完成')
```

## 結語
做完這個之後感覺自己的爬蟲實力有比之前還要進步，過程中學習到蠻多的，完成之後也覺得很有成就感，若是看完這篇文章希望對你有所收穫 ~ 