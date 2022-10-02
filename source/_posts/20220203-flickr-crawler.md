---
title: '[python]利用flickrapi爬取Flickr的圖片'
date: 2022-02-03 17:04:58
categories: python
tags: [python, coding, code, flickrapi, web crawler, 網路爬蟲]
---
![](https://photos.smugmug.com/photos/i-MMLCBKw/0/f2255d5d/X2/i-MMLCBKw-X2.png)
Flickr是世界數一數二大的圖片資料庫，雖然經過幾次轉手，經歷付費政策的風波，其仍具相當的價值
flickrapi可以幫助我們快速收集圖片資料，探索Flickr
這邊使用的python環境為3.7
使用`pip install flickrapi`是安裝flickrapi的好的開始
<!--more-->
在使用flickrapi前記得去申請一組key跟secret方便後續使用

以下就是code的部分
{% codeblock python lang:python %}
import flickrapi
from urllib.request import urlretrieve
import os

# 這邊打自己的key跟secret，不要傻傻複製貼上，跑不動阿!!!
flickr = flickrapi.FlickrAPI(api_key=KEY,
                             secret=SECRET,
                             cache=True)

# 這邊打要搜尋的關鍵字
searchword = 'office ceiling'
# 這邊放搜尋張數上限
limitnum = 500

os.makedirs('./picture/%s' % searchword, exist_ok=True)

photos = flickr.walk(text=searchword,
                     extras='url_c',
                     per_page=200,
                     sort='relevance')
urls = list()
for i, photo in enumerate(photos):
    print(i)

    try:
        url = photo.get('url_c')
        urls.append(url)
        urlretrieve(urls[i], './picture/%s/%s-%s.jpg' % (searchword, searchword, str(i)))
    except Exception as e:
        print(e)

    if i>limitnum-1:
        break
{% endcodeblock %}

使用自行注意著作權問題喔~