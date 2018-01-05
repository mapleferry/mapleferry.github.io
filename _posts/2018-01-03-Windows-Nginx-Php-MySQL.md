---
layout: post
title:  "Windows/Nginx/Php/MySQL环境配置"
date:   2018-01-03 17:20:00 +800
categories: [Nginx]
excerpt: "介绍Windows环境下的Nginx/Php/MySQL配置，以及Nginx脚本启动"
share: true
comments: true
---
Nginx是一个 Web服务器，是一款免费开源软件，可用作反向代理，负载平衡器和 HTTP缓存。
相对于Apache、IIS，Nginx的优势在于“反向代理”和“负载均衡”。因此考虑到能够为Web服务器节省资源，它可以代替apache来提供Web服务。在Windows下如何来配置nginx+php+mySQL环境？

<!--more-->
---
### 一、准备程序包
1.Nginx，[http://nginx.org/en/download.html](http://nginx.org/en/download.html)；  
2.Php，[http://php.net/downloads.php](http://php.net/downloads.php)；  
3.MySQL，[https://www.mysql.com/downloads/](https://www.mysql.com/downloads/)；  


### 二、安装配置
***1.php的安装与配置***  
解压下载的php包，到C盘（C:\），将文件夹重命名为php。进入文件夹将php.ini-development文件重命名为php.ini，并用Sublime Text等工具打开它。  

**a)指定Php的ext路径。找到：**
~~~ JavaScript
;extension_dir = "./ext"
~~~
更改为：
~~~ JavaScript
extension_dir = "C:/php/ext"
~~~
**`注意：去掉它前面的分号。`**  

**b)支持MySQL。再找到：**
~~~ JavaScript
;extension = php_mysql.dll 
;extension = php_mysqli.dll
~~~
去掉它前面的分号。  

**c）支持Nginx。找到：**
~~~ JavaScript
;cgi.fix_pathinfo=1
~~~
去掉前面的分号。这是Php的CGI的设置，这一步非常重要。  

**d）其他配置**  
找到：`;date.timezone =` 先去前面的分号再改为 `date.timezone = Asia/Shanghai`  
找到：`enable_dl = Off` 改为 `enable_dl = On`  
找到： `;cgi.force_redirect = 1` 先去前面的分号再改为 `cgi.force_redirect = 0`  
找到： `;fastcgi.impersonate = 1` 去掉前面的分号  
找到：`;cgi.rfc2616_headers = 0` 先去前面的分号再改为 `cgi.rfc2616_headers = 1`  

***2、nginx的安装与配置。***  
进入nginx的conf目录，打开nginx的配置文件nginx.conf，找到：
~~~ JavaScript
location / {
    root html;　#修改（指向站点的根目录）
    index index.html index.htm index.php; 
}
~~~
再找到：
~~~ JavaScript
# pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
#
#location ~ \.php$ {
#    root  html; # 修改（指向站点的根目录）
#    fastcgi_pass   127.0.0.1:9000;
#    fastcgi_index  index.php;
#    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
#    include        fastcgi_params;
#}
~~~
去掉的“#”，将地址指向站点根目录。再把`/scripts`改为`documentroot`，这里的 `document_root`就是指前面“root”所指的站点路径。修改后的：
~~~ JavaScript
location ~ \.php$ {
    root           XXXXX;# 修改（指向站点的根目录）
    fastcgi_pass   127.0.0.1:9000;
    fastcgi_index  index.php;
    fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
    include        fastcgi_params;
}
~~~
https配置如下：
~~~ dos
# HTTPS server
server {
    listen       443 ssl;
    server_name  localhost;
    ssl_certificate      C://nginx//ssl//buduhuisi.crt;
    ssl_certificate_key  C://nginx//ssl//buduhuisi.key;
    ssl_session_cache    shared:SSL:1m;
    ssl_session_timeout  5m;
    ssl_ciphers  HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers  on;
    location / {
        root   XXXXX;# 修改（指向站点的根目录）
        index  index.html index.htm index.php;
    }
}
~~~

### 三、启动服务
~~~ dos
cd c:\php
php-cgi.exe -b 127.0.0.1:9000 -c C:/php/php.ini
~~~
~~~ JavaScript
cd c:\nginx
start nginx
~~~
测试服务
新建文件phpinfo.php，在文件中输入代码：
~~~ php
<?php
    phpinfo();
?>
~~~
浏览器输入 [http://localhost/phpinfo.php](http://localhost/phpinfo.php)

### 四、建立bat脚本
首先把下载好的RunHiddenConsole.zip解压到nginx目录，创建脚本命名为`start_nginx.bat`内容为：
~~~ JavaScript
@echo off
REM Windows 下无效
REM set PHP_FCGI_CHILDREN=5

REM 每个进程处理的最大请求数，或设置为 Windows 环境变量
set PHP_FCGI_MAX_REQUESTS=1000

echo Starting PHP FastCGI...
RunHiddenConsole C:/php/php-cgi.exe -b 127.0.0.1:9000 -c C:/php/php.ini

echo Starting nginx...
RunHiddenConsole C:/nginx/nginx.exe -p C:/nginx
~~~
创建`stop_nginx.bat`脚本关闭nginx：
~~~ JavaScript
@echo off
echo Stopping nginx...
taskkill /F /IM nginx.exe > nul

echo Stopping PHP FastCGI...
taskkill /F /IM php-cgi.exe > nul

exit
~~~

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
