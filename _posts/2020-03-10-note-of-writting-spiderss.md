---
layout:     post
title:      note of writting spider
categories: python
subtitle:   about random headers
date:       2020-03-10
author:     yao-wen-chao
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - spider
    - random headers
---

# 随机请求头的使用

在爬取网站内容时，如何同一个ip地址使用同一个请求头向服务器发出大量请求，可能会被服务器识别为恶意爬虫而封ip（个人理解，不一定十分准确）。为了避免这种现象发生，可以采用以下方法：1.随机使用请求头；2.使用ip代理池。当然如果两种方法同时使用，效果更佳。

废话不说，直接上代码。

```
import random

def get_headers():
    USER_AGENTS = [
    'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:61.0) Gecko/20100101 Firefox/61.0',
    'Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10_6_8; en-us) AppleWebKit/534.50 (KHTML, like Gecko) Version/5.1 Safari/534.50',
    'Mozilla/5.0 (Windows; U; Windows NT 6.1; en-us) AppleWebKit/534.50 (KHTML, like Gecko) Version/5.1 Safari/534.50',
    'Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0',
    'Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.0; Trident/4.0',
    'Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.0',
    'Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1',
    'Mozilla/5.0 (Macintosh; Intel Mac OS X 10.6; rv:2.0.1) Gecko/20100101 Firefox/4.0.1',
    'Mozilla/5.0 (Windows NT 6.1; rv:2.0.1) Gecko/20100101 Firefox/4.0.1',
    'Opera/9.80 (Macintosh; Intel Mac OS X 10.6.8; U; en) Presto/2.8.131 Version/11.11',
    'Opera/9.80 (Windows NT 6.1; U; en) Presto/2.8.131 Version/11.11',
    'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_7_0) AppleWebKit/535.11 (KHTML, like Gecko) Chrome/17.0.963.56 Safari/535.11',
    'Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Maxthon 2.0',
    'Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; TencentTraveler 4.0',
    'Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1',
    'Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; The World',
    'Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Trident/4.0; SE 2.X MetaSr 1.0; SE 2.X MetaSr 1.0; .NET CLR 2.0.50727; SE 2.X MetaSr 1.0',
    'Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; 360SE',
    'Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Avant Browser',
    'Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1',
    ]
    User_Agent = random.choice(USER_AGENTS)
    headers = {
    'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
    'Accept-Language': 'en',
    'User-Agent': User_Agent
    }
    return headers
```

仅作为个人学习记录之用，写的相当随意。

---

