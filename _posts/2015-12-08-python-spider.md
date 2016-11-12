---
layout: post
title: Python编写简单爬虫
date: 2015-12-08
category: "Linux"
---


Python 小试牛刀

## 利用chrome查看登录相关信息

按F12打开chrome的开发者工具，勾选Preserve log选项。找到核心post请求的*Request URL*和提交的*Form Data*。

（之前一直查看源代码ctrl+f检索input标签找name属性的方法太稚嫩了。。

![chrome-network](images/2015/chrome-network.png "chrome-network")<br>

## 编辑python代码思路

{% highlight javascript %}
urllib2.build_opener(urllib2.HTTPCookieProcessor(cookieJar))  # 自动管理请求得到的cookie
    
urllib2.Request(url, data)  # 封装请求
    
opener.open(req)  # 发送请求
{% endhighlight %}
然后带着登录后服务器给你的cookie再向另一个包含我想要的信息的resultUrl发送请求。

得到相应页面信息后利用BeautifulSoup这个第三方库对html进行解析并拿到相应的数据。

实现代码如下：

{% highlight python %}

# -*- coding: utf-8 -*-
import urllib
import urllib2
import cookielib
from bs4 import BeautifulSoup

__author__ = 'Yan'


class eSprider:
    # 申明相关的属性
    def __init__(self):
        self.loginUrl = 'http://e.ustb.edu.cn/userPasswordValidate.portal'
        self.resultUrl = 'http://e.ustb.edu.cn/index.portal?.pn=p378_p381'
        self.cookieJar = cookielib.CookieJar()
        self.postdata = urllib.urlencode({
            'Login.Token1': 's2015XXXX', 'Login.Token2': 'XXXXXX', 
            'goto': 'http://e.ustb.edu.cn/loginSuccess.portal',
            'gotoOnFail': 'http://e.ustb.edu.cn/loginFailure.portal'
        })
        self.times = []
        self.locations = []
        self.cost = []
        self.balance = []
        self.opener = urllib2.build_opener(urllib2.HTTPCookieProcessor(self.cookieJar))

    def main(self):
        # 初始化链接并且获取cookie
        myRequest = urllib2.Request(self.loginUrl, self.postdata)
        self.opener.open(myRequest)
        result = self.opener.open(self.resultUrl)
        # 拿到相应页面
        # print result.read()
        self.deal_data(result.read())
        self.printall(self.times, self.locations, self.cost, self.balance)

    def deal_data(self, myPage):
        # print myPage
        soup = BeautifulSoup(myPage, 'html.parser', from_encoding="utf-8")
        body = soup.body
        tds = body('td', class_={"table_text", "table_text2"})
        for index, td in enumerate(tds):
            # if td.string.startswith('20'):
            #     self.times.append(td.string)
            if index % 4 == 0:
                self.times.append(td.string)
            if index % 4 == 1:
                self.locations.append(td.string)  # 不知道Unicode怎么转 print出来就好了 或者本身存进去就是Unicode
            if index % 4 == 2:
                self.cost.append(td.string)
            if index % 4 == 3:
                self.balance.append(td.string)

    def printall(self, item1s, item2s, item3s, item4s):
        for index in range(len(item1s)):
            print item1s[index], item2s[index], item3s[index], item4s[index]

if __name__ == '__main__':
    myspider = eSprider()
    myspider.main()

{% endhighlight %}

这个是resultUrl的部分我想要的信息：

![web-info](images/2015/web-info.png "页面端查看信息")<br>

利用printall函数打印出来的效果为：

![console-result](images/2015/console-result.png "控制台输出信息")<br>

大功告成！数据拿到了，实际应用时通过json传给前端也好还是存成txt就无所谓啦。