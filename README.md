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
电脑浏览器访问：http://127.0.0.1:whistle启动的端口号，开始调试  
<h2>whsitle多代理</h2>
whsitle浏览器打开之后默认有一个default代理，可以自行选择创建多个代理，default优先级最低，其他自上而下优先级一次递减。  
![截图](/images/f.jpg) 
## 入门级使用规则  
http:www.baidu.com/index?a=1 127.0.0.1:8080 //请求转发到本地  
http:www.baidu.com/js/a.js file://E:\js\a.js //替换静态资源  
http:www.baidu.com/index?a=1 www.aliexpress.com/index?a=1 //替换请求  
http:www.baidu.com/index.html weinre://test //调试页面dom  
http://www.baidu.com/ log://  //查看页面控制台信息  
http://www.baidu.com js://{vConsole.js} // 注入js文件，会自动在html中插入script标签  
## 命令
### 启动
w2 start，启用默认端口8899  
w2 start -p xxxx，指定端口  
### 重启  
w2 restart  
### 关闭  
w2 stop  
