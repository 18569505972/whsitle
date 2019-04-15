# whsitle
whsitle使用笔记
## 全局安装（局部安装貌似无效）  
cnpm install -g whistle
## 安装浏览器代理插件SwitchyOmega  
[SwitchyOmega下载地址](https://github.com/FelisCatus/SwitchyOmega/releases)  

![以下位置](/images/a.png) 

## 基本命令
### 启动
w2 start，启用默认端口8899  
w2 start -p xxxx，指定端口  
### 重启  
w2 restart  
### 关闭  
w2 stop 
## 界面介绍

![截图](/images/i.png) 

其中Values内可以创建json、js、html等类型文件供rulers调用 

![截图](/images/h.png)
![截图](/images/j.png)

## 代理设置   
选择代理模式，输入本地ip以及whistle启动的端口号   

![](/images/b.png) 
配置与电脑局域网手机代理，ip为局域网下电脑ip，端口号为whistle启动的端口号   

<div align=center><img width="300" src="/images/c.jpg"/></div>

电脑浏览器访问：http://127.0.0.1:whistle启动的端口号  
开始调试  

### whsitle多代理  
whsitle浏览器打开之后默认有一个default代理，可以自行选择创建多个代理，default优先级最低，其他自上而下优先级一次递减。 

![截图](/images/f.png) 
### 设置抓取https请求
使用手机扫码安装即可添加信任凭据，拦截https请求  

![截图](/images/g.png)  

## 入门级使用规则  
### 请求链接匹配规则
#### 请求转发到本地
``http:www.baidu.com/index?a=1    127.0.0.1:8080  ``
#### 替换静态资源
``http:www.baidu.com/js/a.js    file://E:\js\a.js ``  
### 替换请求
``http://www.baidu.com/index?a=1    http://www.aliexpress.com/index?a=1``
#### 调试页面dom
``http://www.baidu.com/index.html    weinre://test``
#### 查看页面控制台打印信息 
``http://www.baidu.com/    log://``
#### 注入js
若为抓取链接为html请求，则自动在html尾部添加script标签，注入js  
``http://www.baidu.com    js://E:\js\a.js``
#### 注入css
若为抓取链接为html请求，则自动在html尾部添加style标签，注入样式表    
``http://www.baidu.com    css://E:\css\a.css``
#### 注入html
``http://www.baidu.com   html://E:\html\a.html``
## 匹配
### 匹配www.baidu.com下所有请求，包括如https、http、ws等不同协议
``www.baidu.com    127.0.0.1:8080``
### 匹配www.baidu.com下所有https请求
``https://www.baidu.com    127.0.0.1:8080``
### 匹配www.baidu.com下所有端口号为8080的请求
``www.baidu.com:8080    127.0.0.1:8080``
### 匹配www.baidu.com某路径下的所有请求
``https://www.baidu.com/index    127.0.0.1:8080``
### 正则匹配（不支持/reg/g，全局，支持/reg/i不区分大小写）
匹配url中含有baidu的请求  
``/baidu/i    127.0.0.1:8080``
### 精确匹配（匹配协议为https,域名为www.baidu.com,路径为/index的请求）
``$https://www.baidu.com/index    127.0.0.1:8080``
### 通配符匹配（若需要高级匹配，正则完全可以满足需要，这种方式可忽略）
^:以某字符串开始  
$:以某字符串结束   
\*:通配符（\*:$0；\*\*:$0$1 .......） 
## 进阶
### proxy、https-proxy（https代理）
将所有请求代理到本地  
```
  www.baidu.com   proxy://127.0.0.1:8888
  https-proxy://127.0.0.1:8888
```
### enable
params(开启参数):https(开启拦截https)、hide(隐藏匹配请求)  
``www.baidu.com    enable://params``
### disable
params(禁用参数):cache(请求缓存)、cookie(请求或响应cookie)、ua(删除ua)、referer(删除referer)、csp(删除scp策略)、timeout(禁用whistle内部请求超时时间)、301(将301转为302)、intercept(禁用https拦截)、dnsCache(禁用dns缓存)  
``www.baidu.com    disable://params``
### urlParams（修改请求参数）
params类别:1、(test=1) 2、Values菜单栏内的json文件  
``www.baidu.com    urlParams://params``
### method（修改请求方法）
www.baidu.com下的请求方法修改为post   
``www.baidu.com    method://post``
### statusCode、replaceCode
两者都可以修改请求状态码,前者请求不会发出去；后者响应后修改   
www.baidu.com下的请求状态码修改为404       
``www.baidu.com    statusCode://404``
### referer
修改www.baidu.com下的请求源页面地址为www.taobao.com      
``www.baidu.com    referer://www.taobao.com`` 
### ua
修改www.baidu.com下的请求用户代理为为Mozilla/5.0      
``www.baidu.com    ua://Mozilla/5.0`` 
### redirect
设置302重定向地址  
``www.baidu.com    redirect:www.taobao.com``
### reqMerge、resMerge
修改get请求参数为a=1&b=2  
``www.baidu.com    reqMerge:(a=1&b=2)``  
修改响应JSON数据  
``www.baidu.com    resMerge:(a=1&b=2)``  
浏览器接收解析为：{a:1,b:2}
### reqScript、resScript
通过脚本批量设置请求规则  
```
if (/index\.html/i.test(url)) {
    rules.push('/./ redirect://http://www.ifeng.com/?test.js');
}

if (/html/.test(headers.accept)) {
    rules.push('/./ resType://text');
}
// 如果请求内容里面有prefix字段，则作为新url的前缀
if (/(?:^|&)prefix=([^&]+)/.test(body)) {
    var prefix = RegExp.$1;
    var index = url.indexOf('://') + 3;
    var schema = url.substring(0, index);
    var newUrl = schema + prefix + '.' + url.substring(index);
    rules.push(url + ' ' + newUrl);
    // rules.push('/./ ' + newUrl);
}
```
通过脚本批量设置响应（同上）  
### reqType、resType（content-type）
修改请求内容类型  
``www.baidu.com    reqType://text/html``  
修改响应内容类型   
``www.baidu.com    resType://text/html`` 
### reqCharset、resCharset（charset）
修改请求编码类型  
``www.baidu.com    reqCharset://utf-8``  
修改响应编码类型   
``www.baidu.com    resCharset://utf-8`` 
### reqCookies、resCookies（cookie）
修改请求cookie  
``www.baidu.com    reqCookies://{cookie}``  
修改响应cookie   
``www.baidu.com    resCharset://{cookie}``  
cookie.json  
```
user:admin
password:admin
```
### reqHeaders、resHeaders
批量修改请求头、响应头  
### reqPrepend、resPrepend
把指定内容加到请求或响应内容前面（没有内容无法追加）  
### reqBody、resBody
用指定内容替换请求或响应内容
### reqAppend、resAppend
把指定内容加到请求或响应内容后面
### reqReplace、resReplace
替换类型为application/x-www-form-urlencoded)、urlencoded、html、json、xml、text等的内容
### htmlPrepend、cssPrepend、jsPrepend
htmlPrepend:在content-type为html的响应内容前插入html  
cssPrepend：在content-type为html、css的响应内容前插入style样式  
jsPrepend：在content-type为html、js的响应内容前插入scripts脚本   
### htmlBody、cssBody、jsBody
htmlBody:替换content-type为html的响应内容  
cssBody：替换content-type为html、css的响应内容（html添加标签）  
jsBody：替换content-type为html、js的响应内容（html添加标签）  
### htmlAppend、cssAppend、jsAppend
htmlAppend:在content-type为html的响应内容后插入html  
cssAppend：在content-type为html、css的响应内容后插入style样式  
jsAppend：在content-type为html、js的响应内容后插入scripts脚本  
