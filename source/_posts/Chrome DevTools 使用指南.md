---
title: Chrome DevTools 使用指南
date: 2019-10-25 13:48:33
tags: [Chrome,DevTools,] 
---
# Chrome DevTools 使用指南
## Chrome DevTools 简介
`Chrome DevTools`（Chrome 开发者工具） 是一套内置于 Google Chrome 中的 Web 开发和调试工具，可用来对网站进行迭代、调试和分析。  
<!-- more -->
  
### 打开 `Chrome DevTools`
* 在 Chrome 菜单中选择更多工具→开发者工具
* 在页面元素上单击右键，选择“检查”
* 使用快捷键 `Ctrl+Shift+I` （Windows） 或 `Cmd+Opt+I` （Mac）

### 面板
`Chrome DevTools` 大体分为十大面板：设备模式、元素面板（Element）、控制台面板（Console）、源代码面板（Sources）、网络面板（Network）、性能面板（Performance）、内存面板（Memory）、应用面板（Application）、安全面板（Security）和审查面板（Audits）。

## 1. 设备模式
使用设备模式可以大致了解页面在移动设备上呈现的外观和效果。  

点击 `Toggle Device Toolbar` 就可以打开模拟移动设备的界面：  
![1](device-1.png)  
设备工具栏在默认情况下，打开时处于自适应视口模式（Responsive），拖动边框可以调整视口大小，也可以在视口模式右侧输入数值。  
  
### 模拟特定移动设备的尺寸  
要模拟特定的移动设备的尺寸，可以在 Device 列表中选择：  
![2](device-2.png)  
点击 iPhone X 时，右侧宽高框就会展示为iPhone X 对应的宽高，其他设备同上，如图：  
![3](device-3.png)  
如果在 Device 列表中没有找到需要的设备，可以点击 “Edit...” 以寻找更多设备或自行添加设备，如图：  
![4](device-4.png)  

### 旋转视口
点击 Rotate 可以将视口旋转为横向或者纵向：  
![5](device-5.png)  
如果 Device Toolbar 布局较窄，则 Rotate 按钮会消失。  

### 设备工具栏
![6](device-6.png)  
设备工具栏从上到下分别是：  
* `Show device frame` （显示设备框架）  
    显示设备视口的物理框架（如果看不到特定设备的设备框架，则可能意味着 DevTools 没有该特定选项的效果图）
* `Show media queries` （显示媒体查询）  
    在视口上方显示媒体查询断点
* `Show rulers` （显示标尺）  
* `Add device pixel ratio` （添加设备像素比）
* `Add device type` （添加设备类型）
* `Capture screenshot` （视口截图，可显示物理框架）
* `Capture full size screenshot` （全尺寸视口截图，不显示物理框架）  
  
图示： 
![7](device-7.png)  

### 限制网络流量和 CPU 占用率
要限制网络流量和 CPU 占用率，可以从 Throttle 列表中选择 `Mid-tier mobile` 或 `Low-end mobile`。  
`Mid-tier mobile` 可模拟快速 3G 网络，并限制 CPU 占用率，以使模拟性能比普通性能低 4 倍。 `Low-end mobile` 可模拟慢速 3G 网络，并限制 CPU 占用率，以使模拟性能比普通性能低 6 倍（限制是相对与笔记本电脑或是桌面设备的普通性能而言）。如果 Device Toolbar 布局较窄，则会隐藏 Throttle 列表。  
也可以在性能面板中，只对 CPU 占用率做限制，而不限制网络流量。  

使用设备模式时，实际上不是在移动设备上运行代码，而是在笔记本电脑或是桌面设备上模拟移动用户体验。由于移动设备的 CPU 架构与笔记本电脑和桌面设备的 CPU 架构大相径庭，因此，如果需要在移动设备（安卓设备）上实际调试代码时，可以用“[远程调试](https://developers.google.com/web/tools/chrome-devtools/remote-debugging/)”功能在笔记本电脑或桌面设备上查看、更改、调试和分析页面代码。  

## 2. 元素面板（Elements）
元素面板中的 DOM 树视图可以显示当前网页的 DOM 结构。通过 DOM 更新实时修改页面的内容和结构。  

点击 `Elements` 即可切换到元素面板。
`Elements` 面板可以检查和实时编辑页面的 HTML 和 CSS 。  
![1](element-1.png)  
元素面板最下方是一个面包屑导航，最左侧是最父级元素，最右侧是最子级元素。

### 编辑 DOM 节点
双击选定元素，然后就可以进行更改：  
![2](element-2.png)  

### 编辑样式
选中一个元素后，在 `Styles` 窗格中就可以实时编辑样式属性名称和值，除了灰色部分（与 User Agent 样式表一样）外，所有样式都可以修改：  
![3](element-3.png)  

### 添加class或伪类效果
`Styles` 窗格可以给选中的元素添加 class 或伪类。在 `Filter` 输入框的右侧： `:hov` 选项可以给元素增加伪类，如 `:hover`、`:active` 等； `:cls` 可以给选中的元素添加或删除 class。

### 检查和编辑盒子模型参数
使用 `Computed` 窗格可以检查和编辑当前选中元素的盒子模型参数。所有值都可以单击修改：  
![4](element-4.png)  

默认情况下，上图的矩形会包含当前元素的 `padding`、`border`、`margin`属性的 `top`、`bottom`、`left`、`right` 值和元素的宽高。对于元素的 `position` 值不是 static 的元素，还会显示 `position` 矩形，包含`top`、`bottom`、`left`、`right` 值：  
![5](element-5.png)  

### 查看本地更改
要查看对页面所做的实时编辑的历史纪录，可以执行以下操作：  
1. 在 `Styles` 窗格中，点击修改的文件。DevTools 会跳转到 `Sources` 面板。
2. 右键点击文件。
3. 选择 `Local modifications`。  

### DOM节点的设置
右键点击一个元素，弹出如下弹窗：  
![6](element-6.png)  
从上到下依次是：  
1. 添加属性（可以给该元素添加属性）
2. 编辑
3. 删除元素
4. 复制  
    包含剪切、复制、粘贴、复制outerHTML、复制选择器、复制 JS 路径（例：`document.querySelector("#cst")`）、复制 XML 路径（例：`//*[@id="cst"]`）
5. 隐藏（visibility设为hidden）
6. 强制状态（:active、:hover等）
7. 断点（子树修改、属性修改、节点移除时，添加的断点会在 `DOM Breakpoints` 中显示）  
8. 全部展开
9. 全部收回
10. 视口跳转到选中的元素
11. 聚焦（输入框）
12. 保存为一个全局变量（在控制台面板中）

### Event Listeners窗格
`Event Listener` 窗格可以查看与 DOM 节点关联的 JavaScript 事件侦听器。
![7](element-7.png)  
  
## 3. 控制台面板（Console）
使用控制台面板，可以记录 `console` 信息，也可以作为 Shell 使用 JS 在页面上进行交互。  
在其他面板中，按下 `Esc` 键可以弹出控制台面板，以方便使用。  

### 消息堆叠
如果一个消息连续重复，会在左侧显示一个数字，表示该消息重复的次数：  
![1](console-1.png)  
可以为每一个消息添加一个独特的标志：  
![2](console-2.png)  
  
清除控制台历史纪录：  
1. 点击 `Clear console`
2. 快捷键 `Ctrl + L` 
3. 调用 `clear()`
4. JS 代码中调用 `console.clear()`  

### 过滤输入
可以通过输入内容，或在左侧选择消息类型的方式来过滤输出。  

## 4. 源代码面板（Sources）
使用源代码面板可以设置断点来调试 JS。  
![1](sources-1.png)  
  
### 断点
源代码面板里的断点分为7个类型：  
1. 代码行
2. 条件代码行
3. DOM
4. XHR
5. 事件监听器
6. 异常
  
#### 代码行断点
找到需要断点的文件，找到需要断点的代码，点击左侧的行号，即可设置断点：  
![2](sources-2.png)  
相当于在代码中写 `debugger`。
  
#### 条件代码行断点
右键点击需要做条件限制的代码行号，选择 `Add conditional breakpoint` ，在下方会出现一个输入框，输入需要设置的条件，回车，即可设置一个条件代码行断点：    
![3](sources-3.png)  
  
#### DOM代码行断点
已在元素面板中说明。  
  
#### XHR断点
使用在 XHR 的请求网址包含指定字符串时中断。  
![4](sources-4.png)  

#### 事件监听器断点
在触发事件后设置断点，可以使用事件监听器断点：  
![5](sources-5.png)  
因页面只绑定了 `mouseenter` 事件，监听 `mouseover` 不会成功中断。  
  
#### 异常断点
点击右上角 `Pause on exceptions` 可以引发捕获异常时暂停。  
  
### Snippets
![6](sources-6.png)  
（showmodaldialog方法）

## 5. 网络面板（Network）
网络面板可以记录页面上的网络请求的详情信息，从发起网页页面请求 Request 后分析 HTTP 请求后得到的各个请求资源信息（包括状态、资源类型、大小、所用时间、 Request 和 Response 等），可以根据这个进行网络性能优化。  
![1](network-1.png)  
  
1. `Controls`： 控制网络面板的外观和功能
2. `Filters`： 控制在 `Request Table` 中显示哪些资源，按住 `Ctrl` 可多选
3. `Overview`： 图表，显示资源检索时间的时间线
4. `Request Table`： 表格，显示检索的每一个资源，默认按时间顺序排序，右键点击表头可显示隐藏信息列
5. `Summary`：此窗格显示了请求总数、传输数据量和加载时间  
  
### Controls
从左到右：打开/关闭记录、清除、打开/关闭屏幕快照、打开/关闭 `Filters`、搜索、放大/缩小表格的请求资源、打开/关闭 `Overview`、按框架分组、保存记录（页面刷新或跳转时不会清除）、缓存开关、网络连接开关及限制网络资源设置。  

### Filters
可筛选需要显示的资源。  

### Overview
显示获取到资源的时间轴信息，按时间显示各个请求加载的开始时间，结束时间。可以在该图表中选择时间区域，筛选在 `Request Table` 中展示的请求资源。双击可以切换为所有时间段。  

### Request Table
按时间排序的请求资源，右键点击表头可显示/隐藏信息列：  
![3](network-3.png)  

点击可显示详细信息：  
![2](network-2.png)  
详细信息中包括以下窗格：  
1. `Headers`： 可以看到 `Request Url`、`Method` 等基本信息和详细的 `Response Headers` 、`Request Headers` 以及 `Form Data` 等信息。
2. `Previews`： 可以根据选择的资源类型（JSON、图片、文本、JS、CSS）显示相应的预览信息。
3. `Response`： 可以根据选择的资源类型（JSON、图片、文本、JS、CSS）显示相应的响应内容。
4. `Cookies`： Cookies信息，如果选择的资源在 `Request` 和 `Response` 过程中存在 Cookies 信息，则该标签会显示出来：
    ![5](network-5.png)  
    * `Name`： Cookies 的名称
    * `Value`： Cookies 的值
    * `Domain`： Cookies 所属域名
    * `Path`： Cookies 所属URL
    * `Expire/Max-Age`： Cookies 存活时长
    * `Size`： Cookies 字节大小
    * `HTTP`： 表示 Cookies 只能被浏览器设置，而且JS不能修改
    * `Secure`： 表示 Cookies 只能在安全连接上传输
5. Timing： 可以显示资源在整个请求生命周期过程中各部分时间花费信息：
    ![4](network-4.png)  
    * `Queued at ***`： 开始排队的时间
    * `Started at ***`： 开始请求的时间
    * `Queueing`： 排队的时间花费
    * `Stalled`： 从HTTP连接建立，到请求能被发送出去的时间花费
    * `Proxy negotiation`： 与代理服务器连接的时间花费
    * `DNS Lookup`： 执行 DNS 查询的时间
    * `Initial Connection`： 建立连接的时间花费
    * `SSL`： 完成 SSL 握手的时间花费
    * `Request sent`： 发起请求的时间花费
    * `Waiting (Time to first byte (TTFB))`： 网络请求被发起到服务器接收到第一个字节的时间，包含了 TCP 连接时间，发送 HTTP 请求时间和获得响应数据第一个字节的时间
    * `Content Download`： 获取相应数据的时间花费

查看请求资源的依赖项和发起项：  
![6](network-6.png)  
按住 `Shfit` 将鼠标悬浮至某一资源上，可以看到其他某些资源被填充为绿色（发起项）/红色（依赖项）背景，该资源上方第一个绿色背景就是该资源的发起者（请求源），第二个（若存在）则是该资源发起者的发起者。该资源下方红色背景的资源都是该资源的依赖项。

### Summary
`Summary` 中显示请求数，数据传输量，加载时间信息：  
![7](network-7.png)  
总共发起27个请求，传输了282KB的数据，获取到782KB的资源，822ms完成请求， `DOMContentLoaded` 在168ms后触发，`Load` 事件在387ms时触发。  
`DOM` 文档的加载流程：  
1. 解析 `HTML` 结构
2. 加载外部 `JS` 和 `CSS` 文件
3. 解析并执行代码
4. 构造 `HTML DOM` 模型 （DOMContentLoaded）
5. 加载图片等外部文件
6. 页面加载完毕 （Load）

## 6. 性能面板（Performance）
使用性能面板可以记录和分析页面在运行时的所有活动。

## 7. 内存面板（Memory）
使用内存面板可以跟踪内存泄露，或分析CPU。
  
## 8. 应用面板（Application）
使用应用面板可以检查加载的所有资源，包括 `Local Storage`、`Session Storage`、`IndexedDB`、`Web SQL`及 `cookie`、图像、字体和样式表等。  
![2](application-2.png)  
应用面板共分为四个部分：`Application`、 `Storage`、 `Cache` 和 `Frames`。  

### Application
`Manifest`、`Service Workers` Web app 相关， `Clear storage` 可清除选定的存储。

### Storage
`Storage` 包含了 `Local Storage`、 `Session Storage` 、 `Cookies` 等存储。  
如果页面使用了 `Local Storage` 或 `Session Storage` 等存储方式，则可以在对应的窗格中看到已存储的键值对：  
![3](application-3.png)  
双击某一键值对的 `Key` 及 `Value` 可以修改对应的值；双击空白单元格可以添加一条；选中一条键值对，点击 X 可以删除；点击 `Clear All` 删除所有键值对。

使用 `IndexedDB` 窗格可以检查、修改和删除 IndexedDB 数据。展开 `IndexedDB` 窗格时，一级目录是数据库：  
![4](application-4.png)  
点击数据库的名称可以查看该数据库的安全源、版本和存储的数据量 `Object stores`。  
![5](application-5.png)  
展开该数据库就可以看到存储的数据。  
增删改的操作与 `Local Storage` 和 `Session Storage` 窗格中一样，但这些改变不会实时更新。点击 `refresh` 按钮可以更新数据库。  

使用 `Web SQL` 窗格可以查询和修改 Web SQL 数据库。  
![6](application-6.png)  

`Cookies` 窗格同网络面板，支持增删改查的操作。  
  
### Cache 
`Cache` 包含 `Cache Storage` 和 `Application Cache`（已从 Web 标准中删除）。  

`Cache Storage` 窗格可以查看、修改和调试使用 `Service workers` 创建的缓存（Service Worker 可以使你的应用先访问本地缓存资源，所以在离线状态时，在没有通过网络接收到更多的数据前，仍可以提供基本的功能）。  
![7](application-7.png)  
  
### Frames 
`Framse` 包含了主文档的 `Fonts`、`Images` 等资源，最后一个就是主文件本身。
![8](application-8.png)  
右键点击某个资源，选择 `Reveal in Network panel` 可以在网络面板中定位该资源。

## 9. 安全面板（Security）
使用安全面板可以调试当前网页的安全和认证等问题，确保你已经在网站上正确地实现 `HTTPS`。  
![1](security-1.png)  
如果网页是安全的，则会提示 “This page is secure (valid HTTPS)”。  
如果网页是不安全的，则会提示 “This page is not secure”。
点击 `View certificate` 可以查看 `Main origin` 的服务器证书信息，点击左侧查看指定源的连接和证书详情。 
![1](security-1.png)   

## 10. 审查面板（Audits）
使用审查面板可以对当前网页进行网络利用情况、网页性能方面的诊断，并给出一些优化建议，比如列出一些没有用到的 CSS 文件等，该功能需要联网并翻墙。  
`Audits` 源于开源自动化分析插件—— `Lighthouse`。`Lighthouse` 不仅能分析页面性能，还能对 `PWA`、无障碍访问、 `SEO` 等进行测试评分，并给出优化建议。相对于性能面板大量且复杂的性能数据，如果开发者经验不足， `Audits` 无疑是更好的选择。  
![1](audits-1.png)  
* `Device`： 选择设备
* `Audits`： 选择需要审查的项目
* `Throttling`： 选择是否需要对 CPU 性能进行限制

点击 `Run audits`，开始进行审查。如果没有联网或翻墙，会一直显示 `Lighthouse is warming up`。审查成功后，窗格展示如下：  
![2](audits-2.png)  

若勾选全部审查功能，则窗格会显示以下内容：  
1. 测试评分，包括所有勾选的内容
2. `Performance` ，网页性能
3. `PWA` ，检查网页对于 `PWA` 的兼容性
4. `Accessibility` ，辅助功能
5. `Best Practice` ，最佳实践
6. `SEO` ，`SEO` 优化  

## 总结 
正所谓：知己知彼，百战不殆。只有熟悉了开发者工具，才能更有条不紊地进行开发工作。`Chrome Devtools` 是一个非常强大的调试工具，从调试页面的结构、样式，查看网络请求，存储调试，到分析并优化网站性能，可谓无所不能。在以后进行网站调试时，可以根据上述内容适当的对网站进行性能分析和性能优化，而非永远只用控制台进行代码debug，用网络面板查看网络请求，把95%的花在了5%的功能上。  