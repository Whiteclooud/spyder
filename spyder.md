# 爬虫学习  
## 爬虫简介  
**什么是爬虫:**   
通过编写程序，模拟浏览器上网，然后让其去互联网上抓取数据的过程。  

***爬虫在使用场景中的分类**  
1. 通用爬虫：  
        抓取系统终于组成部分。抓取的是一整张页面数据。  
2. 聚焦爬虫：  
        是建立在通用爬虫的基础之上。抓取的是页面中特定的局部内容。  
3. 增量式爬虫:  
        检测网站中数据更新的情况。只会抓取网站中最新更新出来的内容。  

**robots.txt**协议：(打开方式：域名后+/robots.txt)  
        君子协议。规定了网站中哪些数据可以被爬虫爬取，哪些数据不可以被爬取。  

**http协议**  
- 概念：就是服务器和客户端进行数据交互的一种形式。  
常用请求头信息  
- User-Agent:请求载体的身份标识。  
- Connection:请求完毕后，是断开连接还是保持连接。  
常用响应头信息  
- ConTent-Type:服务器响应回客户端的数据类型。  
**https协议**  
- 安全的超文本传输协议。  
**加密方式**  
1. 对称密钥加密  
2. 非对称密钥加密  
3. 证书密钥加密  
## requests模块基础  
### requests模块（01）    
1. urllib模块  
2. requests模块  
requests模块：python中原生的一款基于网络请求的模块，功能非常强大，简单便捷，效率极高。  
作用：模拟浏览器发请求。  
**如何使用： (requests模块的编码流程)**  
（1）指定url(requests模块中url编码可用中文。)  
    若url是带参数的，需要将url的参数传到字典里，再赋值给get（）中的params参数。
（2）发起请求   
（3）获取响应数据  
（4）持久化存储  

### UA伪装（02）  
UA:User-Agent(请求载体的身份标识)  
身份标识：  
1. 浏览器端  
2. 爬虫  
**UA检测**：门户网站的服务器会检测对应请求的载体身份标识，如果检测到请求的载体身份标识为某一款浏览器，说明该请求是一种正常的请求。但是，如果检测到请求的载体身份标识不是基于某一款浏览器的，则表示该过程为不正常请求（爬虫）,则服务端就很肯拒绝该次请求。  
**UA伪装**：让爬虫对应的请求载体身份标识伪装成某一款浏览器。   
为了使每次请求都成功，要进行UA伪装。  

```
import requests
#UA伪装：将对应的User-Agent封装到一个字典中
url='https://www.sogou.com/web?query=波晓张'
#处理url携带的参数:封装到字典中
headers={
    'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.4844.74 Safari/537.36 Edg/99.0.1150.46'
    }
kw=input('enter a word:')
param = {
    'query':kw
    }
#对指定url发起的请求是携带参数的，并且请求过程中处理了函数。
response = requests.get(url=url,params=param,headers=headers)
page_text = response.text
fileName = kw + '.html'
with open(fileName,'w',encoding='utf-8') as fp:
    fp.write(page_text)
print(fileName,'保存成功！')
```  
  
