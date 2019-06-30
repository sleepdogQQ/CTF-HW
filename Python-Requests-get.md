#  Python-Requests-get
## 基礎介紹
- Requests這個模組主要可以模擬跟網頁的互動
- 像是常見Post和Get等動作
- 因為要拿來打CTF的關係，這次就研究了關於Get和Header的語法與用法
## 下載說明
- 首先他不是內建的，所以要自行下載，在terminal上使用python的下載指令，如下：`pip3 install requests`
## 使用說明
### 引入
- 當你在python中要使用外來的函式庫時，要先用
```python3=
import requests
```
- 當然像我們本次只會使用到`get`這個指令的話，也可以這樣打
```python3=
from requests import get
```
- 也可以，但要注意呼叫的時候，前著要用`requests.get`，而後者直接打`get`就可以了
### 與網頁互動
- 今天假如要和HackMD的網頁進行連結[連結](https://hackmd.io/)
- 首先把他的網址複製->https://hackmd.io/
- 在python中這樣打
```python3=
import requests
response = requests.get('https://hackmd.io/')
```
- 這個`response`的內容其實就是正常我們進入網頁時，網頁所回給我們的資料
- 說白了，就是F12裡![](https://i.imgur.com/U7xRgHK.png)
- 主要可以看到那Header和Respones的內容
```python3=
print(response.headers) #就會看到有關Headers裡的資訊
```
![](https://i.imgur.com/F5yR4k0.png)
```python3=
print(response.text) #就會看到在Response裡的訊息
```
![](https://i.imgur.com/vWt8PCO.png)

其他像是還是可以查看HTTP狀態碼的指令
```python3=
print(response.status_code) #查看狀態碼
```
![](https://i.imgur.com/oXLKipy.png)

- 既然是模擬Get的部分，那也少不了自訂參數的方法
- 像如果像做到在用Get在url上丟參數，例如想要這樣https://hackmd.io/?BOO=123
- 那就要在丟出`get`時作設定
- 語法如下
```python3=
response = requests.get('https://hackmd.io/', params={'BOO': '123'})
print(reponse.url) # 看握們傳過去之後的往址長怎樣 
```
![](https://i.imgur.com/IL6s9AZ.png)

- 最後還可以自訂我們傳的Header中要新增的元素
- 程式碼如下
```python3=
response = requests.get('https://hackmd.io/', headers= {'XXX': 'QQQ'})
```
- 拿一題CTF時打的當範例
![](https://i.imgur.com/ZAFMti0.png)
- 我們傳果的Headers中含有對X-Token的改變
- 可以錯伺服器回應我們的訊息裡知道，這是有效的

:::info
不管是Get變數，或是新增Headers的內容，所用的都是dictionary的形式喔
:::

### 以上是對Get這個在Python中requests模組中的簡介:smiley: