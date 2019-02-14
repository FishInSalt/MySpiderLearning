# 爬取66ip代理网站的代理地址
headers  
```python
base_headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/54.0.2840.71 Safari/537.36',
    'Accept-Encoding': 'gzip, deflate, sdch',
    'Accept-Language': 'en-US,en;q=0.9,zh-CN;q=0.8,zh;q=0.7'
}
```  
抓取网页的函数 get_page(url)
```python
def get_page(url,options = {}):
    headers = dict(base_headers,**options)
    print('正在抓取', url)
    try:
        response = requests.get(url,headers = headers)
        print('抓取成功',url, response.status_code)
        if response.status_code ==200:
            return response.text
    except ConnectionError:
        print("抓取失败", url)
        return None
```  
抓取后解析
```python
start_url = 'http://www.66ip.cn/{}.html'
urls = [start_url.format(page) for page in range(5)]
for url in urls:
    html = get_page(url)
#print(html.encode('latin1').decode('gbk'))
    if html:
        doc = pq(html)
        trs = doc('.containerbox table tr:gt(0)').items()
        for tr in trs:
            ip = tr.find('td:nth-child(1)').text()
            port = tr.find('td:nth-child(2)').text()
            print(':'.join([ip,port]))
```  
其中，网页的html是gb2312编码方式，直接看会有乱码，用html.encode('latin1').decode('gbk')即可解决。（虽然没什么关系）
结果：  
![image](https://github.com/FishInSalt/MySpiderLearning/blob/master/proxy%20pool/crawl_proxy/24.png)
