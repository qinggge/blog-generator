---
title: Cookie是什么？
date: 2018-07-24 23:23:47
tags: [Hexo，Github，Git]
---
# Cookie
Cookie 是服务器保存在浏览器的一段文本信息，每个 Cookie 的大小一般不能超过4KB。浏览器每次向服务器发出请求，就会自动附上这段信息。  
Cookie 会保存以下几方面的信息。  
1. Cookie 的名字
2. Cookie 的值
3. Cookie 的到期时间
4. Cookie 所属域名（默认是当前域名）
5. Cookie 生效路径（默认是当前路径）

<!-- more -->
比如，用户访问网址 `www.abc.com` ，服务器向浏览器写入一个 Cookie ，该 Cookie 就会包含 `www.abc.com` 这个域名，以及根目录 `/`。即，这个 Cookie 对该域名的根路径以及它所有的子路径都生效。如果将路径设置为 `/xxx` ，那么这个 Cookie 就只有在访问 `www.abc.com/xxx` 以及它的子路径时才会生效。  
浏览器可以设置不接受 Cookie ，也可以设置服务器发送 Cookie 。  
`window.navigator.cookieEnabled` 会返回一个布尔值，表明浏览器是否打开 Cookie 。  
`document.cookie` 返回当前页面的 Cookie 。  

# Cookie 与 HTTP 协议
Cookie 由 `HTTP` 协议生成，也主要时供 `HTTP` 协议使用。  

## Cookie 的用途
Cookie 主要用于以下三个方面：
* 会话状态管理（如用户登录状态、购物车、游戏分数或其他需要记录的信息）
* 个性化设置（如用户自定义设置、主题等）
* 浏览器行为跟踪（如跟踪分析用户行为等）

Cookie 曾一度用于客户端数据的存储，因当时没有其他合适的存储办法而作为唯一的存储手段，但随着现代浏览器开始支持各种各样的存储方式，Cookie 已经渐渐被淘汰。由于服务器指定 Cookie 后，浏览器的每次请求都会携带 Cookie 数据，会带来额外的性能开销（尤其是在移动环境下）。新的浏览器 API 已经允许开发者直接将数据存储到本地，如使用 `Web Storage` 或者 `IndexedDB`。  

## 创建 Cookie
当服务器收到 `HTTP` 请求后，服务器可以在响应头里添加一个 `Set-Cookie` 选项。浏览器收到响应后通常会保存下 Cookie ，之后对该服务器的每一次请求中都通过 `Cookie` 请求头将 Cookie 信息发送给服务器。另外， Cookie 的过期时间、域、路径、有效期、适用站点都可以根据需求来指定。  
一个简单的 Cookie 可能像这样：  
  
`Set-Cookie: <cookie名>=<cookie值>`  
  
服务器通过该响应头告知客户端保存 Cookie 信息。  
服务器响应也可以包含多个 `Set-Cookie` 字段，如下：  

```HTTP
HTTP/1.0 200 OK
Content-type: text/html
Set-Cookie: username=ohno
Set-Cookie: userhobby=idontknow

[页面内容]
```  
  
## Cookie 的属性
除了 Cookie 的值， `Set-Cookie` 字段还可以附加 Cookie 的属性：  
  
```HTTP
Set-Cookie: <cookie-name>=<cookie-value>; Expires=<date>
Set-Cookie: <cookie-name>=<cookie-value>; Max-Age=<non-zero-digit>
Set-Cookie: <cookie-name>=<cookie-value>; Domain=<domain-value>
Set-Cookie: <cookie-name>=<cookie-value>; Path=<path-value>
Set-Cookie: <cookie-name>=<cookie-value>; Secure
Set-Cookie: <cookie-name>=<cookie-value>; HttpOnly
```  


1. `Expires=<date>`  
    Cookie 的最长有效时间，形式为符合 HTTP-date 规范的时间戳。如果不设置这个属性，那么表示这是一个 __会话期 Cookie__ 。一个会话结束于这个会话被关闭时，这意味着会话期 Cookie 在那时会被移除。  

2. `Max-Age=<non-zero-digit>`
    在 Cookie 失效前需要经过的秒数。一个或者多个非零数字（1~9）。一些老的浏览器（IE6~IE8）不支持这个属性。对于其他浏览器来说，假如二者 （指 `Expires` 和 `Max-Age` ） 均存在，那么 `Max-Age` 优先级更高。  

3. `Domain=<domain-value>`
    指定 Cookie 可以送达的域名。如果没有指定，那么默认值为当前文档访问地址中的主机部分（不包含子域名）。假如指定了域名，那么相当于各个子域名也包含在内了。  

4. `Path=<path-value>`
    指定一个 URL 路径，这个路径必须出现在现在要请求的资源路径中才可以发送 Cookie 头部。字符 `%x2F（'/'）` 可以解释为文件目录分隔符，该目录下的下级目录也满足匹配的条件（例如，path=/docs，那么"/docs"，"/docs/Web/"或者"/docs/Web/HTPP"都满足匹配的条件）

5. `Secure`
    一个带有安全属性的 Cookie 只有在请求使用 SSL 和 HTTPS 协议时才会被发送到服务器。但该方法不应该被使用，因为保密或者敏感信息不应该在 HTTP Cookie 中存储或者传输，因为 Cookie 本身就是不安全的。

6. `HttpOnly`
    设置了 HttpOnly 属性的 Cookie 不能使用 `JavaScript` 经由 `document.cookie` 属性、`XMLHttpRequest` 和 `Request` APIs进行访问，以防范跨域脚本攻击（XSS）。

7. `SameSite=Strict` `SameSite=Lax`
    允许服务器设定一则 Cookie 不随着跨域请求一起发送，这样可以在一定程度上防范跨站请求伪造攻击（CSRF）。