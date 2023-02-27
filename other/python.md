## Python编程基础

### 一、Python语言基本语法

Python是一个结合了解释性、编译性、互动性和面向对象的高级程序设计语言，结构简单，语法定义清晰。

Python最具特色的就是使用缩进来表示代码块，不需要使用大括号{}。

缩进的空格数是可变的，但是同一个代码块的语句必须包含相同的缩进空格数。



### 环境安装

python官网（[Python Release Python 3.7.9 | Python.org](https://www.python.org/downloads/release/python-379/)）

>1.下载对应版本程序包无脑安装
>
>2.验证安装：
>打开命令行工具，键入命令：`Python -V`，查看当前系统环境的 Python 版本如果为你所安装的 Python 版本的话，说明安装成功



#### 1、基础数据类型

Python3中有六个标准的数据类型：Number（数字）、String（字符串）、List（列表）、Tuple（元组）、Set（集合）、Dictionary（字典）。其中，不可变数据类型有：Number、String、Tuple；可变数据类型有：List、Dictionary、Set。

Python3支持的数字类型有int（整数）、float（浮点数）、bool（布尔型）、complex（复数）四种类型。

#### 2、变量和赋值

- Python 中的变量是不需要声明数据类型的，变量的“类型”是所指的内存中被赋值对象的类型。
- 同一变量可以反复赋值，而且可以是不同类型的变量，这也是Python语言称之为动态语言的原因。
- Python允许同时为多个变量赋值。

#### 3、操作符和表达式

- 运算符用于执行程序代码运算，会针对一个以上操作数项目来进行运算。
- Python语言支持算术运算符、关系运算符和逻辑运算符。
- 表达式是由操作对象和操作符组成的有意义的式子。

#### 4、字符串

- 字符串被定义为引号之间的字符集合，在Python中，字符串用单引号(’), 双引号("), 三引号(’’’)括起来，且必须配对使用。
- 当Python字符串中有一个反斜杠时表示一个转义序列的开始，称反斜杠为转义符。



#### 字符串的运算

字符串子串可以用分离操作符（[]或者[:]）选取，Python特有的索引规则为：第一个字符的索引是0，后续字符索引依次递增，或者从右向左编号，最后一个字符的索引号为-1，前面的字符依次减1。



#### 5、流程控制

又称为选择结构，根据判断条件，程序选择执行特定的代码。　
Python语言中使用关键字if、elif、else来表示，基本语法格式如下：

```python
if condition：
	if-block
[elif condition:	
	elif-block
else:	
	else-block]
```

其中，冒号(:)是语句块开始标记，[ ]内为可选项。另，在python中，当condition的值为False、0、None、””、（）、[]、{}时，会被解释器解释为假(False)。

#### 循环语句

循环结构是指满足一定的条件下，重复执行特定代码块的一种编码结构。Python中，常见的循环结构是for循环和while循环。

#### (1）while 循环:

while语句语法格式：

```python
#输入：
i = 0
while i < 20:
    if i % 3 == 0:
        print(i,end= " ")
    i = i + 1
    
#输出：
0 3 6 9 12 15 18 
```

#### (2）for循环:

for循环的语句格式：

```python
#输入：
sum = 0
for i in range(1,6):
    sum = sum + i

print("1+2+3+4+5 = %d"%sum)

#输出：
1+2+3+4+5 = 15
```



#### 请求与响应

>## Requests常用参数
>
>method： 请求方式 get，或者 post，put，delete 等
> url 请求的: url 地址 接口文档标注的接口请求地址
> params：请求数据中的链接，常见的一个 get 请求，请求参数都是在 url 地址中
> data ：请求数据，参数 表单的数据格式
> json： 接口常见的数据请求格式
> headers：请求头信息，http 请求中，比如说编码方式等内容添加
> cookie：保存的用户登录信息，比如做一些充值功能，但是需要用户已经登录，需要 cookie 信息的请求信息传输
> file：接口中上传文件
> timeout ：超时处理 proxys 设置代理
> stream ：文件下载功能，通过请求方式，下载文件
>
>## equests响应内容
>
>r.encoding                       #获取当前的编码
> r.encoding = 'utf-8'             #设置编码
> r.text                           #以encoding解析返回内容。字符串方式的响应体，会自动根据响应头部的字符编码进行解码。
> r.cookies                                  #返回cookie
> r.headers                        #以字典对象存储服务器响应头，但是这个字典比较特殊，字典键不区分大小写，若键不存在则返回None
> r.status_code                     #响应状态码
> r.json()                         #Requests中内置的JSON解码器，以json形式返回,前提返回的内容确保是json格式的，不然解析出错会抛异常
> r.content                        #以字节形式（二进制）返回。字节方式的响应体，会自动为你解码 gzip 和 deflate 压缩。
>
>
>
>
>





### 代码练习

>安装类库
>
>```shell
>#在命令行中使用pip安装requests库
>pip install requests
>#卸载lxml库
>pip uninstall lxml
>#安装lxml库
>pip install -i https://pypi.tuna.tsinghua.edu.cn/simple lxml==4.5
>```
>
>
>
>



#### 发起HTTP/HTTPS请求

>#### 方法一：[urllib](https://so.csdn.net/so/search?q=urllib&spm=1001.2101.3001.7020)
>
>urllib是python内置的HTTP请求库，无需安装即可使用，它包含了4个模块：
>
>request：它是最基本的http请求模块，用来模拟发送请求
>
>error：异常处理模块，如果出现错误可以捕获这些异常
>
>parse：一个工具模块，提供了许多URL处理方法，如：拆分、解析、合并等
>
>robotparser：主要用来识别网站的robots.txt文件，然后判断哪些网站可以爬
>
>```python
>import urllib.request as ur
>response=ur.urlopen("https://www.baidu.com")
>html=response.read().decode("utf-8")
>print(html)
>```
>
>timeout参数：用于设置超时时间，单位为秒，如果请求超出了设置时间还未得到响应则抛出异常，支持HTTP,HTTPS,FTP请求
>
>```python
>import urllib.request
>#设置超时时间为0.1秒,将抛出异常
>response=urllib.request.urlopen('http://www.baidu.com',timeout=0.1)  
>print(response.read())
>```
>
>可以使用异常处理来捕获异常
>
>```python
>import urllib.request
>import urllib.error
>import socket
>try:
>    response=urllib.request.urlopen('http://httpbin.org/get',timeout=0.1)
>    print(response.read())
>except urllib.error.URLError as e:
>    if isinstance(e.reason,socket.timeout): #判断对象是否为类的实例
>        print(e.reason) #返回错误信息
>```
>
>
>
>k
>
>k
>
>k
>
>>
>
>l
>
>l





>#### 方法二：requests
>
>requests是python实现的简单易用的HTTP库，使用起来比urllib简洁很多。因为是第三方库，所以使用前需要cmd安装：pip install requests
>
>安装完成后import一下，正常则说明可以开始使用了。
>
>```python
>import requests
>url="http://httpbin.org/get"
>data={"keywords":"itcast"}
>headers={"User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/78.0.3904.108 Safari/537.36"}
>response = requests.get(url,headers=headers,params=data)
>print(response.text)
>```
>
>请求的结果如下：
>
>```python
>{
>  "args": {
>    "keywords": "itcast"
>  }, 
>  "headers": {
>    "Accept": "*/*", 
>    "Accept-Encoding": "gzip, deflate", 
>    "Host": "httpbin.org", 
>    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/78.0.3904.108 Safari/537.36"
>  }, 
>  "origin": "118.123.252.136, 118.123.252.136", 
>  "url": "https://httpbin.org/get?keywords=itcast"
>}
>```
>
>还可以将参数放在url中发送get请求
>
>```python
>import requests
>response = requests.get(http://httpbin.org/get?name=gemey&age=22)
>print(response.text)
>```
>
>除了提出get请求，还可以发出以下几个请求：
>
>```python
>requests.get('http://httpbin.org/get')
>requests.post('http://httpbin.org/post')
>requests.put('http://httpbin.org/put')
>requests.delete('http://httpbin.org/delete')
>requests.head('http://httpbin.org/get')
>requests.options('http://httpbin.org/get')
>```
>
>#### 带参数的请求
>
>1.URL参数
>
>```py
>requests.get(url,params={'key1':'value1'},proxies=proxy)
>```
>
>
>2.表单参数提交
>
>Content-Type:application.x-www-form-urlencoded (表单默认的提交数据格式)
>
>内容：key1=value1&key2=value2
>
>```py
>requests.post(url,data={'key1':'value1','key2':'value2'},proxies=proxy)
>```
>
>
>3.json参数提交
>
>Content-Type: appliction/json (json数据格式)
>
>内容：’{“key”:“value1”,“key2”:“value2”}’
>
>```py
>requests.post(url,json={'key1':'value1','key2':'value2'},proxies=proxy)
>```
>
>#### Response对象
>
>```py
>import requests
>
>response = requests.get('http://www.baidu.com')
>print(response.status_code)  # 打印状态码
>print(response.url)          # 打印请求url
>print(response.headers)      # 打印头信息
>print(response.cookies)      # 打印cookie信息
>print(response.text)  #以文本形式打印网页源码
>print(response.content) #以字节流形式打印
>```
>
>#### 异常捕获
>
>在你不确定会发生什么错误时，尽量使用try…except来捕获异常。所有的requests.exception：
>
>```py
>import requests
>from requests.exceptions import ReadTimeout,HTTPError,RequestException
>
>try:
>    response = requests.get('http://www.baidu.com',timeout=0.5)
>    print(response.status_code)
>except ReadTimeout:
>    print('timeout')
>except HTTPError:
>    print('httperror')
>except RequestException:
>    print('reqerror')
>```
>
>requests库将常见的http错误打了一个包，就是exceptions，通过使用exceptions可以处理项目中各种异常
>
>```apl
>RequestException:
>HTTPError(RequestException) 　
>UnrewindableBodyError(RequestException) 　
>RetryError(RequestException) 　
>ConnectionError(RequestException) ProxyError(ConnectionError)
>SSLError(ConnectionError)
>ConnectTimeout(ConnectionError, Timeout)
>Timeout(RequestException) ReadTimeout
>URLRequired(RequestException) 　
>TooManyRedirects(RequestException) 　
>MissingSchema(RequestException, ValueError) 　
>InvalidSchema(RequestException,ValueError) 　
>InvalidURL(RequestException,ValueError) 　
>InvalidHeader(RequestException,ValueError) 　
>ChunkedEncodingError(RequestException) 　
>StreamConsumedError(RequestException,TypeError) 　
>ContentDecodingError(RequestException,BaseHTTPError)
>```



#### 综合案例——爬取贴吧信息

```py
import requests
from lxml import etree as et
import json

class WebSpider(object):
    def __init__(self):
        self.keyword=input("输入要爬取的贴吧名：")
        self.start_page=int(input("输入起始页码："))
        self.end_page=int(input("输入结束页码："))
        self.base_url="http://tieba.baidu.com/"
        
    def load_page(self):
        user_agent="Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/78.0.3904.108 Safari/537.36"
        header={"User-Agent":user_agent}
        for page in range(self.start_page,self.end_page+1):
            #newkeyword=bytes(self.keyword).encode('utf-8')
            new_url=self.base_url+"f?kw="+self.keyword+"&ie=utf-8&pn="+str((page-1)*50)
            print(new_url)
            response = requests.get(new_url,headers=header)
            self.parse_page(response.text)
            
    def parse_page(self,html):
        html_new=html.replace(r'<!--','"').replace(r'-->','"')
        root=et.HTML(html_new)
        replynum=root.xpath("//ul[@id='thread_list']/li/div/div[1]//span/text()")
        title=root.xpath("//ul[@id='thread_list']/li/div//a[@class='j_th_tit ']/text()")
        #//ul[@id='thread_list']/li/div/div[2]/div[2]/div[1]/div/text()
        url=root.xpath("//ul[@id='thread_list']/li/div//a[@class='j_th_tit ']/@href")
        author=root.xpath("//ul[@id='thread_list']/li/div/div[2]/div[1]/div[2]/span[1]/@title")
        self.append_page(replynum,title,url,author)
        
    def append_page(self,replynum,title,url,author):
        minsize=min(len(replynum),len(title),len(url),len(author))
        items=[]
        for i in range(minsize):
            item={}
            item["回复人数"]=replynum[i]
            item["帖子题目"]=title[i]
            item["帖子链接"]=self.base_url+url[i]
            item["作者"]=author[i]
            items.append(item)
        self.save_json(items)
    
    def save_json(self,items):
        try:
            for i in range(len(items)):
                strr = json.dumps(items[i],ensure_ascii=False)
                ff = open(self.keyword+".json","a",encoding='utf-8')
                ff.write(strr)
                ff.write("\n\n")
                ff.close()
            print("保存完毕")
        except Exception as ex:
            print("保存失败")
            print(ex)
            
if __name__=="__main__":
    spider = WebSpider()
    spider.load_page()
```









