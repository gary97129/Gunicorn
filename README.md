#  使用Gunicorn將Python Flask API背景執行

## 前言
![image](https://github.com/gary97129/Gunicorn/assets/79617638/73fd57a6-ba4b-4fa5-b039-ce982261231d)  
大家在本機開發時執行 Python 程式應該都是使用 python xxx.py 的方式執行，但我們部署到雲端伺服器時如果使用此方法應該會發現當你關閉終端機時你的程式就會結束服務。本篇文章就會教你如何使用 Gunicorn 部署 Flask 程式並且能夠背景執行。

## 安裝 Gunicorn
Gunicorn 又稱綠色獨角獸(源於icon)是Python Web服務器網關接口HTTP服務器。Gunicorn 服務器與許多Web框架廣泛兼容，並且實現簡單，佔用服務器資源少且速度相當快。此外我們也能選擇使用同步或非同步機制部署你的程式，除此之外也能設定 cpu 的 worker 數量或是 thread 處理。本文就不先講得太複雜，下篇文章我們再來探討這部分設置，本篇的重點在於快速部署一個背景執行的服務。

安裝方法很簡單使用 pip 來安裝 Gunicorn 套件。
```
sudo pip install gunicorn
```

## 範例程式
程式很簡單一樣使用 Python Flask 建立一個很簡單的 API 並監聽 80PORT。
```
# run.py
from flask import Flask
app=Flask(__name__)
@app.route('/')
def hello():
    return 'hello !'
if __name__ == '__main__':
    app.run(host='0.0.0.0',port=80)
```

## 使用 Gunicorn 背景執行
本篇的重點來了我們透過此套件來完成背景執行，輸入以下指令:
```
sudo gunicorn -w 1 -b 0.0.0.0:80 run:app
```
上面的指令分成三部分，第一個部分為 -w 就是 worker 的個數，簡單來說就是同一個時間能夠處理(process)多少工作量。官方是建議一個 CUP 可以設置2-4個 worker。-b 設定服務所綁定的端口，格式為 HOST:PORT。最後一個為執行的程式名稱，假設我的檔名為 run.py 我就要在最後面加上 run:app。

執行後他會顯示目前你的運行狀態，因為我只開一個 worker 因此會看到一個 Process 正被運行中，每一個程序都會有一個 PID，如果我需要停止監聽這項服務就必須利用 kill 指令結束程序。眼尖的朋友應該會發現 worker 是以同步狀態運行，因為我們剛下指令時沒有指定 Gunicorn 的運行模式，因此預設值為 async。下一篇文章我會透過實作讓大家了解非同步與同步的差異以及使用時機。  

![image](https://github.com/gary97129/Gunicorn/assets/79617638/4c827fd7-857e-4541-9680-db5f87bbaed2)  

如果想隱藏執行結果可以在尾巴加上 --daemon 就是真的背景運行了。
```
sudo gunicorn -w 1 -b 0.0.0.0:80 run:app --daemon
```
由下圖可以看到剛剛執行的 run.py 有一個主程序和一個子程序， 主程序的 Pid 為 12705。  

![image](https://github.com/gary97129/Gunicorn/assets/79617638/482bf0d6-f39c-468a-92b8-4c0ecfae9dce)  

使用 kill 指令刪除指定 Pid 隨之子程序也會跟著移除。
```
sudo kill -9 12705
```
或是使用以下指令停止所有 gunicorn 背景。
```
pkill gunicorn
```
## 後記
在本機上開發時通常會直接用 python run.py 直接執行 API，若一樣用這指令測試上線時必須將 debug=False 不然會有記憶體流失 (Memory leak) 問題。
