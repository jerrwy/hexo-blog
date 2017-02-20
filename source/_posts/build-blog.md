---
title: 个人博客搭建
date: 2017-02-19 16:22:57
tags:
---

我的博客是利用 [hexo](https://hexo.io/) 在 [github page](https://pages.github.com/) 上搭建的。

主题是用的 [NexT](http://theme-next.iissnan.com/) 主题

然后使用自定义域名 [wangxaing.net.cn](http://wangxaing.net.cn) 替代 [moyunchen.github.io](http://moyunchen.github.io)

## 教程

---

### 安装node

![node](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1487523314647&di=2ec0133574123bd822d40ec6528ae5d8&imgtype=0&src=http%3A%2F%2Fanyinfa.com%2Fcontent%2Fimages%2F2014%2FApr%2F0.jpg)

node安装还是比较简单的,到 [官网](https://nodejs.org/en/download/) 或者 [中文官网](http://nodejs.cn/) 下载安装包后即可点击安装。

安装完成后如何判断node是否安装成功？
在命令行输入
```shell
node -v
```
如果显示
```shell
v6.9.1
```
版本号不一定都是6.9.1,只要是显示了版本号，就表示安装成功

---

### 安装Git

![git](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1487524135710&di=4778cb82a0939fe87d60425737ab6320&imgtype=0&src=http%3A%2F%2Fimages2015.cnblogs.com%2Fblog%2F577880%2F201511%2F577880-20151122023641374-876573777.png)

我使用的是mac电脑,本身就已经装好了git。如果你是linux电脑或者windows，可以到 [官网](https://git-scm.com/downloads) 下载安装包后点击安装。

同理,在命令行输入
```shell
git --version
```

如果显示了当前git客户端版本,则表示安装成功。

---

### 安装hexo
在命令行输入
```shell
npm install hexo -g
```
npm是node的包管理器,上面的命令表示在全局安装hexo模块

---

### 新建hexo项目
在命令行输入
```shell
mkdir blog
cd blog
hexo init
```
项目结构就搭建好了,blog项目的目的是生成博客网站的静态文件，并封装好了提交到github的相关命令

---

### 本地运行

生成静态文件
```shell
hexo generate（hexo g 也可以）
```

本地运行
```shell
hexo server（hexo s 也可以）
```

在浏览器中输入 localhost:4000 即可查看网站效果。

---

### 注册github

![github](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1487524869050&di=387f01cbcdad6fe3b01c2e411c10246a&imgtype=0&src=http%3A%2F%2Fimages2015.cnblogs.com%2Fblog%2F855959%2F201607%2F855959-20160729104619200-2128697779.png)

[GitHub](github.com) 是一个面向开源及私有软件项目的托管平台，因为只支持 Git 作为唯一的版本库格式进行托管，故名 GitHub。

[GitHub](github.com) 于 2008 年 4 月 10 日正式上线，除了 Git 代码仓库托管及基本的 Web 管理界面以外，还提供了订阅、讨论组、文本渲染、在线文件编辑器、协作图谱（报表）、代码片段分享（Gist）等功能。目前，其注册用户已经超过350万，托管版本数量也是非常之多，其中不乏知名开源项目 Ruby on Rails、jQuery、python 等。

在 github 中创建一个 repository, 命名为`[账号名].github.io`。 这个名称不能乱填,因为这个名称是每个github账户的专属github page名称。
 
---

### 配置git地址

找到blog目录中的_config.yml文件,修改以下部分,将账号名换为自己的账号名称即可。
``` javascript
deploy:
  type: git
  repo: https://github.com/[账号名]/[账号名].github.io.git
  branch: master
```

发布网站到github page,运行一下命令
```shell
hexo deploy (hexo d 也可以)
```

---

### 发表文章

运行一下命令,生成文章
```shell
hexo new "文章标题"
```
在blog目录下可以找到文件 /source/_post/文章标题.md

编辑该md文本的内容。

同上,运行命令
```shell
hexo g //生成静态文件
hexo d //发布到github page项目中
```

即可在`[账号名].github.io` 中看到你刚发布的文章。

---

### 更换主题

官方推荐的 [主题列表](https://github.com/hexojs/hexo/wiki/Themes)

我使用的是 NexT 主题

下载主题文件
```shell
git clone <repository> themes/<theme-name>
```

修改hexo配置文件 _config.yml文件
```javascript
theme: [theme name]
```

同上,运行命令
```shell
hexo g //生成静态文件
hexo d //发布到github page项目中
```

---

### 设置自定义域名

github page 支持自定义域名

方法如下:

1. 在 `source/` 目录下 添加`CNAME`文件，内容为 xxx.com   (xxx.com 是你的自定义域名)
2. 先添加一个CNAME，主机记录写@，后面记录值写上你的 http://xxxx.github.io
3. 再添加一个CNAME，主机记录写www，后面记录值也是 http://xxxx.github.io

---

### 高级设置

更多高级设置可以查看 [NexT使用文档](http://theme-next.iissnan.com/getting-started.html)

里面可以设置侧边社交链接、设置字体、代码高亮、打赏功能、友情链接等。

还可以可以添加第三方服务 比如：多说评论 百度统计 阅读次数统计 查询服务等。

---

### 其他

喜欢的可以去我的 [github page 项目](https://github.com/moyunchen/moyunchen.github.io) 点个赞~ 

谢谢观看~

---