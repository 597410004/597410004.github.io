## 技术栈
1. request
2. beautifulsoup
3. flask
## 思路
1. 选取爬取的天气网址
2. 分析获取天气的接口及接口返回信息
3. 解析接口返回内容
4. 解析内容并编写自己接口实时返回天气
## 难易程度 ★★☆☆☆
## 代码

```
# encoding=utf-8
import json
import re
import time

import requests
from bs4 import BeautifulSoup
from flask import Flask, request

app = Flask(__name__)


@app.route('/weather/get', methods=['POST'])
def show_user_profile():
    try:
        data = request.get_data()
        dict1 = json.loads(data)
        response = {'code': 200,
                    'data': json.loads(get_weather((dict1['weather'])))}
        return json.dumps(response, ensure_ascii=False)
    except:
        return json.dumps({'code': 500})


def get_weather(city_name):
    mill = int(round(time.time() * 1000))

    headers = {'Accept': '*/*',
               'Accept-Encoding': 'gzip, deflate',
               'Accept-Language': 'zh-CN,zh;q=0.9,en;q=0.8',
               'Connection': 'keep-alive',
               'Cookie': 'vjuids=-14768fb78.163f88bf07a.0.ee06aa72c6622; f_city=%E5%8C%85%E5%A4%B4%7C101080201%7C; UM_distinctid=166621631e8368-02bcbf8e9ce7fb-9393265-1fa400-166621631e9641; Hm_lvt_080dabacb001ad3dc8b9b9049b36d43b=1539706259,1539832348,1539832353,1539856676; vjlast=1528883311.1547965750.21; Wa_lvt_1=1547965750; Wa_lpvt_1=1547965761',
               'Host': 'd1.weather.com.cn',
               'Referer': 'http://www.weather.com.cn/weather1d/101010100.shtml',
               'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36}'}
    r = requests.get('http://d1.weather.com.cn/sk_2d/' + read_city(city_name) + '.html?_=' + str(mill), headers=headers)
    r.encoding = 'utf-8'
    return parse_html(r.text)


def parse_html(source):
    soup = BeautifulSoup(source, 'html5lib')
    text = soup.body.text
    return text.split('=')[1]


def read_city(city_name):
     with open('./citycode.txt', mode='r', encoding='utf-8') as r:
        for line in r.readlines():
            if city_name in line:
                return re.split('=', line)[0]


if __name__ == '__main__':
    app.run(host='localhost', port=8089)
    # print(json.loads(get_weather('南京')))


```
## github

[天气](https://github.com/597410004/python-weather-api)


