---
author: 飞雪无情
comments: true
date: 2013-12-27 12:59:13+00:00
layout: post
slug: linux-php-server-with-nginx
title: 使用Nginx搭建PHP服务器
wordpress_id: 217
categories:
- Linux
tags:
- Apache
- Linux
- Nginx
- PHP
- Ubuntu
---

一般我们都是采用Apache 作为PHP的解析服务器，这次则是采用Nginx这个强大的反向代理服务器来搭建PHP服务器。下面就以Linux发行版Ubuntu为例搭建一个Nginx的PHP服务器。

首先下载安装Nginx

    
    sudo apt-get install nginx


安装完成后，启动Nginx

    
    sudo /etc/init.d/nginx start


这时候打开浏览器里输入http://localhost/就可以看到Welcome to nginx!的页面了，说明我们的Nginx服务器安装成功

接下来安装PHP5

    
    sudo apt-get install php5-fpm


安装成功后，我们要修改Nginx的虚拟机配置，让浏览器请求的php文件可以被php cgi解析。编辑Nginx虚拟机配置文件/etc/nginx/sites-available/default

    
    sudo vim /etc/nginx/sites-available/default


然后把里面的配置修改为如下配置内容：

    # You may add here your
    # server {
    #	...
    # }
    # statements for each of your virtual hosts to this file
    
    ##
    # You should look at the following URL's in order to grasp a solid understanding
    # of Nginx configuration files in order to fully unleash the power of Nginx.
    # http://wiki.nginx.org/Pitfalls
    # http://wiki.nginx.org/QuickStart
    # http://wiki.nginx.org/Configuration
    #
    # Generally, you will want to move this file somewhere, and start with a clean
    # file but keep this around for reference. Or just disable in sites-enabled.
    #
    # Please see /usr/share/doc/nginx-doc/examples/ for more detailed examples.
    ##
    
    server {
    	listen   80; ## listen for ipv4; this line is default and implied
    	#listen   [::]:80 default ipv6only=on; ## listen for ipv6
    
    	root /usr/share/nginx/www;
    	index index.html index.htm index.php;
    
    	# Make site accessible from http://localhost/
    	server_name localhost;
    
    	location / {
    		# First attempt to serve request as file, then
    		# as directory, then fall back to index.html
    		try_files $uri $uri/ /index.html;
    		# Uncomment to enable naxsi on this location
    		# include /etc/nginx/naxsi.rules
    	}
    
    	location /doc/ {
    		alias /usr/share/doc/;
    		autoindex on;
    		allow 127.0.0.1;
    		deny all;
    	}
    
    	# Only for nginx-naxsi : process denied requests
    	#location /RequestDenied {
    		# For example, return an error code
    		#return 418;
    	# }
    
    	error_page 404 /404.html;
    
    	# redirect server error pages to the static page /50x.html
    	#
    	error_page 500 502 503 504 /50x.html;
    	location = /50x.html {
    		root /usr/share/nginx/www;
    	}
    
    	# pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    	#
    	location ~ \.php$ {
    		fastcgi_split_path_info ^(.+\.php)(/.+)$;
    		# NOTE: You should have "cgi.fix_pathinfo = 0;" in php.ini
    
    		# With php5-cgi alone:
    		fastcgi_pass 127.0.0.1:9000;
    		# With php5-fpm:
    		#fastcgi_pass unix:/var/run/php5-fpm.sock;
    		fastcgi_index index.php;
    		include fastcgi_params;
    	}
    
    	# deny access to .htaccess files, if Apache's document root
    	# concurs with nginx's one
    	#
    	location ~ /\.ht {
    		deny all;
    	}
    }
    
    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #	listen 8000;
    #	listen somename:8080;
    #	server_name somename alias another.alias;
    #	root html;
    #	index index.html index.htm;
    #
    #	location / {
    #		try_files $uri $uri/ /index.html;
    #	}
    # }
    
    # HTTPS server
    #
    #server {
    #	listen 443;
    #	server_name localhost;
    #
    #	root html;
    #	index index.html index.htm;
    #
    #	ssl on;
    #	ssl_certificate cert.pem;
    #	ssl_certificate_key cert.key;
    #
    #	ssl_session_timeout 5m;
    #
    #	ssl_protocols SSLv3 TLSv1;
    #	ssl_ciphers ALL:!ADH:!EXPORT56:RC4+RSA:+HIGH:+MEDIUM:+LOW:+SSLv3:+EXP;
    #	ssl_prefer_server_ciphers on;
    #
    #	location / {
    #		try_files $uri $uri/ /index.html;
    #	}
    # }

重新加载我们刚刚更改的Nginx配置

    
    sudo /etc/init.d/nginx reload


然后我们在/usr/share/nginx/www/目录下新建一个phpinfo.php文件，可以查看php的配置和环境信息

    
    sudo vim /usr/share/nginx/www/phpinfo.php


在phpinfo.php中录入如下内容:

    
    <?php
      phpinfo();
    ?>


我们在浏览器里输入http://localhost/phpinfo.php就可以看到PHP的信息页了，有版本等信息。

PHP5还有很多支持的模块，如果需要的话可以选择安装，一般这些模块都是php5-开头，比如php5-mysql，在Ubuntu里安装他只需

    
    sudo apt-get install php5-mysql


PHP的模块安装后别忘记重启PHP5哦，执行如下命令可以重启

    
    sudo /etc/init.d/php5-fpm restart


