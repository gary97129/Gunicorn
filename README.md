#  使用Gunicorn將Python Flask API背景執行

## 前言
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
