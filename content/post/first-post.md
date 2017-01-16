+++
date = "2016-11-20T14:03:43-08:00"
title = "用Hugo在GitHub上创建自己的个人网站"
tags = [ "Blogging", "Hugo", "Tech", "Tools" ]
categories = [ "Development" ]
series = [ "Hugo" ]
draft = false
+++

# 起源

一直都有想有个地方能记录自己平时的一些想法，并可以作为交流和分享的平台。

我希望的特点有：

- 无论在线，还是不在线，都可以随手拈来。
- 易于保存和备份，不用害怕哪天服务终止。
- 简单，不需要费心管理。
- 有修改历史记录。
- 可以使用自定义域名，便于迁移。

于是各大社交网络和博客网站就被排除在外。而我有用Git保存笔记的习惯(emacs + org mode)，知道GitHub Pages的服务，但却因为各种原因（主要是拖延症）并没有真正开始。

最近因为抢注 <http://yizhang.us> 和 [yiz@github](https://www.github.com/yiz) 成功，觉得应该好好利用一下，于是就有了这第一篇文章。不知道能坚持多久，且写且珍惜。^_^

<!--more-->

# GitHub Pages和Hugo简介

[GitHub Pages](https://pages.github.com)是GitHub提供的免费建站空间。优点是几乎不需要配置，只要注册帐号，按命名规则建立一个git代码仓库，提交的文件就会自动部署到对应`github.io`域名。

但是GitHub Pages只支持静态页面，这就轮到Hugo出场了。

[Hugo](https://gohugo.io)是用go语言开发，基于[Markdown](https://daringfireball.net/projects/markdown/syntax)的静态页面生成引擎，比起GitHub Pages推荐的[Jekyll](http://jekyllrb.com/)，强调速度和轻量，特别符合我的审美哲学。更详细可以参考作者写的[介绍](http://gohugo.io/overview/introduction/)。

我喜欢Hugo的特点有：

- 极简单的安装，下载预先编译好的可执行文件即可。
- 无干扰，Markdown不用考虑格式化的问题，只要按语义标记文章内容就可以了。
- 快，`hugo server`运行起来后只要保存Markdown文件，浏览器会自动刷新看到效果。
- 该有的功能都有，到目前为止还没发现有功能缺失。

# GitHub上的repository设置

GitHub Pages本身需要一个同名代码仓库自动部署，而我用另一代码仓库存放所有源文件。

点击**New Repository**，创建名为`blog`的代码仓库，用于存放文章源代码以及Hugo的配置。

点击**New Repository**，创建名为`username.github.io`的代码仓库，用于存放网页源代码，并会自动发布到 <https://username.github.io> 上。

配置和使用GitHub参考[GitHub官方文档](https://help.github.com/articles/set-up-git/)。

# 初始化Hugo代码库

Hugo的安装，基本没有什么可说的。到[官网](http://gohugo.io/)下载对应系统版本，放到`PATH`任何一个路径即可。我是放到`$USER/bin`。

下面的命令会用hugo建立一个新网站的目录结构，初始化本地Git代码仓库，并添加GitHub作为远程代码仓库：
```
$ hugo new site site-name
$ cd site-name
$ git init
$ git remote add origin git@github.com:username/blog.git
$ git submodule add git@github.com:username/username.github.io.git public
```

# 开始编辑第一篇文章

生成Markdown模版：
```
$ hugo new post/first-post.md
```

用喜欢的编辑器打开`post/first-post.md`尽情写作吧，`emacs`或者`vim`都行，我是不会挑起信仰之争的。^_^

在看到自己的大作前，需要选择一个模版，Hugo是没有自带默认模版的。上 <http://themes.gohugo.io> 挑选吧。我选择了一个简洁的 [bootstrap](http://getbootstrap.com) 模版：
```
$ cd themes
$ git submodule add https://github.com/alanorth/hugo-theme-bootstrap4-blog.git
$ cd ..
```

欣赏一下，运行下面命令并在浏览器打开 <http://localhost:1313> ：
```
$ hugo server --theme=hugo-theme-bootstrap4-blog --buildDrafts
```

# 编辑`config.toml`

`config.toml`是Hugo的配置文件，没仔细研究，有几个基本参数是需要设置一下的，因为中文标题的原因`permalinks`最好用文件名而不是默认的标题：
```
baseurl = "https://username.github.io/"
title = "username's blog"
theme = "hugo-theme-bootstrap4-blog"

copyright = "All Rights Reserved."

[permalinks]
  post = "/:year/:month/:filename/"

[params]
  author = "Name"
```

# 发布文章

下面的命令会生成最终静态文件并发布到GitHub Pages上：

```
hugo undraft content/post/first-post.md
hugo
cd public
git add --all
git commit -m 'initial commit'
git push
```

上传源代码到GitHub：

```
cd ..
git add --all
git commit -m 'initial commit'
git push --set-upstream origin master
```

在这之后，可以用一行命令搞定，献给像我一样的懒癌患者：

```
COMMIT_COMMENT='awesome post'; \
hugo; cd public; git commit -am $COMMIT_COMMENT; git push; cd ..; \
git commit -am $COMMIT_COMMENT; git push
```
