---
tags: Wargame
title: Wargame
---
# Wargame
- 網址[https://overthewire.org/wargames/]
## 簡介
- 左邊每一個不同的項目都是不同的系列題目，但進去會有有他的每一關(從Level0開始)
- 在Level0的地方會跟你說每一個系列大概長怎樣，主題是什麼，怎麼操作這樣
- 據我所知，Bandit是網路協定相關，也可以看作是玩終端機，Natas是跟網頁有關
- 然後就可以開始玩玩看了
## Bandit
### Level1
![](https://i.imgur.com/hQtzJmH.png)
- 第一關要你用ssh來遠端連線一台主機，下面是提示和一些可能用到的指令
- 啊不知道ssh的連線或是看規則的提示都可以知道要怎麼查
- 既然他有給主機位置，所以就簡單的試驗看看囉`ssh bandit.labs.overthewire.org`![](https://i.imgur.com/ZEfaPvJ.png)
- 但結果好像不太對，於是再去查查看，原來還要指定port號
- 於是去找找看要怎麼輸入port，打`man ssh`就可以看到相關的指令![](https://i.imgur.com/4x2cfH8.png)
- 然後就看到加port好的方式上去![](https://i.imgur.com/H7RCosl.png)
- 結果還是失敗了，於是就去找找是不是漏了什麼
- 然後就發現他其實每一關都有一個帳號與密碼設定，而Level的帳號和密碼都是bandit0
- 所以就再去找是否漏了帳號，果不其然，應該要家帳號，而加的方法就是ssh -p [port] [帳號]@[主機位置]
- 所以再重新試試看![](https://i.imgur.com/nUIEKbZ.png)
- 然後就成功進來了，來就是找看看這個主機下有什麼![](https://i.imgur.com/M9CFenm.png)
- 之後就成功找到下一關的密碼了
### Level2
![](https://i.imgur.com/atYIthp.png)
- 上來就告訴你他放了一個叫located的檔案在這裡面
- 所以我們就再去看看這裡有什麼東西
- ![](https://i.imgur.com/VW2xFnQ.png)
- 然後就會發現很弔詭的事情，為什麼`cat -`沒有東西 明明就有一個叫-的檔案，然後去[查了一下](https://unix.stackexchange.com/questions/16357/usage-of-dash-in-place-of-a-filename)，如果在終端機上貌似會內建成一個輸入輸出檔，而他的名稱就是-，所以才會發生我輸入`？`他也給我`？`的情況，但這並不代表就不能cat底下這個-檔
:::info
我想這個-檔應該跟內建的-是兩個檔案，一個是內建設好的，一個是另外生成的
:::
- 所以要去檢索這個被另外生成又叫-的檔案，就必須要指定事當前這個目錄的檔案
- 所以指令就變成了這樣![](https://i.imgur.com/AtLEn3f.png)
- 於是就會拿到下一題的密碼了
### Level3
![](https://i.imgur.com/6a0VF8g.png)
- 他說有一個檔案在終端機中一個叫spaces in this filename的檔案裡
- 於是我就![](https://i.imgur.com/mrovPlz.png)
- 看起來很蠢，但我是用快捷鍵，所以才會饒過這題的空白，在終端繼上打空白正常是要加一個\的如果直接打 系統並不會認為是一個空格，但如果你打出關鍵字案tab他就會跑出正確的格式了**囧**
### Level4
![](https://i.imgur.com/XtEygXE.png)
- 上來就告訴你有一個檔案**隱藏**在名為inhere的資料夾裡
- ![](https://i.imgur.com/YR3cjSa.png)
- 因為在做以前的一些調整時就知道，當你在檔案面前家.時，他就不會被正常的看到，但前面的例子就可以發現，`ls`或許發現不了，但`find`是會丟出所有在這個目錄底下的檔案的，當然這也包含了那些隱藏檔
- 所以指令就變成這樣![](https://i.imgur.com/4dg1u0y.png)
- 先用find找出檔案名稱，然後cat的檔案名稱明確，其實還是有辦法cat到的
### Level5
![](https://i.imgur.com/J7iHZG2.png)
- 他有一個只有人類讀得懂的檔案在inhere這個資料夾裡，所以我們當然是帘裹去看看到底裡面長怎樣囉
- ![](https://i.imgur.com/XJMyEIG.png)
- 想說反正總有一個是人讀得懂的，那就全部cat吧![](https://i.imgur.com/bAn7LUK.png)
- 很顯然的，沒有我想的那麼簡單，於是我去查查看這個提示訊息是什麼意思
- 但我不管查了`cat`還是`file`的`--help`，都沒有特別針對檔案解讀的指令，所以我又回到了原點上看看，然後我就在看了一次find![](https://i.imgur.com/3PSbzma.png)
- 該不會是-的問題吧，於是我依照上一題的打法，在cat時打一個明確的檔名看看
- 結果如下![](https://i.imgur.com/N09UuTz.png)
- 啊怎麼跟之前不一樣，我的天，於是我就每個都再掃一遍![](https://i.imgur.com/qNDZW1A.png)
- 結果就在第七個檔案時找到了他的正解



#### 補充
[reset和clear之差別-1-英](https://unix.stackexchange.com/questions/61584/how-to-solve-the-issue-that-a-terminal-screen-is-messed-up-usually-after-a-res)
[reset和clear之差別-2-中](https://blog.csdn.net/linczone/article/details/79119883)
