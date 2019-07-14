---
tags: 程式學習
---
# Python
## 基本簡介
- 某個人為了打發時間在聖誕節期間寫的，這人叫Guido von Rossum
    - 優點：一個易讀、好上手、支援和擴展性都高的語言
    - 缺點：運算效率沒有C++和Java好，所以大量集中運算時不建議使用python
## 下載說明（請原諒我只有一台電腦，而且是MAC）
- MAC上自帶Python2的，可是要用Python3也只要去[官網](https://www.python.org/downloads/)下載就可
## 推薦自學網站-[Python-100days](https://github.com/jackfrued/Python-100-Days)
## 好用的介面工具
1. Jupyter
    - 在終端機上下`pip install jupyter`
    - 如果你是用python3下`pip3 install jupyter`
    - 下載完了就下`jupyter notebook`
    - 然後就會出現下圖（這個放著又好，你關了jupyter也會跟著關喔）![](https://i.imgur.com/siVzLpt.png)
    - ![](https://i.imgur.com/6otZUES.png)
    - 先進到Desktop，然後看到有上角的New，點開就可以新增一個Python檔了![](https://i.imgur.com/xZzpjQT.png)

:::info
- 順便說一下，所謂的pip是拿來管理python套件的系統，所以要下載相關工具當然要通過他，而pip如果沒意外是用在python2，pip3用在python3
- 這時候你應該會想問，一台電腦的python會有兩種版本？答案是可以的，你可以同時有python2和python3的喔
:::
2. VSCode
    - [官網在這](https://code.visualstudio.com/)
    - 沒毛病的話，進去後長這樣![](https://i.imgur.com/4JnQ1HV.png)
    - 然後看左上角會有這行![](https://i.imgur.com/ef1eY71.png)
    - 點從上樹下來第五個，裡面就可以搜尋python，沒什麼問題下載第一個就好![](https://i.imgur.com/gkw4nWO.png)
    - 以後有機會要擴充其他功能再來也不遲

## 覺得需要注意的文法
### '和"
```python=
print("Hello World!")
print('Hello World')
```
:::info
Note：不需要;，'和"沒有差
:::
### 丟變數的語法
```python=
print("%d + %d = %d"%(a,b,a+b))
```
:::info
Note：丟變數直接接在%後面就好了，然後裡可以、做運算
:::
### import的語法
```python
import math
from math import pi
```
:::info
別把from和import的順序搞混了
:::
### Bool比較的一些規則
```python=
a = 1
b = 2
b > a == True
b >= a == True
b => a#不能這樣接喔，比較符號一定要在左邊
a =< b#同上
```
### 改變結尾的東西
```python=
print('AA')#自帶換行
print('S',end=' ')#換行被替代成空白，所以會跟下一個接在一起
print("leep")
```
### 模組的呼叫
```python=
import math
#假如果今天要用math中的factoreial這個函數，那我就要連math一起打
#除非你當初是這樣引進的 from math import factoreial
a = factoreial(4)#這樣不會過
a = math.factorial(4)#這樣才會
```
### 列表生成
```python=
a = [x for x in range(1,10)]
b = [1,2,3,4,5,6,7,8,9]
#這兩者一樣，都會產生[1, 2, 3, 4, 5, 6, 7, 8, 9]
```
### 列表、字典、元組、集合
#### 列表
```python=
a = [aa,bb,cc]
b = [1,2,3]
c = ['a',1]
#以上都是列表(list)
```
#### 字典(dictionnary)
```python=
a = {'Leo':1,'Kevin':2,'Jerry':'3'}
```
:::info
Note1：放入不同的屬性是ok的
Note2：注意是用:而不是=喔，更不是=>
:::
- 取值的兩種方法
```python=
score = {'Leo':1,'Kevin':2}
print(score['Leo']
print(score.get('Leo'))
```
:::info
Note：不同的除了有沒有get還有中小括號喔
:::
#### 元組
```python=
t1 = ('aa','bb',34,43)#這是元組，跟列表(list)是兩回事
#最大的差別在於能不能改裡面的值，tuple不行，但list可以
t1[0]='cc'#所以這是不可行的，會噴錯
```
#### 集合
```python=
set1 = {1,2,3,4,5}
set2 = set(range(1,7))#{1, 2, 3, 4, 5, 6}
#集合基本上是拿來看子集那些的，就是你數學上學的那些
print(set1 >= set2)#Flase
print(set1 <= set2)#True
print(set2 >= set1)#True
print(set2 >= set1)#Flase
```
#### 總結一下
```python=
list1 = [1,2,3,'5','6','7']
dictionnary1 = {'Leo':1,'Kevin':2}
tuple1 = (1,2,3,'5','6','7')
set1 = {1,2,3,5,6,7}
#小括tuple、中括list、大括dictionary或set
```
### 自己遇到的一些奇怪函數
#### yield
[可參考網站一](https://openhome.cc/Gossip/Python/YieldGenerator.html)
[可參考網站二](https://eastlakeside.gitbooks.io/interpy-zh/content/Generators/Generators.html)
- 先來看看這兩個城市
```python=
def fib(n):
    a = b = 1
    result = []
    for i in range(n):
        result.append(a)
        a, b = b, a + b
    return result
```
```python=
def fib(n):
    a, b = 0, 1
    for _ in range(n):
        a, b = b, a + b
        yield a#主要用這個在回傳資料
        # return a#這反而行不通
for val in fib(5):
        print(val)
```
- 以上兩者都是生成費波那契數列，但他們用不同的方法來生成，return和yield，差別在於他們成生的東西佔記憶體的大小，後者(yield)會明顯較小
![](https://i.imgur.com/rXWCXBz.png)
![](https://i.imgur.com/rrHraTe.png)
- 簡單來說，他是一個（生成器），這時候就要說說迭代的機制了（更詳細可以參考網站二）
- 迭代是生成器一個特別的特性，他也會生成值，但並不會整個存到你的內存，你可以通過遍歷來找到他的值（像是用前面的next），而且return有個特性是會中斷你的程式，但yield並不會
- 好處呢？剛說了他不會一次把所有值都存在你的內存，所以內存大大減少了，效率當然就更好了啊
- 字串也可以有迭代的效果喔，如下
```python=
s1 = "WOWAOA"
iter1 = iter(s1)
print(next(iter1))#W
print(next(iter1))#O
print(next(iter1))#W
print(next(iter1))#A
print(next(iter1))#O
print(next(iter1))#A
```
#### bytearray.fromhex("想解碼的十六進制").decode()
```python=
a = bytearray.fromhex("4920616D20536C656570646F67").decode()
print(a)#I am Sleepdog
```


### 某些函數的活用
### for
[可參考網站](https://openhome.cc/Gossip/Python/ForComprehension.html)
```python=
s1 = [('Leo',95),('Kevin',90),('Jerry',85)]
a = {name : score for (name,score) in s1}#莫名的列表轉字典
print(a)

b = {i for i in 'omaewa mou shindeiru'}
print(b)
```
