---
title: HTTP协议简单入门
date: 2018-03-31 18:53:47
tags: [HTTP] 
---
# 什么是HTTP？
> HTTP协议（HyperText Transfer Protocol，超文本传输协议）是用于从WWW服务器传输超文本到本地浏览器的传送协议。
> 它的发展是万维网协会（World Wide Web Consortium）和Internet工作小组IETF（Internet Engineering Task Force）合作的结果。它可以使浏览器更加高效，使网络传输减少。
> 它不仅保证计算机正确快速地传输超文本文档，还确定传输文档中的哪一部分，以及哪部分内容首先显示(如文本先于图形)等。  

# HTTP版本
> 最早版本是1991年发布的HTTP0.9版，该版本只有一个命令`GET`，且只能回应HTML格式的字符串，无法回应别的格式。  
> 最常用的版本是HTTP1.0/1.1。
> 最新版本是HTTP2.0，与1.0/1.1相比，有了更高的性能、安全性和灵活性。
<!-- more -->
# HTTP请求与响应
## 服务器与浏览器的交互
![server_browser](http://p69er22kd.bkt.clouddn.com/server_browser.png)  
* 浏览器负责发起请求
* 服务器在80端口接收请求
* 服务器负责返回内容（响应）
* 浏览器负责下载响应内容  

## HTTP请求  
用curl创造一个请求，并获得响应。  
e.g.  
`curl -s -v -H "Frank: xxx" -- "https://www.baidu.com"`  
![HTTP_1](http://p69er22kd.bkt.clouddn.com/HTTP_1.png)  

`curl -X POST -s -v -H "Frank: xxx" -- "https://www.baidu.com"`  
![HTTP_2](http://p69er22kd.bkt.clouddn.com/HTTP_2.png)    

`curl -X POST -d "1234567890" -s -v -H "Frank: xxx" -- "https://www.baidu.com"`  
![HTTP_3](http://p69er22kd.bkt.clouddn.com/HTTP_3.png)  
  
请求的格式：  
1.请求方式 路径 协议/版本  
2.key: value  
2.key: value  
2.Content-Length: 10（例）  
2.Content-Type: application/x-www-form-urlencoded（例）  
3.（空行）  
4.要上传的数据

## HTTP响应
以上的HTTP请求对应的HTTP响应如下：  
![HTTP_4](http://p69er22kd.bkt.clouddn.com/HTTP_4.png)  

![HTTP_5](http://p69er22kd.bkt.clouddn.com/HTTP_5.png)  

![HTTP_6](http://p69er22kd.bkt.clouddn.com/HTTP_6.png)  

响应的格式：  
1.协议/版本 状态码 状态解释  
2.key: value  
2.key: value  
2.Content-Length: 17931（例）  
2.Content-Type: text/html（例）  
3.（空行）  
4.要下载的内容  

## 常见Content-Type
| Content-Type           | 文件扩展名         |
| ---------------------- | --------          |
| text/plain             | .sol/.txt/.sor    |
| text/html              | .html/.jsp/.xhtml |
| text/css               | .css              |
| image/jpeg             | .jpg/.jpeg        |
| image/png              | .png              |
| image/svg+xml          | .svg              |
| audio/mp3              | .mp3              |
| video/mp4              | .mp4/.m4e         |
| application/javascript | .js               |
| application/pdf        | .pdf              |
| application/zip        | .zip              |

## 常见HTTP状态码
1. 200 OK  
一切正常，对GET和POST请求的响应文档跟在后面  

2. 301 Moved Permanently  
客户请求的文档在其他地方，新的URL在Location头中给出，浏览器应该自动地访问新的URL。  

3. 302 Found  
类似于301，但新的URL应该被视为临时性的替代，而不是永久性的。  

4. 304 Not Modified  
客户端有缓冲的文档并发出了一个条件性的请求（一般是提供If-Modified-Since头表示客户只想比指定日期更新的文档）。服务器告诉客户，原来缓冲的文档还可以继续使用。  

5. 400 Bad Request  
请求出现语法错误。  

6. 401 Unauthorized  
客户试图未经授权访问受密码保护的页面。应答中会包含一个WWW-Authenticate头，浏览器据此显示用户名字/密码对话框，然后在填写合适的Authorization头后再次发出请求。  

7. 403 Forbidden  
资源不可用。  

8. 404 Not Found  
无法找到指定位置的资源。  

9. 410 Gone  
所请求的文档已经不再可用，而且服务器不知道应该重定向到哪一个地址，返回410表示文档永久地离开了指定的位置。  

10. 500 Internal Server Error  
服务器遇到了意料不到的情况，不能完成客户的请求。  

11. 501 Not Implemented  
服务器不支持实现请求所需要的功能。例如，客户发出了一个服务器不支持的PUT请求。 
  
  
# 如何使用curl命令？
常见参数  

| 参数                      | 释义                        |
| ------------------------- | -------------------------- |
| -s/--silent               | 静音模式，不显示进度与错误     |
| -v/--verbose              | 显示请求（否则只显示响应）     |
| -H/--header               | 添加一个Key/Value           |
| -- “`https://XXXX.XXX`”   | 需要请求的网址               |
| -X/--request              | 指定请求的命令，如`-X POST`   |
| -d/--data                 | POST方式传输数据             |

e.g.  

`curl -s -v -H "Frank: xxx" -- "https://www.baidu.com"`  

`curl -X POST -s -v -H "Frank: xxx" -- "https://www.baidu.com"`  

`curl -X POST -d "1234567890" -s -v -H "Frank: xxx" -- "https://www.baidu.com"`  

# 使用Chrome开发者工具查看HTTP请求/响应内容
Chrome浏览器不仅可以调试页面、JS、请求、资源、cookie，还可以模拟手机进行调试，是一个十分强大的网页浏览器。以下介绍如何使用Chrome浏览器下的开发者工具查看HTTP请求及响应内容。  

## 使用Chrome开发者工具查看HTTP请求
1. 打开Chrome浏览器，按下键盘上的 `F12` 键或 `Ctrl+Shift+I` 即可打开开发者工具栏，也可在浏览器右上角点击 `自定义及控制Google Chrome` 按钮，`更多` → `开发者工具` 里打开。  
![HTTP_7](http://p69er22kd.bkt.clouddn.com/HTTP_7.png)  

2. 在 `开发者工具` 中切换至 `Network` 选项，将调试对象切换至 `All`。  
![HTTP_8](http://p69er22kd.bkt.clouddn.com/HTTP_8.png)  

3. 在网址栏中输入 `www.baidu.com` 并确认，即可在 `开发者工具`中找到我们需要的HTTP请求内容。  
![HTTP_9](http://p69er22kd.bkt.clouddn.com/HTTP_9.png)  

4. 点击进入，在Headers中可看到 `Request Headers` 选项，进入该选项，点击 `View Source` ，即可看到本次访问 `www.baidu.com` 的HTTP请求。  
![HTTP_10](http://p69er22kd.bkt.clouddn.com/HTTP_10.png)  

## 使用Chrome开发者工具查看HTTP响应
1. 同上。
2. 同上。
3. 同上。
4. 点击进入，在Headers中可看到 `Response Headers` 选项，进入该选项，点击 `View Source` ，即可看到本次访问 `www.baidu.com` 的HTTP响应。  
![HTTP_11](http://p69er22kd.bkt.clouddn.com/HTTP_11.png)  