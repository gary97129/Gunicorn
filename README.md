#  使用Gunicorn將Python Flask API背景執行

## 前言
大家在本機開發時執行 Python 程式應該都是使用 python xxx.py 的方式執行，但我們部署到雲端伺服器時如果使用此方法應該會發現當你關閉終端機時你的程式就會結束服務。本篇文章就會教你如何使用 Gunicorn 部署 Flask 程式並且能夠背景執行。
