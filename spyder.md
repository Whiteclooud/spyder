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
## requests模块（01）    
1. urllib模块  
2. requests模块  
requests模块：python中原生的一款基于网络请求的模块，功能非常强大，简单便捷，效率极高。  
作用：模拟浏览器发请求。  
**如何使用： (requests模块的编码流程)**  
(1) 指定url(requests模块中url编码可用中文。)  
        若url是带参数的，需要将url的参数传到字典里，再赋值给get（）中的params参数。  
(2) UA伪装    
(3) 发起请求   
(4) 获取响应数据  
(5) 持久化存储  

### get请求  
参数在url上  
若url是带参数的，需要将url的参数传到字典里，再赋值给get（）中的params参数。  
UA:User-Agent(请求载体的身份标识)  
身份标识：  
1. 浏览器端  
2. 爬虫  
**UA检测**：门户网站的服务器会检测对应请求的载体身份标识，如果检测到请求的载体身份标识为某一款浏览器，说明该请求是一种正常的请求。但是，如果检测到请求的载体身份标识不是基于某一款浏览器的，则表示该过程为不正常请求（爬虫）,则服务端就很肯拒绝该次请求。  
**UA伪装**：让爬虫对应的请求载体身份标识伪装成某一款浏览器。   
为了使每次请求都成功，要进行UA伪装。  

### Post请求(ajax动态请求)   
参数在post请求中(放在字典中传给data参数)  
**Ajax请求:**使用Ajax技术网页应用能够快速地将增量更新呈现在用户界面上，而不需要重载（刷新）整个页面，这使得程序能够更快地回应用户的操作。(页面局部刷新)  
实战之破解百度翻译  
**若返回json,则输出时用xx.json()**  
#### 将字典写入json文件  
```json.dump(obj,fp,ensure_ascii=False)```  
obj:传入的字典 。 
fp:写入的文件。  
ensure_ascii:中文不可用ascii码，需要赋值为False。  

## 数据解析(聚焦爬虫)  
1. 正则  
2. bs4  
3. xpath(重点)  
### 正则表达式  
正则表达式:一种使用表达式的方式对字符串进行匹配的语法规则。   
优点:速度快，效率高，准确性高。  
若想提取一部分内容，就拿括号框起来。  
```
#正则在线测试网站:  
https://tool.oschina.net/regex/  
```  
#### 元字符  
元字符:具有固定含义的特殊符号。  
```
#常用元字符
1  .   匹配除换行以外的任意字符(未来在python的re模块中是一个坑)  
2  \w  匹配字母或数字或下划线  
3  \s  匹配任意的空白符  
4  \d  匹配数字  
5  \n  匹配一个换行符  
6  \t  匹配一个制表符  

7  ^   匹配字符串的开始  
8  $   匹配字符串的结尾  

9  \W  匹配非字母或数字或下划线  
10 \D  匹配非数字  
11 \S  匹配非空白符  
12 a|b 匹配字符a或字符b  
13 ()  匹配括号内的表达式,也表示一个组  
14 [...] 匹配字符组中的字符  
15 [^...] 匹配除了字符组中字符的所有字符  
```  
量词:控制前面的元字符出现的次数  
```
1  *   重复0次或更多次  
2  +   重复1次或更多次  
3  ?   重复0次或1次  
4 {n}  重复n次  
5 {n,} 重复n次或更多次  
6 {n,m} 重复n到m次  
```  
贪婪匹配和惰性匹配  
```
1  .*   贪婪匹配，尽可能多的去匹配结果  
2  .*?  惰性匹配，尽可能少的去匹配结果  
```
###
聚焦爬虫：爬取页面中指定的页面内容。  
**编码流程** 
- 指定url  
- 发起请求  
- 获取响应数据  
- 数据解析  
- 持久化存储  
数据解析原理概述:  
- 解析的局部的文本内容都会在标签之间或者标签对应的属性中进行存储。  
- 1.进行指定标签的定位。
- 2.标签或者标签对应的属性中存储的数据值进行提取（解析）  

#### re模块  
```
#findall  
re.findall(r"正则表达式","查询对象") #返回全部匹配到的内容加r可以忽视"\"的转义作用。  
#iter  
result = re.iter(r"正则表达式","查询对象") #返回迭代器  
for item in result:  #从迭代器中拿到内容   
        print(item.gtoup()) #从匹配的结果中拿数据。  
#search   
re.search(r"正则表达式","查询对象") #只会匹配第一次匹配的内容。  
#match  
re.match(r"正则表达式","查询对象") #从字符串的开头进行匹配，类似在正则前面加上了^  
```  
##### 预加载,提前把正则对象加载完毕  
```
obj = re.compile(r"正则表达式")  
#直接把加载好的正则进行使用  
result = obj.findall("查询对象")  
```
想要提取数据必须用小括号括起来,可以单独取名字。  
(?P<名字>正则)  
提取数据时需要.group("名字")
```
#将查找到的多个部分对应上不同的名字  
obj = re.compile(r"<span id='(?P<id>正则表达式)'>(?P<name>正则表达式</span>)")  
result = obj.finditer(查询对象)
for item in result: #分别提取不同的部分
        id = item.item.group("id")
        name = item.group("name")
```