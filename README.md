# whsitle
whsitle使用笔记
## 全局安装（局部安装貌似无效）  
cnpm install -g whistle
## 安装浏览器代理插件SwitchyOmega  
[SwitchyOmega下载地址](https://github.com/FelisCatus/SwitchyOmega/releases)  

![以下位置](/images/a.png) 
## 代理设置   
选择代理模式，输入本地ip以及whistle启动的端口号   

![](/images/b.png) 
配置与电脑局域网手机代理，ip为局域网下电脑ip，端口号为whistle启动的端口号   

<div align=center><img width="300" src="/images/c.jpg"/></div>

电脑浏览器访问：http://127.0.0.1:whistle启动的端口号  
开始调试  

## whsitle多代理  
whsitle浏览器打开之后默认有一个default代理，可以自行选择创建多个代理，default优先级最低，其他自上而下优先级一次递减。 

![截图](/images/f.png) 
## 设置抓取https请求
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
## 命令
### 启动
w2 start，启用默认端口8899  
w2 start -p xxxx，指定端口  
### 重启  
w2 restart  
### 关闭  
w2 stop  
