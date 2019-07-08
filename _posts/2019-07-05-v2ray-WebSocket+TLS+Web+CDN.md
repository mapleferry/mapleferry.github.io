---
layout: post
title:  "v2ray配置WebSocket+TLS+Web+CDN"
date:   2019-07-05 17:06:00 +800
categories: [v2ray]
excerpt: "介绍VPS支持多站点下的V2ray，WebSocket+TLS+Web模式，并支持CDN转发"
share: true
comments: true
---
官网：[https://www.v2ray.com](https://www.v2ray.com)；

V2Ray(Project V) 相对于 Shadowsocks，V2Ray 更像全能选手，拥有更多可选择的协议 / 传输载体 (Socks、HTTP、TLS、TCP、mKCP、WebSocket )，还有强大的路由功能。

<!--more-->

---
### 一、购买VPS、申请域名
* 想要搭建 V2Ray，就必须要拥有一台VPS；
* 申请域名。  
`确认域名解析服务器可以使用LNMP申请Let'sEncrypt免费SSL证书`

### 二、安装LNMP
***1.安装前检测Screen、Wget是否安装***  
如果没有安装，分别执行命令：  
screen
~~~ dos
yum install screen
~~~ 
wget
~~~ dos
yum install wget
~~~
***2.安装LNMP***  
~~~ dos
wget http://soft.vpser.net/lnmp/lnmp1.6.tar.gz -cO lnmp1.6.tar.gz && tar zxf lnmp1.6.tar.gz && cd lnmp1.6 && ./install.sh lnmp
~~~
***3.检测LNMP*** 
~~~ dos
lnmp status
~~~
***4.添加虚拟站点*** 
~~~ dos
lnmp vhost add
~~~
按申请的域名填写，并选择申请SSL。
![Smithsonian Image](/img/lnmp_vhost_add.jpg)
***5.修改Nginx配置***  
添加站点成功后，在/usr/local/nginx/vhost下找到与域名相同的文件夹，修改conf文件的配置。
~~~ dos
location /v2ray { 
	proxy_redirect off;
	proxy_pass http://127.0.0.1:10086;
	proxy_http_version 1.1;
	proxy_set_header Upgrade $http_upgrade;
	proxy_set_header Connection "upgrade";
	proxy_set_header Host $http_host;
}
~~~
`注：首先确认端口未被使用。端口必须与V2ray的端口保持一致。`


### 三、安装v2ray
***1.安装v2ray***
~~~ dos
bash <(curl -L -s https://install.direct/go.sh)
~~~
此脚本会自动安装以下文件：
* /usr/bin/v2ray/v2ray：V2Ray 程序
* /etc/v2ray/config.json：配置文件
***2.WebSocket + TLS + Nginx配置***  
修改v2ray服务端配置文件/etc/v2ray/config.json
~~~ JavaScript
{
    "log": {
    "loglevel": "warning",
    "access": "/var/log/v2ray/access.log", 
    "error": "/var/log/v2ray/error.log"
  },

  "inbounds": [
    {
      "port": 10086,
      "listen":"127.0.0.1",
      "protocol": "vmess",
      "settings": {
        "clients": [
          {
            "id": "67ad2a37-de3f-4af1-这里一串是UUID",
            "alterId": 95
          }
        ]
      },
      "streamSettings": {
        "network": "ws",
        "wsSettings": {
        "path": "/v2ray"
        }
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    }
  ]
}
~~~
### 四、客户端V2rayN配置
保持配置和v2ray服务端一致即可。  
![图片](/img/v2rayN_set.jpg)
### 五、CDN
当然用CloudFlare了，实现TLS最简单的办法也是CloudFlare。  
注意：  
* 确保域名已经可以在 Cloudflare正常使用  
* 在 Cloudflare 的 Overview 选项卡可以查看域名状态，请确保为激活状态，即是： Status: Active。  
* 在 DNS 选项卡那边添加一个 A 记录的域名解析，假设你的域名是 mydomain.com，并且想要使用 v2ray.mydomain.com 作为翻墙的域名。那么在 DNS 那里配置，Name 写 v2ray，IPv4 address 写你的小鸡的 IP，务必把云朵点灰，然后选择 Add Record 来添加解析记录即可
(如果你已经添加域名解析，请务必把云朵点灰，即是 DNS only)。  
* 当你的V2ray搭建好，nginx配置好后，设置 Crypto和开启中转。  
确保 Cloudflare 的 Crypto 选项卡的 SSL 为 Full并且请确保 SSL 选项卡有显示 Universal SSL Status Active Certificate 这样的字眼，如果你的 SSL 选项卡没有显示这个，不要急，只是在申请证书，24 小时内可以搞定。  
* 在 DNS 选项卡那里，把刚才点灰的那个云朵图标，点亮它，`一定要点亮一定要点亮一定要点亮`。云朵图标务必为橙色状态，即是 DNS and HTTP proxy(CDN)

### 六、参考资料
[https://www.vjsun.com/161.html](https://www.vjsun.com/161.html)  
[https://www.v2izh.com/index.php/archives/29/](https://www.v2izh.com/index.php/archives/29/)  
[http://www.357global.com/archives/699.html](http://www.357global.com/archives/699.html)


[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
