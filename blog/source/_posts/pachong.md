##爬虫的知识点


```
1.请求：requests模块，urllib模块
res = requests.get(url,headers,params,proxies)
	url:请求的网址
     headers:请求头
     params:传递参数，没有就不传
     proxies:代理id，proxies = {'http':'http://id:port','https':'http://ip;port'}
res = requests.post(url,headers,data,proxies)

#res的相应方法与属性
res.text:获取文本形式的HTML页面源码
res.content:获取响应的二进制形式，图片，压缩包，软件包的爬取
res.json():适用于ajax请求返回的json串
    
#res其他属性：
res.status_code:状态码
res.headers:获取头信息
res.history:获取请求历史
res.cookie:cookie信息
```



```
2.数据解析，re正则，xpath，bs4
正则：
	元字符：.(任意字符，换行符除外) *（匹配0个或多个） ?(少匹配) \d(数字) \w(数字，字母，下划线)
    贪婪匹配与非贪婪匹配：直接在量词后加一个?
    分组:用括号将要匹配出来的括起来
re模块：
re.findall():匹配出所有满足条件的结果，返回一个列表
re.match():从头进行匹配，如果开头不满足，匹配出来就是None，如果成功，返回一个对象，通过group取值
re.search():匹配到第一个就返回对象，通过group取值，没有匹配到返回None
re.compile():公式正则表达式编译为一个对象，存储在内存中，提高了正则的匹配效率
    
    
s = '<a href="https://www.baidu.com">跳转链接</a>'
ret = re.findall(r'href="(.*?)"',s)


#xpath解析数据：
from lxml import etree
tree = etree.HTML(res.text)
tree.xpath('xpath表达式')
语法：基础语法，单属性多值匹配，多属性匹配，按序匹配

nodeName:节点名定位

./:从当前节点的根节点向下匹配

.//:从当前的任意位置开始向下匹配

nodename[@atteribute="value"]:根据节点的属性进行定位

@atteribute：获取节点的属性值

text():获取节点的文本

#单属性多值匹配(contains)和多属性匹配(and)

nodename[containins((@atteribute),'value')]

nodename[@atteributename="value" and @attribuenname="value"]


#按序选择：
1).索引定位：索引从1开始
2).last()函数：last代表最后一个，倒数第二个用last()-1
3).postition()函数：代表位置
```



```
#数据持久化：
文件系统：txt,json,csv
	1).txt存储
    with open('文件路径'，'写入方式w',encoding='utf-8') as f:
        f.write('字符串')
    2).json存储
    with open('文件路径/肯德基.json','w',encoding='utf-8') as f:
        f.write(json.dumps(lst,indent=4,ensuer_ascii=Flase))
    3).存csv文件：
    import csv
    with open('liupi.csv','w',encoding='utf-8') as csvf:
        write = csv.write(csvf,delimiter=',')
        write.writerow(['field1','field2','field3'])
        
    writh open('liupi.csv','a',encoding='utf-8') as csvf:
        write = csv.writer(csvf,delimiter=',')
        write = writerow([字段1，字段2，字段3])
数据库：MySQL，MongoDB，Redis

```

