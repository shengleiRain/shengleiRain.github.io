---
layout:     post
title:      "flask+uwsgi后台搭建+分页接口设计"
subtitle:   ""
date:       2020-06-06
author:     "Rain"
header-img: "img/post-bg-2020-06-06.jpg"
tags:
    - flask
    - uwsgi
    - 分页

---


## WSGI协议

WSGI 全称是 Web Server Gateway Interface，也就是 Web 服务器网关接口，它是 Python 语言定义出来的 Web 服务器和 Web 应用程序之间的简单而通用的接口，基于现存的 CGI 标准设计。总的来说，WSGI可分为两部分：服务器和应用程序。WSGI是沟通这两部分的桥梁。

按照 web 组件分类，WSGI 内部可以分为三类，web 应用程序，web 服务器，web 中间件。

## uwsgi的介绍

uwsgi是一种实现WSGI的web服务器，处理从应用程序发送到服务器的请求，并且返回响应。完成 WSGI 的服务端，进程管理以及对应用的调用。

## Flask的介绍

Flask是完成WSGI协议应用程序端的框架。有了框架，开发者就不需要处理 WSGI，框架会帮忙解决这些，开发者只需处理 HTTP 请求和响应。

## Flask框架的部署

### virtualenv

virtualenv是flask项目的虚拟环境，不同的项目可能会引用各种不同的依赖包，为了避免版本与和应用之间的冲突而造成的“依赖地狱”。

```shell
sudo apt install virtualenv
```

然后，在某个python项目的目录下，例如：`/root/python/ImageBackend`下，输入以下指令，构造虚拟环境venv,venv为虚拟环境名。

```shell
virtualenv venv
```

接着输入`source venv/bin/activate`可激活当前虚拟环境，之后的安装部署都在该虚拟环境下进行。

![截屏2020-06-06 下午10.22.01](../assets/images/20200606/%E6%88%AA%E5%B1%8F2020-06-06%20%E4%B8%8B%E5%8D%8810.22.01.png)

在项目中，需要添加多种依赖，可以将这些依赖都写到一个文件中，例如`requirement.txt`

```
Flask
```

安装依赖，可一行指令安装所有项目需要的依赖

```shell
pip install -r requirement.txt
```

## uwsgi的部署

同样在之前的venv虚拟环境中，安装uwsgi

```shell
pip install uwsgi
```

### 使用uwsgi运行项目

新建配置文件`config.ini`

```ini
[uwsgi]
# uwsgi 启动时所使用的地址与端口
#socket = 127.0.0.1:8001 
# 这样相当于只要开放了5000端口，外网ip都能访问5000端口
http = :5000
# 指向网站目录
#chdir = /home/www/ 

# python 启动程序文件
wsgi-file = app.py 

# python 程序内用以启动的 application 变量名
callable = app 

# 处理器数
processes = 1

# 线程数
threads = 2

#状态检测地址
stats = :9191
```

运行

```shell
uwsgi config.ini
```

## supervisor用于进程监控

### 安装

```shell
sudo apt-get install supervisor
```

### 生成默认配置文件

```shell
echo_supervisord_config > supervisord.conf
```

### 将项目运行配置文件，加入到supervisor监控下

在`/etc/supervisor/conf.d`目录下，新建一个文件`image_backend.conf`

```python
[program:image_backend] #image_backend是进程名称
command=/root/python/ImageBackend/venv/bin/uwsgi  /root/python/ImageBackend/config.ini
stopsignal=QUIT
autostart=true
autorestart=true
# 命令程序所在目录
directory=/root/python/ImageBackend
# 运行命令的用户名
user=root
stdout_logfile=/var/log/uwsgi/supervisor_image.log 
stderr_logfile=/var/log/uwsgi/supervisor_image_err.log
```



### 手动开启supervisord进程

```shell
 supervisord -c /etc/supervisor/supervisord.conf
```

### supervisor的几个重要命令

使用supervisorctl命令，来管理进程的停止与运行

### 使用 supervisorctl

Supervisorctl 是 supervisord 的一个命令行客户端工具，启动时需要指定与 supervisord 使用同一份配置文件，否则与 supervisord 一样按照顺序查找配置文件。

```bash
supervisorctl -c /etc/supervisor/supervisord.conf
```

上面这个命令会进入 supervisorctl 的 shell 界面，然后可以执行不同的命令了：

```bash
> status    # 查看程序状态
> stop usercenter   # 关闭 usercenter 程序
> start usercenter  # 启动 usercenter 程序
> restart usercenter    # 重启 usercenter 程序
> reread    ＃ 读取有更新（增加）的配置文件，不会启动新添加的程序
> update    ＃ 重启配置文件修改过的程序
```

上面这些命令都有相应的输出，除了进入 supervisorctl 的 shell 界面，也可以直接在 bash 终端运行：

```bash
$ supervisorctl status
$ supervisorctl stop usercenter
$ supervisorctl start usercenter
$ supervisorctl restart usercenter
$ supervisorctl reread
$ supervisorctl update
```

## 图片后端分页接口设计

当客户端向服务器端请求加载一批图片的时候，尤其是在浏览图片资源网站的时候，服务器端的图片数量通常是海量的，不可能一次性将服务器上的所有图片都发送到客户端上，这时候就需要设计分页接口，来使得客户端一批批地请求加载浏览图片。

在本次搭建的图片后端接口中，采用一个JSON文件充当数据库的角色，里面有300～400条数据，分页接口设计为：`/image?offset=0&limit=10`,offset代表偏移量，即本次请求的图片起始位置，limit为本次请求的图片数量。同时，响应为一个JSON格式数据，如下：

```json
{
    "code": 0,//状态码，0表示请求成功，1表示请求失败，即没有更多的图片数据
    "offset": 0,//偏移量
    "limit": 10,//返回的图片数量
    "data": [
       {
            "url": "http://image.zhangxinxu.com/image/study/s/s256/mm1.jpg",
            "description": "美眉1"
        },...
    ]
}
```

github项目地址：https://github.com/shengleiRain/ImageBackend.git

## 参考文档

[使用 supervisor 管理进程](http://liyangliang.me/posts/2015/06/using-supervisor/)