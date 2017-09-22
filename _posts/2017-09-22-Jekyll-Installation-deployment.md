---
layout: post
title:  "Jekyll部署配置手册"
date:   2017-09-22 15:30:25 +800
categories: [工具]
excerpt: "介绍Jekyll工具，Jekyll与Hexo、WordPress博客工具的区别，以及Jekyll在本地安装部署和远端部署的方式。"
comments: true
---

### Jekyll是什么

Jekyll是一个简单的免费的Blog生成工具，一个生成静态网页的工具，不需要数据库支持，只用 Markdown (或 Textile)、Liquid、HTML & CSS 就可以构建可部署的静态网站，最关键的是jekyll可以免费部署在Github上，而且可以绑定自己的域名。

更多详细信息可以查看：[jekyll中文文档](http://jekyll.com.cn/)、[jekyll英文文档](https://jekyllrb.com/)

<!--more-->
---
### Jekyll、,hexo、 Wordpress之PK ###
在网上反复查找资料，发现目前博客的搭建一般采用三种方式：WordPress、Hexo、Jekyll。

有什么不同呢，简单的说Jekyll、Hexo不用付费，不用备案，Wordpress需要购买虚拟主机服务。那还各有什么优缺点？
#### 1）Jekyll
**优点：**
* jekyll是一个静态文件生成器，不需要数据库，只要把文章放到对应的目录即可。
* 能部署到github，不需要自己的vps，因为是静态的，迁移起来非常方便。
* 原生支持markdown。现在github默认支持jekyll, 原生的文件如果放到github上，自动帮你生成博客网站。
* 可以直接在github上编辑和发布博客，PC间切换和同步非常方便。

**缺点：**
* liquid语法不友好。
* 没有强大的后台和插件支持，学习成本较高。

#### 2）Hexo
**优点：**
* 搭建博客速度快、免费，可以搭建在 Github 上。
* 操作比 Jekyll 简单，命令少。
* 支持markdown，生成静态博客，低负载、高速度。
  
**缺点：**
* 每次在新电脑都要重新安装和配置编译环境，不适合随时随地写博客。
* 相对Wordpress而言，没有强大的后台和插件支持，学习成本较高。

#### 3）wordpress
**优点：**
* 安装简单方便，很多虚拟主机都提供了Wordpress的一键式安装工具。
* 功能强大，可扩展性高，丰富的插件。
* wordpress对seo搜索引擎友好，收录也快，排名靠前。

**缺点：**
* 对域名空间要求，需要自己购买虚拟主机。
* 迁移成本高，插件装多了会变慢。
* Wordpress对中小型网站是不错的选择，对于大型门户网站，存在数据库、用户管理、内容的分类管理等方面的限制。

---

### 本地安装Jekyll
#### 1）Jekyll安装相当简单，但开始前需要确保在系统里已经有如下配置：
- [Git环境](https://git-scm.com/downloads)（部署到远端以及代码管理）
- [Ruby环境](https://www.ruby-lang.org/en/downloads/)（jekyll基于Ruby开发）
- [包管理器RubyGems](https://rubygems.org/pages/download/)
- Linux, Unix, or Mac OS X（官方文档并不建议在Windows平台安装）
- Mac用户需要安装Xcode和Command-Line Tools，下载方式**Preferences → Downloads → Components**

#### 2）RubyGems安装Jekyll
安装Jekyll：
{% highlight ruby %}$ gem install jekyll{% endhighlight %}

创建博客：
{% highlight ruby %}$ jekyll new newBlog{% endhighlight %}

进入博客目录：
{% highlight ruby %}$ cd newBlog{% endhighlight %}

启动服务：
~~~ ruby 
$ jekyll serve 
~~~
或者
~~~ ruby 
$ jekyll serve  --watch
~~~

在浏览器里输入：`http://127.0.0.1:4000`，就可以看到博客了。

---

### 部署到远端
这里是部署到GitHub Page，除了这个也可以部署到 Gitlab、Coding等，主要就是当做一个免费的服务器使用。
1）github 上创建一个仓库，命名为 username.github.io，例如我的仓库就是 [mapleferry.github.io](https://mapleferry.github.io)，这是标准命名规范。
2）本地创建好的博客用git管理，然后推送到GitHub上（远程仓库中不需要README.md文件，本地需要新建一个README.md文件用于推送到远端）
{% highlight ruby %}
 $ cd newBlog
 $ git init
 $ git add README.md
 $ git commit -m "update blog"
 $ git remote add origin git@github.com:mapleferry/test.git
 $ git push -u origin master
{% endhighlight %}
 
在浏览器中输入 username.github.io 就可以访问该博客了。




[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
