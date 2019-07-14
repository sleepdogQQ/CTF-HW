---
tags: CTF
---
# CTF Week3
## [CTF網址](https://ctf.hackersir.org/challenges)
### 7/9
- 第一題
    - [Flask](http://ctf.hackersir.org:10028/)
    - 首頁就告訴你flag再/flag，但你直接輸入`http://ctf.hackersir.org:10028/flag`是沒有用的，查看[程式碼](http://ctf.hackersir.org:10028/app)，就會知道因為伺服器沒有查/flag的函式
    - 所以我們只好繞道，用../來回去上一個路徑，因為send_file是直接複製貼上的概念，所以可以貼上js/../會抵銷，就可以回去到根目錄了
    - 但在試了還是不行，原來瀏覽器會自動抵銷掉../的部分，所以`js/../flag`=`/flag`，而剛說過直接使用/flag是沒有用的，簡而言之，不用能瀏覽器來做。
    - 那只嘗試用curl和requests了
        - 用curl的情況![](https://i.imgur.com/se291wm.png)
        - 用requests的情況![](https://i.imgur.com/wPzYs64.png)
    - 正當覺得都不行要令找其他方法時，看到requests裡某一行訊息![](https://i.imgur.com/E9xIsCd.png)
    - 不覺得好像跟一開始目標不太一樣嗎？我們的/flag在根目錄，如果照上面的路徑，我們找的是/app/flag，因此我們在加一個../看看
    - 而答案就會在眼前了
    - 原來他查找時路徑的檔案，是從app這個資料夾開始的，所以還要再多一行才對
:::info
- 順帶一提，用正確的路徑「直接」丟curl還是不行喔![](https://i.imgur.com/8WodNE5.png)
- 因為其實curl也會把../過濾掉，但有一個指令可以刪掉他，打`curl -h`查查看，然後可以發現這一行![](https://i.imgur.com/MMJOytf.png)他會取消js/../這個壓縮的功能，所以就會正常的把../../flag傳入了
- 所以最後要打這樣`curl --path-as-is ctf.hackersir.org:10028/js/../../flag`
:::
### 7/10
- 第二題
    - [❓](http://ctf.hackersir.org:12004/)
    - 這題一進去又看到了page這個找路徑的參數，在經過上一次的LFI之後，就先輸入page=php://filter/read=convert.base64-encode/resource=./index來找這個index.php是怎麼寫的（記得他回傳的是base64加密，所以還要解密回去）
    - 結果如下![](https://i.imgur.com/yMbfG8h.png)
    - 在看完下面的if之後只知道給page的東西且不能沒有或沒設定過，或包含到config，他才會把他當作路徑，不然就給你首頁
    - 然後就瞄到上面有一個login.php又被跳過，於是想說這一定有鬼的書在url上，結果不意外的出現了這個![](https://i.imgur.com/PcMZns8.png)
    - 然後一樣用page=php://filter/read=convert.base64-encode/resource=./login的方式把他的原始碼弄出來
    - 結果如下![](https://i.imgur.com/PTAImRH.png)
    - 很好!是用SQL驗證，所以就索性試了一下常見的SQL注入![](https://i.imgur.com/ZMpziKR.png)
    - 結果如下![](https://i.imgur.com/QP5h8f9.png)
    - 回首頁了!?
    - 原來他只會驗證你有沒有登入而已，其他根本沒有東西會出來，簡單來說，被耍了
    - 於是轉換試試看有沒有可能是單純一點的問題
    - 就打上了robots.txt，結果出現這個![](https://i.imgur.com/5Q5xH5d.png)
    - 於是就高興的去/admin看看
    - 出現了類似Terminal的視窗，當然是給他ls，看有沒有什麼檔案，結果如圖![](https://i.imgur.com/EAzkwWB.png)
    - 然後就把index.php改成f1lll1l1ll111llaggggggggg.php
    - 結果裡面是空白的，什麼都沒有
    - 左看右看沒什麼可疑的東西，想說那只好用cat掃看看，結果發現command有限定字數，所以不能完整的打上去，而且只能有7個字cat .php也塞不下，看來只能令尋出路了
    - 既然用介面不行，他就用curl來送參數看看，結果curl結果如下![](https://i.imgur.com/gpNLCFM.png)![](https://i.imgur.com/0SLiJMK.png)
    - 失敗了 但不知道問題在哪
    - 於是想到了前面的LFI，就用它來看看index.php裡的審查機制寫什麼
    - `http://ctf.hackersir.org:12004/index.php?page=php://filter/read=convert.base64-encode/resource=./admin/index`
    - 結果如圖![](https://i.imgur.com/iNxHUX6.png)
    - 很好，不能用cat，而且還是會被限定字數
    - 整當我沒什麼進展的時候，我才想到原來用LFI去看f1lll1l1ll111llaggggggggg.php裡面長怎麼不就好了
    - 於是url又改成了`http://ctf.hackersir.org:12004/index.php?page=php://filter/read=convert.base64-encode/resource=./admin/f1lll1l1ll111llaggggggggg`
    - 然後答案就出來了.....
- 第三題
    - [Session_easy](http://ctf.hackersir.org:10029/)
    - 這題考的是php用session時做驗證時，有可能會被看到使用者的session_id，進而被利用的情況
    - 裡面直接給你他的過程![](https://i.imgur.com/rfv02RN.png)
    - 先來說說session_start這個函式，他會先去找你在Cookie裡有沒有一個叫做PHPSESSID(session_id)的值，如果有找到，就會去資料庫裡找對應的資料，沒有的話去URL上找，有就拿去找資料，又沒有的話，就看內部怎麼生成，隨機給你一個。
    - 而在php的常規下，存session_id對應資料的地方，名稱通常會是"`sess_"+"你的sesssion_id"`這樣的形式在儲存
    - 所以這題就是去找你在cookie的session_id，然後他好心的給了你一個名為"fox"的URL參數，去加在sess_後面，所以就只要把?fox=你的seesion_id，就會看你的資料（這題的答案）
### 7/11
- [Hacker101](https://ctf.hacker101.com/ctf?congrats=many)
- 第四題
    - [Trivial](http://34.94.3.143/3232a7c8e5/)
    - 想說第一題都會在F12裡，果斷打開來看
    - ![](https://i.imgur.com/fHNbv0O.png)
    - 沒有！沒事，他說有個背景叫`background.png`，還加了url
    - 很明顯，加在原網址後面就行了

