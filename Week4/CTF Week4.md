---
tags: CTF
---
# CTF Week4
## [CTF網址](https://ctf.hackersir.org/challenges)
### 7/24
- 第一題
    - [pwntools](https://ctf.hackersir.org/challenges#Pwntools?)![](https://i.imgur.com/7cS6las.png)
    - 連過去會發現他要你做一堆加減乘的運算，然後可以用Pwntools來做
    - 結果如下![](https://i.imgur.com/AVxL1VI.png)
    - remote就是連到指定的伺服器和埠號位置，如果你在本地端有EXE可以先測試，適用`process("路徑")`
    - recvuntil就是會一直接收到雨到你指定的字元
    - sendline就是發給他一個字串，就跟你用終端機的手打一樣，但是
    - interactive就是將控制權還給我們，這樣我們進去到終端機就可以換我們操作了
### 7/25
- 第二題
    - [Fake_IP_Again??](http://ctf.hackersir.org:10027/)
    - 題目一開始會跟前幾題中，一題叫Fake_IP的有關，所以這次還是先用X-Forwarded-For來測看看，簡單來說就是指竄改IP的話，結果如下![](https://i.imgur.com/2VF5nGh.png)
    - 他會要求要真正的localhost，於是就再去網路上找找和Host有關的資料，發現在Hearder裡，還有一個管理著host的欄位
    - 這個欄位主要是告訴伺服器，他要在這個IP下，訪問那一個主機，就好像去辦公室裡找某某教授交作業，辦公室的位置就是IP，而那個老師就是Host，那既然叫我要用localhost，就直接把host那欄改成了localhot，答案也就出來了
:::info
Host那欄直接打127.0.0.1也會過
:::
- 第三題
    - [flask_method](http://ctf.hackersir.org:10030/)
    - 一開始進去，只會看到一個C![](https://i.imgur.com/6Dhs1l5.png)
    - 然後看原始碼也是真的什麼都沒有，然後就開始拿題目名稱當關鍵字Google
    - 之後Google到[SSTI](https://xi4or0uji.github.io/2019/01/15/flask%E4%B9%8Bssti%E6%A8%A1%E6%9D%BF%E6%B3%A8%E5%85%A5/)或[python沙盒逃逸](https://www.kingkk.com/2018/06/Flask-Jinja2-SSTI-python-%E6%B2%99%E7%AE%B1%E9%80%83%E9%80%B8/)等漏洞，但看起來都不是這題要的，所以就改從Flask-Method下手，然後發現他會就是告訴Sever，該以什麼方式去看待這個請求，所以我就試著用Burp Suite去攔截這些資訊，然後把Head裡的Get改成其他的模式，如下圖![](https://i.imgur.com/I6LBjc9.png)
    - 然後發現字真的有改變！![](https://i.imgur.com/JfO1u2D.png)
    - 於是就開始再去找其它常見的請求，發現也只有Post、Head、Get、Delete、Put會有東西，但根本構不成我們要的東西
    - 所以就去注意Option（因為只有他回傳是空白又會過），然後就發現他的Response中友告訴你他會列出它會有那用的Head請求，如下![](https://i.imgur.com/JPwiDpk.png)
    - 然後當然是逐一去試啦，但當我全部試玩之後（如下圖）![](https://i.imgur.com/5jkIJxW.png)
    - 他是不規則排序，我根本不知道他的順序，在暴力了一下卻沒有結果之後，我又跑回去看看Response有沒有提示，然後我從Get上看到有一行名叫「Next_Method」![](https://i.imgur.com/hfOeFYF.png)
    - 我瞬間明白了什麼，於是順著順著發，最後就在它又返回C的時候結束
    - 然後就造成了Flag，這題也就解完了





