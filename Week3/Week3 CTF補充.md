---
tags: 未整理
---
# Week3 CTF補充
## flask相關
### send_file指令
- 此指令拿來回傳指定路徑的語句
- 舉個例子
```flask
@app.route('/js/<path:path>')
def send_js(path):
    return send_file('js/'+path)
```
- `path:path`前者是變數名稱，你接再js/後面的字都會被令作path，代表下面的path，而後者則是變數的型態，所以如果後面是int，那他們就會被轉成int型態（通常不會這樣啦）。
- 然後就是send_file了，他會回傳下面的路徑給伺服器，讓他回傳你所指定的檔案
- 所以如果你傳的長這樣`/js/QQ.js`，那path就會等於QQ.jg而接到下面的send_file後，就會回傳給你QQ.js的內容了

## LFI相關
### 參考網址
- [網站一](https://blog.csdn.net/weixin_33851177/article/details/91272023)
- [網站二](https://www.itread01.com/p/1412642.html)
- [網站三](https://blog.csdn.net/u014549283/article/details/81301623)
- [網站四](https://www.jianshu.com/p/0a59c40183e5)
- [網站五](https://www.cnblogs.com/wh4am1/p/6542398.html)
### 成因
- 主要是PHP中的函數不當管理而產生不當使用的空間
- 主要的函數有
    1. include
    2. require
    3. include_once
    4. require-once
- 當URI資料作為’include()’函式的一個引數時,攻擊者可以提交POST PHP 命令來執行。 
### 利用方法
1. 使用php://filter協議和base64編碼去訪問本地文件得到原始碼
- EX:`http://127.0.0.1/LFI.php?include=php://filter/read=convert.base64-encode/resource=func1.php` 
2. 使用zip://去訪問壓縮文件中的文件
- EX:`http://127.0.0.1/LFI.php?include=zip://func1.jpg#func1.php`
3. 使用phar://去讀取服務器端的phar文件，執行phar文件中的php腳本或者其他。這需要事先本地創建一個phar文件再上傳到目標站點
```php=
<?php
$phar = new Phar('webshell4.phar');
$phar->addFile('createwebshell.php', 'cws.php');
?>
```
- 第一行事建立一個phar類型的文件，這種檔案類似Java的Jar檔（歸檔檔案），主要拿來放一堆php類型的文件，但跟Jar不同的是Jar可以用壓縮文檔打開(因為Jar本身也延伸自zip)，而phar不是，當然不能用普通的壓縮文檔打開，但可以用編輯器打開，會發現他是用特殊格式（哪裡特殊我還不知道ＱＱ）
- 通過`http://127.0.0.1/LFI.php?include=phar://webshell.phar/cws.php`就可以執行cws.php這個腳本了，而且`webshell4.phar`可以上一些詞綴來饒過一些檢查

## Proxy（代理伺服器）簡介
### 參考網址
- [網站一](http://macgyver.info.fju.edu.tw/docs/whatisproxy.html)

### 來由
- 原指防火牆的功能描述，因為在內部與外部網路之間建立一道牆，所以進出都要通過它，且是看不到牆內內部的結構的，內外網路的進出都要通過它，所以又稱作「代理主機」，所以相對來說需要紀錄很多的資料來源與目的，進而需要大量的硬碟空間來存放這些資訊
### Proxy的好處
- 假如今天每個都要看一場比賽，每個人都要跟那個網站要一次，這樣會浪費寬頻和減慢速度，而今天大家都先連到一台Proxy Server上，那Proxy會先幫第一個要的人代理去那個網站要資料，然後後面一樣要的人，就可以先到Proxy去看有沒有要的資料，那他就不用直接去目標網站送一份訊息了，而且這時候會使用的是內網網路，通常會比你用外部的網路來的快速。
- 簡而言之，他更「快」（有點像快取的感覺）
### Proxy的壞處
- 容易被內網的人濫用
- 有可能快取的資訊是舊的，導致沒有拿到正確的資訊

## h2