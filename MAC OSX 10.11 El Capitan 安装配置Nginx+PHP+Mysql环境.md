### MAC OSX 10.11 El Capitan 安装配置Nginx+PHP+Mysql环境


>10.10以前一直都是使用自带的apache+php的环境，但是自带的PHP版本不新，而且有些PHP的插件并没有安装上，所以这次升级了10.11就准备直接重新安装PHP，而且现在我的服务器用的都是Nginx，所以，就打算索性直接也安装一个nginx好了。在此记录一下安装过程。

### MAC环境下有一个好用的软件安装方法，使用homebrew来安装各个软件既快捷又不用考虑依赖包的问题。

1. 先安装homebrew，官网：http://brew.sh/index_zh-cn.html，终端下执行：

>ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
一句命令就安装完成了。

2. 安装Nginx，终端下执行：


> brew install nginx
安装完成后，使用浏览器打开http://localhost:8080 ，如果展示页面正常，证明安装完成。



>nginx有以下几个操作命令（命令执行不成功，请使用sudo提权）：

#打开 nginx
>nginx
#nginx -s 重新加载配置|重启|停止|退出
nginx -s reload|reopen|stop|quit
#测试配置是否有语法错误
nginx -t

* 我本次安装的nginx版本是1.8.0，所以，默认访问目录为/usr/local/Cellar/nginx/1.8.0/html

* nginx的配置文件位置为：/usr/local/etc/nginx/nginx.conf

* 如果需要开机自动启动nginx，执行

>mkdir -p ~/Library/LaunchAgents
cp /usr/local/Cellar/nginx/1.8.0/homebrew.mxcl.nginx.plist ~/Library/LaunchAgents/
launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.nginx.plist


3. 安装Mysql，终端执行


* brew install mysql安装完成后默认没有密码，修改密码


>mysql -uroot -p
Enter password: 【输入原来的密码，安装完默认为空，直接回车】
mysql>use mysql;
mysql> UPDATE user SET password=password("修改的密码") WHERE user='root';
mysql的操作命令

#开启
>mysql.server start
#结束
>mysql.server stop

 * 我本次安装的Mysql版本是5.6.27，配置文件位置
>/usr/local/Cellar/mysql/5.6.27/my.cnf



* 需要开机自启动执行
>mkdir -p ~/Library/LaunchAgents/
cp /usr/local/Cellar/mysql/5.6.27/homebrew.mxcl.mysql.plist ~/Library/LaunchAgents/
launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist


4. 安装php，添加扩展库，终端执行

>liondeMacBook-Pro:~ lion$ brew update
liondeMacBook-Pro:~ lion$ brew tap homebrew/dupes
liondeMacBook-Pro:~ lion$ brew tap homebrew/versions
liondeMacBook-Pro:~ lion$ brew tap homebrew/php

* 可以查看有哪些版本可供安装
 >brew search php

* 我本次安装的是php7.0.12，默认安装方法
> brew install php70

 * 也可以通过查看可选选项来自定义安装
>brew options php70
brew install php70 --with-debug 等等

* 安装完成以后，由于OSX 10.11自带php5.5.39，所以，需要把默认指向的位置改为新安装的php位置

>echo 'export PATH="$(brew --prefix homebrew/php/php56)/bin:$PATH"' >> ~/.bash_profile  #for php
echo 'export PATH="$(brew --prefix homebrew/php/php56)/sbin:$PATH"' >> ~/.bash_profile  #for php-fpm
echo 'export PATH="/usr/local/bin:/usr/local/sbin:$PATH"' >> ~/.bash_profile #for other brew install soft
source ~/.bash_profile  #更新配置

* 执行完成以后，可以使用一下命令来检测是否是自己安装的新php

>php -v
php-fpm -v

* php配置文件位置
> /usr/local/etc/php/7.0/php.ini

* php-fpm配置文件位置
>/usr/local/etc/php/7.0/php-fpm.conf
启动 php-fpm 的话就直接在终端里执行 "php-fpm"，默认打开 php-fpm 会显示一个状态 shell 出来，也可以把 php-fpm 的配置文件里的 "daemonize = no" 改为 "daemonize = yes"，就会以后台守护进程的方式启动，对于刚修改的配置文件，可以执行 "php-fpm -t" 来检测配置有没有问题。

* 如果需要开机自动启动php-fpm，执行

>mkdir -p ~/Library/LaunchAgents
cp /usr/local/Cellar/php56/5.6.14/homebrew-php.josegonzalez.php56.plist ~/Library/LaunchAgents/
launchctl load -w ~/Library/LaunchAgents/homebrew-php.josegonzalez.php56.plist

5. 配置nginx
打开nginx的配置文件在最后的 “ } ” 之前添加一句

* include vhost/*.conf;
 请无视这一行/*

* 用于引入vhost目录下的其他配置文件，同时新建目录
 >/usr/local/etc/nginx/vhost

* 在目录里面新建一个配置文件，例如：8001.phpmyadmin.conf， 这里以phpmyadmin为例，内容为

> server
    {
        listen 8001;
        #listen [::]:80;
        server_name localhost;
        index index.html index.htm index.php default.html default.htm default.php;
        root  /Users/leo/web/8001_phpMyAdmin; #这里是你的项目目录
          #include none.conf;
        #error_page   404   /404.html;
        location ~ [^/]\.php(/|$)
        {
            # comment try_files $uri =404; to enable pathinfo
            try_files $uri =404;
            fastcgi_index index.php;
            include fastcgi.conf;
            fastcgi_pass 127.0.0.1:9000;
            #include pathinfo.conf;
        }
        location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
        {
            expires      30d;
        }

        location ~ .*\.(js|css)?$
        {
            expires      12h;
        }

        access_log off;
    }
6. 全部安装配置完成，启动nginx、php-fpm和mysql即可，访问http://localhost:8001就可以进入phpmyadmin的页面了。

>如果不想开机自启动以上程序，可以写个脚本用于 启动｜停止｜重启


* 新建bh_nmp.sh，内容如下：

>#!/bin/bash

param=$1

start()
{
    #启动nginx
    nginx
    #启动mysql
    mysql.server start
    #启动php-fpm
    fpms=`ps aux | grep -i "php-fpm" | grep -v grep | awk '{print $2}'`
    if [ ! -n "$fpms" ]; then
        php-fpm
        echo "PHP-FPM Start"
    else
        echo "PHP-FPM Already Start"
    fi
}

stop()
{
    #停止nginx
    nginx -s stop
    #停止mysql
    mysql.server stop
    #停止php-fpm
    fpms=`ps aux | grep -i "php-fpm" | grep -v grep | awk '{print $2}'`
    echo $fpms | xargs kill -9

    for pid in $fpms; do
        if echo $pid | egrep -q '^[0-9]+$'; then
            echo "PHP-FPM Pid $pid Kill"
        else
            echo "$pid IS Not A PHP-FPM Pid"
        fi
    done
}

case $param in
    'start')
        start;;
    'stop')
        stop;;
    'restart')
        stop
        start;;
    *)
        echo "Usage: ./bh_nmp.sh start|stop|restart";;
esac
