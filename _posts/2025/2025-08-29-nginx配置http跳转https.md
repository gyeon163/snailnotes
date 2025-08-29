---
layout: post
title: "2025-08-29-nginx配置http跳转https"
author: "gyeon"
date: "2025-08-29_15:05:26"
header-img: "img/post-bg-web.jpg"
header-mask: 0.4
tags:
  - Nginx
---

# Nginx配置http跳转https

以域名 `foo.vip` 为例。

```nginx
server {
	server_name foo.vip www.foo.vip;
	location / {
		root /www/foo;
		index index.html;
	}
	listen 443 ssl;
	## 下面是证书配置
	include /etc/letsencrypt/options-ssl-nginx.conf;
	ssl_certificate /etc/letsencrypt/live/foo.vip/fullchain.pem;
	ssl_certificate_key /etc/letsencrypt/live/foo.vip/privkey.pem;
	ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;
}

server {
	if ($host = www.foo.vip) {
		return 301 https://$host$request_uri;
	}
	if ($host = foo.vip) {
		return 301 https://$host$request_uri;
	}
	listen 80;
	server_name foo.vip www.foo.vip;
	return 404;
}

```

