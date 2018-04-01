---
title: 如何搭建一个Hexo博客？
date: 2018-03-28 23:23:47
tags: [Hexo，Github，Git]
---
# 什么是Hexo？
本站由[Hexo](https://hexo.io/zh-cn/)搭建而成。
> Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。  
# 安装Hexo
安装Hexo前，需已在电脑中安装下列程序：
<!-- more -->
  * [Node.js](http://nodejs.cn/)
  * [Git](https://git-scm.com/)  

接下来只需要使用npm即可完成Hexo的安装。  
`$ npm install -g hexo-cli`  
# 博客搭建
安装完Hexo后，需执行以下命令将你的Hexo博客新建在指定的文件夹中：  
`$ hexo init [folder]`  
`$ cd [folder]`  
`$ npm install`  
新建完成之后，指定的文件夹目录将如下所示：  

    .
    ├── _config.yml
    ├── package.json
    ├── scaffolds
    ├── source
    |   ├── _drafts
    |   └── _posts
    └── themes  
## _config.yml
此文件为配置信息，可在此配置大部分参数。
## package.json
此文件为应用程序的信息，已安装的依赖将显示在此文件内：  
```
{
  "name": "hexo-site",
  "version": "0.0.0",
  "private": true,
  "hexo": {
    "version": "3.6.0"
  },
  "dependencies": {
    "hexo": "^3.2.0",
    "hexo-deployer-git": "^0.3.1",
    "hexo-generator-archive": "^0.1.4",
    "hexo-generator-category": "^0.1.3",
    "hexo-generator-index": "^0.2.0",
    "hexo-generator-tag": "^0.2.0",
    "hexo-renderer-ejs": "^0.3.0",
    "hexo-renderer-marked": "^0.3.0",
    "hexo-renderer-stylus": "^0.3.1",
    "hexo-server": "^0.2.0",
    "hexo-wordcount": "^2.0.1"
  }
}  
```
## source
此文件夹是存放用户资源的文件夹，除 `_posts` 文件夹之外，其他开头命名为下划线的文件夹/文件/隐藏文件都将会被忽略。Markdown 和 HTML 文件会被解析并放到 `public` 文件夹，而其他文件会被拷贝过去。
## themes
此文件夹为主题文件夹。　　
# 命令
## init
`$ hexo init [folder]`  
新建一个网站。若不设置参数 `folder` ，则默认在目前的文件夹搭建网站。  
## new
`$ hexo new [layout]　[title]`  
新建一篇文章。默认 `layout` 为 `_config.yml` 文件里的 `default_layout` 参数。若标题 `title` 里包含空格的话，需使用引号括起来。
## generate
`$ hexo generate`  
该命令可简写为：  
`$ hexo g`  
生成静态文件。  
可选参数：  
```
`-d` , `--deploy` 文件生成后立即部署网站  
`-w` , `--watch` 监视文件变动  
```
## server
`$ hexo server`  
此命令可启动本地服务器。默认情况下，访问网址为 `http://localhost:4000/`。   
## deploy
部署网站，该命令可简写为：  
`$ hexo d`  
可选参数 `-g` ，部署之前预先生成静态文件。  
## clean
`$ hexo clean`  
清除缓存文件 (db.json) 和已生成的静态文件 (public)。  
在某些情况（尤其是更换主题后），如果发现对站点的更改无论如何也不生效，则可能需要运行该命令。
# 将网站部署到GitHub上
1. 在 GitHub 上新建一个空 repo，repo 名称是 [`你的用户名.github.io`] （请将你的用户名替换成真正的用户名）。
2. 修改 `_config.yml` 配置文件，编辑网站配置。  
&nbsp;* 把第六行的 `title` 改为你想要的网站标题  
&nbsp;* 把第九行的 `author` 改为你的名字  
&nbsp;* 把最后一行的 `type:` 后面加上 ` git` ，即 `type: git`（注意`git`前有一个空格）。  
&nbsp;* 在最后一行下面新增一行，缩进与`type`平齐，内容为 `repo: [你的GitHub仓库地址]`，如`repo: git@github.com:xxx.github.io`。
3. 安装Git部署插件：  
`$ npm install hexo-deployer-git --save`
4. `$ hexo deploy`
5. 进入 [`你的用户名.github.io`] 对应的repo，打开GitHub Pages功能，点击预览链接。
6. 大功告成！
# 更多设置
请参考[Hexo](https://hexo.io/zh-cn/)官网进行进一步的配置及修改。