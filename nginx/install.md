---
title: Nginx编译安装及指南
description: 
published: true
date: 2021-07-14T14:39:15.747Z
tags: nginx
editor: markdown
dateCreated: 2021-07-14T14:00:46.375Z
---

## Nginx编译安装及指南

在Debian 10 下载源码编译Nginx，以及相关nginx.conf配置说明。

### 一、安装依赖库

```shell
apt update
#安装依赖：gcc、g++依赖库
apt install build-essential libtool
#安装 pcre依赖库（http://www.pcre.org/）
apt install libpcre3 libpcre3-dev
#安装 zlib依赖库（http://www.zlib.net）
apt install zlib1g-dev
#安装ssl依赖库
apt install openssl libssl-dev
```

上面是一些必要的依赖，执行下面命令全部安装：

```shell
apt install build-essential libtool libpcre3 libpcre3-dev zlib1g-dev openssl libssl-dev
```

### 二、下载Nginx源码编译

1. `Nginx`源码下载

    Nginx官方下载并解压[https://nginx.org/en/download.html](https://nginx.org/en/download.html)

    ```shell
    wget https://nginx.org/download/nginx-1.19.2.tar.gz -O - | tar -xz
    ```

2. 打入插件

    ```shell
    cd nginx-1.19.2

    # --prefix是各个文件路径，--with是打入的插件

    ./configure --prefix=/etc/nginx \
    --sbin-path=/usr/sbin/nginx \
    --modules-path=/usr/lib/nginx/modules \
    --conf-path=/etc/nginx/nginx.conf \
    --error-log-path=/var/log/nginx/error.log \
    --http-log-path=/var/log/nginx/access.log \
    --pid-path=/var/run/nginx.pid \
    --lock-path=/var/run/nginx.lock \
    --http-client-body-temp-path=/var/cache/nginx/client_temp \
    --http-proxy-temp-path=/var/cache/nginx/proxy_temp \
    --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp \
    --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp \
    --http-scgi-temp-path=/var/cache/nginx/scgi_temp \
    --user=nginx \
    --group=nginx \
    --with-compat \
    --with-file-aio \
    --with-threads \
    --with-http_addition_module \
    --with-http_dav_module \
    --with-http_flv_module \
    --with-http_gunzip_module \
    --with-http_gzip_static_module \
    --with-http_random_index_module \
    --with-http_realip_module \
    --with-http_secure_link_module \
    --with-http_slice_module \
    --with-http_sub_module \
    --with-mail --with-mail_ssl_module \
    --with-stream_realip_module \
    --with-stream_ssl_module \
    --with-stream_ssl_preread_module \
    --with-pcre \
    --with-stream \
    --with-http_mp4_module \
    --with-http_auth_request_module \
    --with-http_stub_status_module \
    --with-http_ssl_module \
    --with-http_v2_module

    # 编译

    make
    make install
    ```

    3. 检查`nginx`

        编译完成后就可以用`nginx -t`检查一下Nginx有没有什么错误提示，如下显示即为OK。

        ```shell
        nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
        nginx: configuration file /etc/nginx/nginx.conf test is successful
        ```

    如果出现报错的话可以按下面方法修复。

### 三、常见报错

1. 启动Nginx前先用下面命令检查`80/443`端口有没有被占用：

    ```shell
    netstat -lntp

    # 若未安装netstat工具,安装即可

    apt install net-tools
    ```

2. `nginx -t`常见报错：

    如下错误说明nginx用户不存在，创建nginx用户即可

    ```shell
    nginx: [emerg] getpwnam("nginx") failed in /etc/nginx/nginx.conf:2
    nginx: configuration file /etc/nginx/nginx.conf test failed

    useradd nginx
    ```

    如果出现下错误，说明client_temp文件夹不存在，创建一个即可

    ```shell
    nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
    nginx: [emerg] mkdir() "/var/cache/nginx/client_temp" failed (2: No such file or directory)
    nginx: configuration file /etc/nginx/nginx.conf test failed

    mkdir -p /var/cache/nginx/client_temp
    ```

### 三、创建`systemd`服务

1. 创建`nginx.service`文件

    ```shell
    vi /lib/systemd/system/nginx.service
    ```

    添加内容如下：

    ```shell
    [Unit]
    Description=The NGINX HTTP and reverse proxy server
    After=syslog.target network-online.target remote-fs.target nss-lookup.target
    Wants=network-online.target

    [Service]
    Type=forking
    PIDFile=/var/run/nginx.pid
    ExecStartPre=/usr/sbin/nginx -t
    ExecStart=/usr/sbin/nginx
    ExecReload=/usr/sbin/nginx -s reload
    ExecStop=/bin/kill -s QUIT $MAINPID
    PrivateTmp=true

    [Install]
    WantedBy=multi-user.target
    ```
    
    重载服务：`systemctl daemon-reload`,然后就可以启动Nginx了：`systemctl start nginx`。
    
2. `Nginx`的常用命令

    ```shell
    systemctl enable nginx # 设置开机自动启动
    systemctl status nginx # 查看运行状态，显示running表示成功运行
    systemctl reload nginx # 重新载入
    systemctl restart nginx # 重新启动
    systemctl stop nginx # 立刻停止运行Nginx
    nginx -t # 测试配置文件
    ```
    
### 四、配置`nginx.conf`

