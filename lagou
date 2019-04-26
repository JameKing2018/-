# -*- coding: UTF-8 -*-
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver import ActionChains
from time import sleep

from requests_html import HTMLSession
from requests_html import AsyncHTMLSession as ah

from openpyxl import Workbook

base_url = "https://www.lagou.com/zhaopin/"
base_url = base_url


def crawl():
        lg = []

        page = 1
        session = HTMLSession()
        headers = {
            'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8', 'Accept-Encoding': 'gzip, deflate, br', 'Accept-Language': 'zh-CN,zh;q=0.9,en;q=0.8', 'Connection': 'keep-alive', 'Cookie': '_ga=GA1.2.812393592.1556158097; _gid=GA1.2.439158839.1556158097; index_location_city=%E4%B8%8A%E6%B5%B7; user_trace_token=20190425100834-4f920035-bf29-49c5-8e91-a33c9eb1ae46; LGUID=20190425100846-0ef30aa8-66ff-11e9-b597-525400f775ce; JSESSIONID=ABAAABAAAFCAAEG7C1F9AFAFC4D9EF955D93652FB77C5E5; Hm_lvt_4233e74dff0ae5bd0a3d81c6ccf756e6=1556158097,1556173065,1556173703,1556243101; LGSID=20190426115357-eadf8160-67d6-11e9-b7d7-525400f775ce; PRE_UTM=; PRE_HOST=; PRE_SITE=; PRE_LAND=https%3A%2F%2Fwww.lagou.com%2Fzhaopin%2F; SEARCH_ID=33eceee11fb84ec4b5d3c9093e9f3ed1; X_HTTP_TOKEN=5907a0c99e0ebdf100115265511998af9bf8233ae0; Hm_lpvt_4233e74dff0ae5bd0a3d81c6ccf756e6=1556251089; LGRID=20190426115820-87c6dc6d-67d7-11e9-9d14-5254005c3644', 'Host': 'www.lagou.com', 'Origin': 'https://www.lagou.com', 'Upgrade-Insecure-Requests': '1', 'Referer': 'https://www.lagou.com/zhaopin/3/', 'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.100 Safari/537.36'
        }
        sleep(page)

        while page <= 30:

            url = base_url+str(page)
            re = session.get(url, headers=headers)

            length = len(re.html.xpath("//*[@id='s_position_list']/ul/li"))
            page = page+1

            i = 1

            while(i <= length):
                position = {}
                base_xpath = "//*[@id='s_position_list']/ul/li["+str(i)+"]"
                position['company'] = formatstr(re.html.xpath(
                    base_xpath+"/div[1]/div[2]/div[1]/a/text()"))
                position['position'] = formatstr(re.html.xpath(
                    base_xpath+"/div[1]/div[1]/div[1]/a/h3/text()"))
                position['salary'] = formatstr(re.html.xpath(
                    base_xpath+"/div[1]/div[1]/div[2]/div/span/text()"))
                position['location'] = formatstr(re.html.xpath(
                    base_xpath+"/div[1]/div[1]/div[1]/a/span/em/text()"))
                position['posttime'] = formatstr(re.html.xpath(
                    base_xpath+"/div[1]/div[1]/div[1]/span/text()"))
                position['slogen'] = formatstr(re.html.xpath(
                    base_xpath+"/div[2]/div[2]/text()"))
                position['biaoqian'] = ''
                j = 1
                if (j <= len(re.html.xpath(base_xpath+"/div[2]/div[1]/span"))):
                    position['biaoqian'] = position['biaoqian'] + formatstr(re.html.xpath(base_xpath +"/div[2]/div[1]/span["+str(j)+"]/text()"))
                    j = j+1
                lg.append(position)
                i = i+1

        return lg


def formatstr(s):
    rs = str(s)
    return rs.replace("['", '').replace("']", '')


def save_data(data):
        wb = Workbook()
        ws = wb.active
        ws.append(['company', 'position', 'salary', 'location',
                   'posttime', 'slogen', 'biaoqian'])
        for da in data:
            ws.append([da['company'],da['position'],da['salary'],da['location'],da['posttime'],da['slogen'],da['biaoqian']])
        wb.save('lagou.xlsx')


d = crawl()
save_data(d)
