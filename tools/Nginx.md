# Nginx

#### 作者：高天阳
#### 邮箱：gty@haomo-studio.com

```
更改历史

* 2018-02-25        高天阳     合并Nginx文档、添加示例
* 2018-02-09        高天阳     格式化文档
* 2017-07-03        高天阳     初始化文档

```
## 1 介绍、名词解释、特点、原理

### 1.1 介绍

Nginx是一个高性能的HTTP和反向代理服务器，也是一个IMAP/POP3/SMTP代理服务器。

Nginx是一款轻量级的Web服务器/反向代理服务器以及电子邮件代理服务器，并在一个BSD-like协议下发行。
由俄罗斯的程序设计师lgor Sysoev所开发，供俄国大型的入口网站及搜索引擎Rambler使用。
其特点是占有内存少，并发能力强，事实上nginx的并发能力确实在同类型的网页服务器中表现较好。

Nginx相较于Apache\lighttpd具有占有内存少，稳定性高等优势，并且依靠并发能力强，丰富的模块库以及友好灵活的配置而闻名。
在Linux操作系统下，nginx使用epoll事件模型,得益于此，nginx在Linux操作系统下效率相当高。
同时Nginx在OpenBSD或FreeBSD操作系统上采用类似于Epoll的高效事件模型kqueue.

### 1.2 名词解释

#### 1.2.1 代理服务器

一般是指局域网内部的机器通过代理服务器发送请求到互联网上的服务器，代理服务器一般作用在客户端。

#### 1.2.2 反向代理

客户端（用户A）向反向代理的命名空间（name-space）中的内容发送普通请求，接着反向代理将判断向何处（原始服务器）转交请求，
并将获得的内容返回给客户端。而客户端始终任务它访问的事原始服务器B而不是服务器Z。由于防火墙作用，只允许服务器Z进出，
防火墙和反向代理共同作用保护了院子资源服务器B。

![](../../assets/Nginx/nginx1.png)

用途：将防火墙后面的服务器提供给Internet用户访问。反向代理还可以为后端的多台服务器提供负载平衡或为后端较慢的服务器提供缓冲服务。

#### 1.2.3 正向代理

正向代理是一个位于客户端A和原始服务器（服务器B）之间的服务器（代理服务器Z），为了从原始服务器取得内容，
用户A向代理服务器Z发送一个请求并指定目标（服务器B），然后代理服务器Z向服务器B转交请求并将获得的内容返回给客户端。
客户端必须要进行一些特别的设置才能使用正向代理。

![](../../assets/Nginx/nginx2.png)

用途：在防火墙内的局域网客户端提供访问Internet的途径。还可以使用缓冲特性减少网络使用率。

从安全性来讲：

```
正向代理允许客户端通过它访问任意网站并且隐藏客户端自身，因此你必须采取安全措施以确保仅为经过授权的客户端提供服务。

反向代理对外都是透明的，访问者并不知道自己访问的是一个代理。

```
### 1.3 特点 

### 1.3.1 优势
 
1、作为Web服务器，Nginx处理静态文件、索引文件，自动索引的效率非常高

2、作为代理服务器，Nginx可以实现无缓存的反向代理加速，提高网站运行速度

3、作为负载均衡服务器，Nginx既可以在内部直接支持Rails和PHP，也可以支持HTTP代理服务器对外进行服务，
同时还支持简单的容错和利用[算法](http://lib.csdn.net/base/datastructure)进行负载均衡

4、在性能方面，Nginx是专门为性能优化而开发的，实现上非常注重效率。它采用内核Poll模型，可以支持更多的并发连接，
最大可以支持对5万个并发连接数的响应，而且只占用很低的内存资源

5、在稳定性方面，Nginx采取了分阶段资源分配技术，使得CPU与内存的占用率非常低。Nginx官方表示，Nginx保持1万个没有活动的连接，
而这些连接只占用2.5MB内存，因此，类似DOS这样的攻击对Nginx来说基本上是没有任何作用的

6、在高可用性方面，Nginx支持热部署，启动速度特别迅速，因此可以在不间断服务的情况下，对软件版本或者配置进行升级，
即使运行数月也无需重新启动，几乎可以做到7x24小时不间断地运行

Nginx具有很高的稳定性；支持热部署；代码质量非常高，代码很规范，手法成熟，模块扩展也很容易；
采用了一些os提供的最新特性如对sendfile\(Linux2.2+\),accept-filter（FreeBSD4.1+）,TCP\_DEFER\_ACCEPT\(Linux 2.4+\)的支持，
从而大大提高了性能。

### 1.3.2 模块化

高度模块化的设计设Nginx架构的基础。在Nginx中，除了少量的核心代码，其他一切皆为模块。模块化设计具有以下特点：

1、高度抽象的模块接口

2、灵活性

3、配置模块的设计使Nginx提供了高可配置性、高可扩展性、高可定制性、高可伸缩性。

4、核心模块接口简单化

5、多层次、多类别的模块设计

所有模块间是分层次、分类别的，官方Nginx有五大类型的模块：核心模块、配置模块、事件模块、HTTP模块、mail模块。

配置模块和核心模块这两种模块类型是由Nginx的框架代码所定义的，这里的配置模块是所有模块的基础，
它实现了最基本的配置项解析功能（解析nginx.conf文件）。Nginx框架还会调用核心模块，但其他三种模块都不会与框架产生直接关系。
事件模块、HTTP模块、mail模块这三种模块的共性是：实际上它们在核心模块中各有1个模块作为自己的代言人，
并在同类模块中有1个作为核心业务与管理功能的模块。

![](../../assets/Nginx/nginx3.png)

Nginx模块直接被编译仅Nginx,因此属于静态编译方式。启动Nginx后，Nginx的模块被自动加载，不像Apache，
首先将模块编译为一个so文件，然后再配置文件汇总指定是否进行加载。在解析配置文件时，Nginx的每一个模块都有可能去处理某个请求，
但是同一个处理请求只能由一个模块来完成。


### 1.3.3 管理进程、多工作进程设计

Nginx由一个master进程和多个worker进程组成，但master进程或者worker进程中并不会再创建线程。

![](../../assets/Nginx/nginx4.png)

#### 1.3.3.1 master进程和worker进程的作用

master进程

不需要处理网络事件，不负责业务的执行，只会通过管理worker等子进程来实现重启服务、平滑升级、更换日志文件、配置文件实时生效等功能。

master是通过fork系统调用子进程来实现和子进程的通信。

worker进程

```
用来处理master进程fork过来的请求

worker进程是通过处理信号来实现和master通信的

```

#### 1.3.3.2 信号的处理过程

#### Master进程接收到信号是怎样进行处理的？

master进程接收到信号后，会先重新加载配置文件，然后再启动新的进程，并向所有老的进程发送信号，告诉他们可以光荣退休了。
新的进程在启动后，就开始接受新的请求，而老的进程在收到来自master信号后，就不再接收新的请求，
并且在当前进程中的所有未处理完的请求处理完成后再退出。

#### Worker进程接收到信号是怎样进行处理的？

```
首先，worker进程之间是平等的，每个进程，处理请求的机会也是一样的。当我们提供80端口的http服务时，一个连接请求过来，
每个进程都有可能处理这个连接，怎么做到的呢？首先，每个worker进程都是从master进程fork过来的，
在master进程里面，先建立好需要listen的socket之后，然后再fork出多个worker进程，这样每个worker进程都可以去接受这个socket。
一般来说，当一个连接进来后，所有在accept这个socket上面的进程都会收到通知，而只有一个进程可以接受这个连接，
其他的则accept失败，这就是所谓的惊群现象。

那么为了解决这个问题，Nginx提供了一个accept\_mutex（可选项，默认打开）。这是一个加在accept上的一把共享所。有了这把锁之后，
同一时刻，就会只有一个进程在accept连接,这样就不会有惊群问题了。

当一个worker进程在accept这个连接之后，就开始读取请求，解析请求，处理请求，产生数据后，再返回给客户端，最后才断开连接。
一个请求，完全由worker进程来处理，而且只在一个worker进程中处理。

```

#### 1.3.3.3 这样设计的优点

* 利用多核系统的并发处理能力

现在[操作系统](http://lib.csdn.net/base/operatingsystem)都是多核CPU，如果一直是只有一个进程在工作，
那么浪费资源，如果是worker进程间地位不平等势必造成进程瓶颈问题，Nginx为了避免这个问题，设计worker间进程平等。
另外，worker数与cpu核数一致，则会达到CPU资源的充分利用，如果worker数多于cpu核数，势必造成资源的竞争；
若小于cpu核数，势必造成资源浪费。提高网络性能，降低请求时延。

* 负载均衡

多个worker之间通过进程通信来实现负载均衡。即当一个请求到来时，更容易分配到负载较轻的worker进程中处理。
这将降低请求的时延，并在一定程度上提高网络性能

* 管理进程负责监控工作进程的状态，并负责管理其行为

这样做的好处是：

```
a.管理进程不会占用多少系统资源

b.管理进程负责监控工作进程状态，如果某个工作进程死掉，管理进程负责创建出新的工作进程，避免系统性能下降。提高了系统的可靠性。

```
### 1.4 Nginx工作原理

Nginx会按需同时运行多个进程：一个主进程和几个工作进程，
配置了缓存时还会有缓存加载器进程（cache loader）和缓存管理器进程（cache manager\)等。
所有进程均是仅含有一个线程，并主要通过“共享内存”的机制实现进程间通信。主进程以root用户身份运行，
而worker、cache loader和cache manager均应以非特权用户身份运行。

#### 1.4.1 Nginx模块常规的HTTP请求和响应的过程

![](../../assets/Nginx/nginx5.png)

当它接收到一个HTTP请求时，它仅仅是通过查找配置文件将此次请求映射到一个location block,
而此location中配置的各个指令则会启动不同的模块去完成工作，因此模块可以看做Nginx真正的劳动工作者。
通常一个location中的指令会涉及一个handler模块和多个filter模块（当然，多个location可以服用同一个模块）。
handler模块负责处理请求，完成响应内容的生成，而filter模块对响应内容进行处理。

#### 1.4.2 Nginx作为Http反向代理服务器的用法

由于Nginx具有“强悍”的高并发高负载能力，因此一般会作为前段的服务器直接向客户端提供静态文件服务。
但也有一些复杂、多变的业务不适合放到Nginx服务器上，这时会用Apache、Tomcat等服务器来处理。
于是，Nginx通常会被配置为既是静态Web服务器也是反向代理服务器，不适合Nginx处理的请求就会直接转发到上游服务器中处理。

Nginx作为HTTP服务器以及反向代理服务器：

![](../../assets/Nginx/nginx6.png)

Nginx作为反向代理服务器时转发请求的流程：

![](../../assets/Nginx/nginx7.png)

Nginx减轻了上游服务器的并发压力；延长了一个请求的处理时间，并增加了用于缓存请求内容的内存和磁盘空间。

## 2 安装和使用

### 2.1 安装

### 2.2 使用

### 2.3 示例

#### 2.3.1 以张家口OA项目为例

##### 2.3.1.1 连接服务器

```
➜  ~ ssh member@haomo-tech.com
```

```
Welcome to Ubuntu 16.04.2 LTS (GNU/Linux 4.4.0-62-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

155 个可升级软件包。
66 个安全更新。


  ######################################################################
  #                              Notice                                #
  #                                                                    #
  #  1. Please DO NOT upgrade the kernel, as the kernel upgrade would  #
  #   damage the original operating system.                            #
  #                                                                    #
  #  2. Please create unique passwords that use a combination of words,#
  #   numbers, symbols, and both upper-case and lower-case letters.    #
  #   Avoid using simple adjacent keyboard combinations such as        # 
  #   "Qwert!234","Qaz2wsx",etc.                                       #
  #                                                                    #
  #  3. Unless necessary, please DO NOT open or use high-risk ports,   #
  #   such as Telnet-23, FTP-20/21, NTP-123(UDP), RDP-3389,            #
  #   SSH/SFTP-22, Mysql-3306, SQL-1433,etc.                           #
  #                                                                    #
  #                                                                    #
  #                     Any questions please contact 4000-955-988      #
  ######################################################################
```

##### 2.3.1.2 进入Nginx目录

```
member@ecs-4354:~$ cd /etc/nginx/sites-enabled
```

##### 2.3.1.3 查看Nginx目录

```
member@ecs-4354:/etc/nginx/sites-enabled$ ll
```

```
total 92
drwxr-xr-x 2 root root  4096 2月   9 11:48 ./
drwxr-xr-x 7 root root  4096 12月 12 23:00 ../
-rw-r--r-- 1 root root   435 1月  30 18:55 cardbk.conf
-rw-r--r-- 1 root root   356 1月  30 19:13 card.conf
lrwxrwxrwx 1 root root    34 11月 13 18:24 default -> /etc/nginx/sites-available/default
-rw-r--r-- 1 root root  1267 12月 13 19:28 dev.conf
-rw-r--r-- 1 root root  2631 2月   5 14:34 digitizing.conf
-rw-r--r-- 1 root root  1150 1月   2 11:22 etroindocs.conf
-rw-r--r-- 1 root root   971 2月   1 00:10 officedocument.conf
-rw-r--r-- 1 root root   417 1月  31 23:52 onlyoffice.conf
-rw-r--r-- 1 root root   951 2月   1 00:27 ownoffice.conf
-rw-r--r-- 1 root root   976 12月 12 23:03 rocket.conf
-rw-r--r-- 1 root root  2684 12月 25 15:53 seccasadminweb.conf
-rw-r--r-- 1 root root  2747 12月 25 14:56 spoken.conf
-rw-r--r-- 1 root root   241 12月  1 16:40 video.conf
-rw-r--r-- 1 root root   968 12月 14 14:25 video-lingdao.conf
-rw-r--r-- 1 root root   333 2月   2 06:23 wordpress.conf
-rw-r--r-- 1 root root 10115 1月  29 19:40 xuetang.conf
-rw-r--r-- 1 root root  9206 1月  12 15:18 xuetang-dev.conf
```

##### 2.3.1.4 复制一个配置文件范本

```
member@ecs-4354:/etc/nginx/sites-enabled$ sudo cp digitizing.conf /etc/nginx/sites-enabled/zjkweb.conf
```
##### 2.3.1.5 查看Nginx目录

```
member@ecs-4354:/etc/nginx/sites-enabled$ ll
```

```
total 92
drwxr-xr-x 2 root root  4096 2月   9 11:48 ./
drwxr-xr-x 7 root root  4096 12月 12 23:00 ../
-rw-r--r-- 1 root root   435 1月  30 18:55 cardbk.conf
-rw-r--r-- 1 root root   356 1月  30 19:13 card.conf
lrwxrwxrwx 1 root root    34 11月 13 18:24 default -> /etc/nginx/sites-available/default
-rw-r--r-- 1 root root  1267 12月 13 19:28 dev.conf
-rw-r--r-- 1 root root  2631 2月   5 14:34 digitizing.conf
-rw-r--r-- 1 root root  1150 1月   2 11:22 etroindocs.conf
-rw-r--r-- 1 root root   971 2月   1 00:10 officedocument.conf
-rw-r--r-- 1 root root   417 1月  31 23:52 onlyoffice.conf
-rw-r--r-- 1 root root   951 2月   1 00:27 ownoffice.conf
-rw-r--r-- 1 root root   976 12月 12 23:03 rocket.conf
-rw-r--r-- 1 root root  2684 12月 25 15:53 seccasadminweb.conf
-rw-r--r-- 1 root root  2747 12月 25 14:56 spoken.conf
-rw-r--r-- 1 root root   241 12月  1 16:40 video.conf
-rw-r--r-- 1 root root   968 12月 14 14:25 video-lingdao.conf
-rw-r--r-- 1 root root   333 2月   2 06:23 wordpress.conf
-rw-r--r-- 1 root root 10115 1月  29 19:40 xuetang.conf
-rw-r--r-- 1 root root  9206 1月  12 15:18 xuetang-dev.conf
-rw-r--r-- 1 root root   197 2月   9 11:48 zjkweb.conf
```

##### 2.3.1.6 配置Nginx文件

```
member@ecs-4354:/etc/nginx/sites-enabled$ sudo vim zjkweb.conf 
```

```
server {
        listen       80;
        server_name  digitizing.haomo-tech.com;

        location / {

                root   /var/www/html/digitizing;
                index  index.html index.htm;
		try_files $uri$args $uri$args/ $uri $uri/ /index.html =404;
        }

        location /digitizing    {
                proxy_pass    http://localhost:8082/digitizing;
                proxy_set_header  X-Real-IP  $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                
                #add_header Access-Control-Allow-Origin *;
                #add_header Access-Control-Allow-Methods "POST, GET, OPTIONS";
                #add_header Access-Control-Allow-Headers "Origin, Authorization, Accept";
                #add_header Access-Control-Allow-Credentials true;
                if ($request_method = 'OPTIONS') {
                    add_header 'Access-Control-Allow-Origin' '*';
                    add_header 'Access-Control-Allow-Headers' '*';
                    return 204;
                 }
                 if ($request_method = 'POST') {
                    add_header 'Access-Control-Allow-Origin' '*';
                    add_header 'Access-Control-Allow-Headers' '*';
                 }
                 if ($request_method = 'GET') {
                    add_header 'Access-Control-Allow-Origin' '*';
                    add_header 'Access-Control-Allow-Headers' '*';
                 }
        
        }
        location /reptile    {
                proxy_pass    http://localhost:3003;
                proxy_set_header  X-Real-IP  $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

                #add_header Access-Control-Allow-Origin *;
                #add_header Access-Control-Allow-Methods "POST, GET, OPTIONS";
                #add_header Access-Control-Allow-Headers "Origin, Authorization, Accept";
                #add_header Access-Control-Allow-Credentials true;
                if ($request_method = 'OPTIONS') {
                    add_header 'Access-Control-Allow-Origin' '*';
                    add_header 'Access-Control-Allow-Headers' '*';
                    return 204;
                 }
                 if ($request_method = 'POST') {
                    add_header 'Access-Control-Allow-Origin' '*';
                    add_header 'Access-Control-Allow-Headers' '*';
                 }
                 if ($request_method = 'GET') {
                    add_header 'Access-Control-Allow-Origin' '*';
                    add_header 'Access-Control-Allow-Headers' '*';
                 }
    
        }
}
```

![](../../assets/Nginx/nginx8.jpeg)
![](../../assets/Nginx/nginx9.jpeg)

* 关于配置文件的详细讲解

![](../../assets/Nginx/nginx10.jpeg)
![](../../assets/Nginx/nginx11.jpeg)

##### 2.3.1.7 进入代码仓库目录

```
member@ecs-4354:/etc/nginx/sites-enabled$ cd
member@ecs-4354:~$ cd /var/www/html
```

##### 2.3.1.8 查看目录

```
member@ecs-4354:/var/www/html$ ll
```

```
total 1007340
drwxrwxrwx 15 member member       4096 2月   9 10:53 ./
drwxrwxrwx  3 root   root         4096 12月  1 16:32 ../
drwxrwxr-x  3 member member       4096 2月   6 18:10 admin/
drwxrwxrwx  4 member member       4096 1月   4 14:43 admin_test/
drwxrwxr-x  3 member member       4096 1月   2 22:18 code/
drwxrwxr-x  7 member member       4096 1月  26 10:33 demo/
drwxrwxrwx  7 member member       4096 2月   5 14:40 digitizing/
-rw-rw-r--  1 member member 1031449088 1月  24 01:55 Nextcloud_yd.tar
drwxrwxrwx  7 member member       4096 12月 28 17:39 spoken/
drwxrwxr-x  7 member member       4096 2月   8 21:00 static/
drwxrwxrwx  2 member member       4096 12月  8 05:05 video/
drwxrwxr-x  7 member member       4096 1月  30 17:51 wxgame/
drwxrwxr-x  2 member member       4096 1月  30 18:06 wxgamebk/
drwxrwxr-x  6 member member       4096 2月   8 20:59 xuetang/
drwxrwxr-x  3 member member       4096 1月   2 23:25 xuetangx/
```

##### 2.3.1.9 创建张家口项目目录

```
member@ecs-4354:/var/www/html$ mkdir zjk
```
##### 2.3.1.10 查看目录

```
member@ecs-4354:/var/www/html$ ll
```

```
total 1007340
drwxrwxrwx 15 member member       4096 2月   9 10:53 ./
drwxrwxrwx  3 root   root         4096 12月  1 16:32 ../
drwxrwxr-x  3 member member       4096 2月   6 18:10 admin/
drwxrwxrwx  4 member member       4096 1月   4 14:43 admin_test/
drwxrwxr-x  3 member member       4096 1月   2 22:18 code/
drwxrwxr-x  7 member member       4096 1月  26 10:33 demo/
drwxrwxrwx  7 member member       4096 2月   5 14:40 digitizing/
-rw-rw-r--  1 member member 1031449088 1月  24 01:55 Nextcloud_yd.tar
drwxrwxrwx  7 member member       4096 12月 28 17:39 spoken/
drwxrwxr-x  7 member member       4096 2月   8 21:00 static/
drwxrwxrwx  2 member member       4096 12月  8 05:05 video/
drwxrwxr-x  7 member member       4096 1月  30 17:51 wxgame/
drwxrwxr-x  2 member member       4096 1月  30 18:06 wxgamebk/
drwxrwxr-x  6 member member       4096 2月   8 20:59 xuetang/
drwxrwxr-x  3 member member       4096 1月   2 23:25 xuetangx/
drwxrwxr-x  3 member member       4096 2月   9 10:55 zjk/
```

##### 2.3.1.11 配置项目中的部署脚本

```
#!/usr/bin/env bash

npm run build:sit
#rsync -avz dist/* member@60.205.191.90:/data/projects/zjk/
rsync -avz --delete dist/*  member@haomo-tech.com:/var/www/html/zjk/
```

##### 2.3.1.12 部署

```
➜  zhangjiakouweb git:(master) ✗ sh deploy_dev.sh 
```

##### 2.3.1.13 重启Nginx

1. studio服务器重启nginx

```
sudo gitlab-ctl restart nginx
```

2. tech服务器重启nginx

```
sudo pkill -9 nginx
sudo nginx -c /etc/nginx/nginx.conf
```

##### 2.3.1.13 查看效果

访问[http://zjk.haomo-tech.com](http://zjk.haomo-tech.com)测试配置是否成功

#### 2.3.2 以代理转接网址为例

##### 2.3.2.1 连接服务器

```
➜  ~ ssh member@haomo-tech.com
```

```
Welcome to Ubuntu 16.04.2 LTS (GNU/Linux 4.4.0-62-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

155 个可升级软件包。
66 个安全更新。


  ######################################################################
  #                              Notice                                #
  #                                                                    #
  #  1. Please DO NOT upgrade the kernel, as the kernel upgrade would  #
  #   damage the original operating system.                            #
  #                                                                    #
  #  2. Please create unique passwords that use a combination of words,#
  #   numbers, symbols, and both upper-case and lower-case letters.    #
  #   Avoid using simple adjacent keyboard combinations such as        # 
  #   "Qwert!234","Qaz2wsx",etc.                                       #
  #                                                                    #
  #  3. Unless necessary, please DO NOT open or use high-risk ports,   #
  #   such as Telnet-23, FTP-20/21, NTP-123(UDP), RDP-3389,            #
  #   SSH/SFTP-22, Mysql-3306, SQL-1433,etc.                           #
  #                                                                    #
  #                                                                    #
  #                     Any questions please contact 4000-955-988      #
  ######################################################################
```

##### 2.3.2.2 进入Nginx目录

```
member@ecs-4354:~$ cd /etc/nginx/sites-enabled
```

##### 2.3.2.3 查看Nginx目录

```
member@ecs-4354:/etc/nginx/sites-enabled$ ll
```

```
total 92
drwxr-xr-x 2 root root  4096 2月   9 11:48 ./
drwxr-xr-x 7 root root  4096 12月 12 23:00 ../
-rw-r--r-- 1 root root   435 1月  30 18:55 cardbk.conf
-rw-r--r-- 1 root root   356 1月  30 19:13 card.conf
lrwxrwxrwx 1 root root    34 11月 13 18:24 default -> /etc/nginx/sites-available/default
-rw-r--r-- 1 root root  1267 12月 13 19:28 dev.conf
-rw-r--r-- 1 root root  2631 2月   5 14:34 digitizing.conf
-rw-r--r-- 1 root root  1150 1月   2 11:22 etroindocs.conf
-rw-r--r-- 1 root root   971 2月   1 00:10 officedocument.conf
-rw-r--r-- 1 root root   417 1月  31 23:52 onlyoffice.conf
-rw-r--r-- 1 root root   951 2月   1 00:27 ownoffice.conf
-rw-r--r-- 1 root root   976 12月 12 23:03 rocket.conf
-rw-r--r-- 1 root root  2684 12月 25 15:53 seccasadminweb.conf
-rw-r--r-- 1 root root  2747 12月 25 14:56 spoken.conf
-rw-r--r-- 1 root root   241 12月  1 16:40 video.conf
-rw-r--r-- 1 root root   968 12月 14 14:25 video-lingdao.conf
-rw-r--r-- 1 root root   333 2月   2 06:23 wordpress.conf
-rw-r--r-- 1 root root 10115 1月  29 19:40 xuetang.conf
-rw-r--r-- 1 root root  9206 1月  12 15:18 xuetang-dev.conf
-rw-r--r-- 1 root root   197 2月   9 11:48 zjkweb.conf
```

##### 2.3.2.4 复制一个配置文件范本

```
member@ecs-4354:/etc/nginx/sites-enabled$ sudo cp zjkweb.conf /etc/nginx/sites-enabled/vr.conf
```
##### 2.3.2.5 查看Nginx目录

```
member@ecs-4354:/etc/nginx/sites-enabled$ ll
```

```
total 92
drwxr-xr-x 2 root root  4096 2月   9 11:48 ./
drwxr-xr-x 7 root root  4096 12月 12 23:00 ../
-rw-r--r-- 1 root root   435 1月  30 18:55 cardbk.conf
-rw-r--r-- 1 root root   356 1月  30 19:13 card.conf
lrwxrwxrwx 1 root root    34 11月 13 18:24 default -> /etc/nginx/sites-available/default
-rw-r--r-- 1 root root  1267 12月 13 19:28 dev.conf
-rw-r--r-- 1 root root  2631 2月   5 14:34 digitizing.conf
-rw-r--r-- 1 root root  1150 1月   2 11:22 etroindocs.conf
-rw-r--r-- 1 root root   971 2月   1 00:10 officedocument.conf
-rw-r--r-- 1 root root   417 1月  31 23:52 onlyoffice.conf
-rw-r--r-- 1 root root   951 2月   1 00:27 ownoffice.conf
-rw-r--r-- 1 root root   976 12月 12 23:03 rocket.conf
-rw-r--r-- 1 root root  2684 12月 25 15:53 seccasadminweb.conf
-rw-r--r-- 1 root root  2747 12月 25 14:56 spoken.conf
-rw-r--r-- 1 root root   241 12月  1 16:40 video.conf
-rw-r--r-- 1 root root   968 12月 14 14:25 video-lingdao.conf
-rw-r--r-- 1 root root   333 2月   2 06:23 wordpress.conf
-rw-r--r-- 1 root root 10115 1月  29 19:40 xuetang.conf
-rw-r--r-- 1 root root  9206 1月  12 15:18 xuetang-dev.conf
-rw-r--r-- 1 root root   197 2月   9 11:48 zjkweb.conf
-rw-r--r-- 1 root root   197 2月  25 16:38 vr.conf
```
##### 2.3.2.6 配置Nginx文件

```
member@ecs-4354:/etc/nginx/sites-enabled$ sudo vim vr.conf 
```

```
server {
        listen       80;
        server_name  vr.haomo-tech.com;

        location / {
                proxy_pass  https://playcanv.as/;
        }
        
        location /demo/ {
                proxy_pass  https://playcanv.as/p/;
        }
        
        location /css/ {
                proxy_pass  https://playcanv.as/css/;
        }
        
        location /demo/room/ {
                proxy_pass  https://playcanv.as/p/zi09Xvld/;
        }
        
        location /demo/bmw/ {
                proxy_pass  https://playcanv.as/p/RqJJ9oU9/?overlay=fal;
        }
}
```

##### 2.3.2.7 重启Nginx

1. tech服务器重启nginx

```
sudo pkill -9 nginx
sudo nginx -c /etc/nginx/nginx.conf
```

##### 2.3.2.8 查看效果

访问[http://vr.haomo-tech.com/demo/room](http://vr.haomo-tech.com/demo/room)测试配置是否成功
访问[http://vr.haomo-tech.com/demo/bmw](http://vr.haomo-tech.com/demo/bmw)测试配置是否成功

#### 2.3.3 以修改服务器接口返回json为例

##### 2.3.3.1 连接服务器
```
➜  ~ ssh member@haomo-studio.com
```

```
Warning: the ECDSA host key for 'haomo-studio.com' differs from the key for the IP address '115.28.80.125'
Offending key for IP in /Users/haomo/.ssh/known_hosts:2
Matching host key in /Users/haomo/.ssh/known_hosts:7
Are you sure you want to continue connecting (yes/no)? yes
Welcome to Ubuntu 14.04.5 LTS (GNU/Linux 3.13.0-106-generic x86_64)

 * Documentation:  https://help.ubuntu.com/
New release '16.04.3 LTS' available.
Run 'do-release-upgrade' to upgrade to it.


Welcome to aliyun Elastic Compute Service!

You have new mail.
Last login: Sat Feb 24 10:26:03 2018 from 117.136.40.205
```
##### 2.3.3.2 进入Nginx目录
   
```
member@AY140710084449224635Z:~$ cd /etc/nginx/
```

##### 2.3.2.3 查看Nginx目录

```
member@AY140710084449224635Z:/etc/nginx$ ll
```

```
total 104
drwxr-xr-x   6 root root  4096 Jan 30 20:59 ./
drwxr-xr-x 124 root root 12288 Jan 19 07:35 ../
drwxr-xr-x   2 root root  4096 Feb 12  2015 conf.d/
-rw-r--r--   1 root root   911 Mar  5  2014 fastcgi_params
-rw-r--r--   1 root root  2258 Mar  5  2014 koi-utf
-rw-r--r--   1 root root  1805 Mar  5  2014 koi-win
-rw-r--r--   1 root root  2085 Mar  5  2014 mime.types
-rw-r--r--   1 root root  5287 Mar  5  2014 naxsi_core.rules
-rw-r--r--   1 root root   287 Mar  5  2014 naxsi.rules
-rw-r--r--   1 root root   222 Mar  5  2014 naxsi-ui.conf.1.4.1
-rw-r--r--   1 root root     0 Apr  1  2017 nginx
-rwxr-xr-x   1 root root  6747 May 18  2017 nginx.backup.conf*
-rwxrwxrwx   1 root root 14787 Oct 12 15:35 nginx.conf*
-rw-r--r--   1 root root   180 Mar  5  2014 proxy_params
-rw-r--r--   1 root root   465 Mar  5  2014 scgi_params
drwxr-xr-x   2 root root  4096 Mar 29  2017 sites-available/
drwxr-xr-x   2 root root  4096 Feb 25 12:19 sites-enabled/
drwxr-xr-x   2 root root  4096 Nov  6 11:46 ssl/
-rw-r--r--   1 root root   532 Mar  5  2014 uwsgi_params
-rw-r--r--   1 root root  3071 Mar  5  2014 win-utf
```

##### 2.3.3.4 配置nginx.conf文件

```
member@ecs-4354:/etc/nginx/sites-enabled$ sudo vim nginx.conf 
```
```
server {
        listen       80;
        server_name  trendshealth.haomo-studio.com;

        location / {
                root   /var/www/html/pt/TrendsHealth;
                index  index.html index.htm;
                try_files $uri $uri/ @rewrites;
        }

        location @rewrites {
                rewrite ^(.+)$ /index.html last;
        }

        location /api/ {
                proxy_pass    http://localhost:8081/;
                proxy_set_header  X-Real-IP  $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

                #add_header Access-Control-Allow-Origin *;
                #add_header Access-Control-Allow-Methods "POST, GET, OPTIONS";
                #add_header Access-Control-Allow-Headers "Origin, Authorization, Accept";
                #add_header Access-Control-Allow-Credentials true;
        }
}

server {
        listen       80;
        server_name  www.haomo-studio.com haomo-studio.com haomo-tech.com www.haomo-tech.com;
	default_type 'text/html';
	charset utf-8;

        location /books {
                root   /var/www/html/;
                index  index.html index.htm;
        }

        location / {
                root   /var/www/html/official_site;
                index  index.html index.htm;
        }

	location /gitbook/ {
		proxy_pass	https://hxgqh.gitbooks.io/;
		proxy_set_header  X-Real-IP  $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	}

	location /slides/ {
		proxy_pass	http://slides.com/hxgqh/;
		proxy_set_header  X-Real-IP  $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	}
}

server {
        listen       80;
        server_name  skillee.haomo-studio.com;

        location / {
                root   /var/www/html/skillee/SkilleeWeb;
                index  index.html index.htm;
		try_files $uri $uri/ @rewrites;
        }

	location @rewrites {
		rewrite ^(.+)$ /index.html last;
  	}

	location /api/ {
            	proxy_pass    http://localhost:8083/;
		proxy_set_header  X-Real-IP  $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

		#add_header Access-Control-Allow-Origin *;
            	#add_header Access-Control-Allow-Methods "POST, GET, OPTIONS";
            	#add_header Access-Control-Allow-Headers "Origin, Authorization, Accept";
            	#add_header Access-Control-Allow-Credentials true;
        }
}

server {
        listen       80;
        server_name  asmwechat.haomo-studio.com;

	location / {
            	proxy_pass    http://localhost:8181/;
		proxy_set_header  X-Real-IP  $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

		add_header Access-Control-Allow-Origin *;
            	add_header Access-Control-Allow-Methods "POST, GET, OPTIONS";
            	add_header Access-Control-Allow-Headers "Origin, Authorization, Accept";
            	add_header Access-Control-Allow-Credentials true;
        }
	location /MP_verify_v4p2vSiFQZ9PkvXB.txt {
		alias /var/www/html/pt/asm/asmwechat/MP_verify_v4p2vSiFQZ9PkvXB.txt;
        }
}

server {
        listen       80;
        server_name  hmorganization.swagger.haomo-studio.com;

        location / {
		proxy_pass http://115.28.80.125:8807/hz/;
		proxy_set_header  X-Real-IP  $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

                #add_header Access-Control-Allow-Origin *;
                #add_header Access-Control-Allow-Methods "POST, GET, OPTIONS";
                #add_header Access-Control-Allow-Headers "Origin, Authorization, Accept";
                #add_header Access-Control-Allow-Credentials true;
  	}
}

server {
        listen       80;
        server_name  asmwechattest.haomo-studio.com;

        location / {
                proxy_pass    http://localhost:8181/;
                proxy_set_header  X-Real-IP  $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

                add_header Access-Control-Allow-Origin *;
                add_header Access-Control-Allow-Methods "POST, GET, OPTIONS";
                add_header Access-Control-Allow-Headers "Origin, Authorization, Accept";
                add_header Access-Control-Allow-Credentials true;
        }
        location /MP_verify_v4p2vSiFQZ9PkvXB.txt {
                alias /var/www/html/pt/asm/asmwechat/MP_verify_v4p2vSiFQZ9PkvXB.txt;
        }
}

server {
        listen       80;
        server_name  techradar.haomo-studio.com;

        location / {
                root   /var/www/html/techradar;
                index  index.html index.htm;
		try_files $uri $uri/ @rewrites;
        }

	location @rewrites {
		rewrite ^(.+)$ /index.html last;
  	}
}

server {
        listen       80;
        server_name  company.haomo-studio.com;

        location / {
		proxy_pass http://127.0.0.1:12380/;
  	}
}

server {
        listen       443;
        server_name  company.haomo-studio.com;

        location / {
		proxy_pass https://127.0.0.1:12443/;
  	}
}

# Store templates  *.tpl.haomo-studio.com
server {
        listen       80;
        server_name  fuse.tpl.haomo-studio.com;

        location / {
                root   /var/www/html/templates/fuse;
                index  index.html index.htm;
        }
}

server {
        listen       80;
        server_name  iview.tpl.haomo-studio.com;

        location / {
                root   /var/www/html/haomo_tpl/iViewAdminTemplate;
                index  index.html index.htm;
		try_files $uri $uri/ @rewrites;
        }

	location @rewrites {
		rewrite ^(.+)$ /index.html last;
  	}
}

server {
        listen       1080;
        server_name  115.28.80.125 haomo-studio.com;
	default_type 'text/html';
	charset utf-8;

        location / {
                root   /var/www/html;
                index  index.html index.htm;
        }
}

server {
        listen       1081;
        server_name  115.28.80.125 haomo-studio.com;

        location / {
                root   /var/www/html/templates/fuse/;
                index  index.html index.htm;
        }
}

server {
        listen       1085;
        server_name  115.28.80.125 haomo-studio.com;

        location / {
                root   /var/www/html/auction/;
                index  index.html index.htm;
        }
}

server {
        listen       1082;
        server_name  115.28.80.125 haomo-studio.com;

        location / {
                root   /var/www/html/pt/seektruth/web/;
                index  index.html index.htm;
        }
}

server {
        listen       1083;
        server_name  115.28.80.125 haomo-studio.com;

        location / {
                root   /var/www/html/pt/Powersaver/;
                index  index.html index.htm;
        }
}

server {
	listen        1088;
	server_name   115.28.80.125 haomo-studio.com;

	location / {
		root    /alidata1/var/www/html/haomo_books/haomotraining/;
		index   index.html index.htm;
	}

}

server {
        listen       81;
        server_name  115.28.80.125 haomo-studio.com;

        location / {
                root   /var/www/html/templates/fuse/;
                index  index.html index.htm;
        }
}

server {
        listen        1085;
        server_name   115.28.80.125 haomo-studio.com;

        location / {
                root    /var/www/html/skillee/Skilleeh5;
                index   index.html index.htm;
        }
}


server {
        listen        1086;
        server_name   115.28.80.125 haomo-studio.com;

        location / {
                root    /var/www/html/auction;
                index   index.html index.htm;
        }

}

server {
        listen        8191;
        server_name   115.28.80.125 haomo-studio.com;
        add_header Access-Control-Allow-Origin *;
        add_header Access-Control-Allow-Methods "POST, GET, OPTIONS";

        location ~ ^/get_category {
            default_type application/json;

            if ( $query_string ~* ^(.*)type=Person(.*)$ ){
                return 200 '{"categoryName":"资源","categoryId":"23a0a9b812f14a879e0b39c235678dfc","children":[{"categoryName":"服务员","categoryId":"bbe74d31d2064c4586ba040014d2b130","children":[{"categoryName":"技师","categoryId":"bd842c24f36a466ca1a7394c987bae53"}]},{"categoryName":"管理员","categoryId":"0c9074262a594578bd44db73f19dfd78"}]}';
            }

            if ( $query_string ~* ^(.*)type=Equipment(.*)$ ){
                return 200 '{"categoryName":"资源","categoryId":"23a0a9b812f14a879e0b39c235678dfc","children":[{"categoryName":"设备","categoryId":"0c9074262a594578bd44db73f19dfd78"}]}';
            }

            if ( $query_string ~* ^(.*)type=Room(.*)$ ){
                return 200 '{"categoryName":"资源","categoryId":"23a0a9b812f14a879e0b39c235678dfc","children":[{"categoryName":"大包厢","categoryId":"0c9074262a594578bd44db73f19dfd78"}]}';
            }

	    return 200 '{"categoryName":"资源","categoryId":"23a0a9b812f14a879e0b39c235678dfc","children":[{"categoryName":"人","categoryId":"bbe74d31d2064c4586ba040014d2b130","children":[{"categoryName":"技师","categoryId":"bd842c24f36a466ca1a7394c987bae53"}]},{"categoryName":"设备","categoryId":"0c9074262a594578bd44db73f19dfd78"}]}';
	}

        location ~ ^/get_tag {
            default_type application/json;
            return 200 '[{"id":"77f90d0b16a24bd3b8d2ffd3c51286ef","tagName":"房>间明亮","tagDescription":""},{"id":"dd48f14b06884de4ae6146bf88b55d0e","tagName":"国外进口","tagDescription":""},{"id":"7789c19cd18c4912bad07f7737a06119","tagName":"英语流利","tagDescription":""}]';
        }

        location ~ ^/get_info {
            default_type application/json;

	    if ( $query_string ~* ^(.*)type=Person(.*)$ ){
                return 200 '[{"resourceId":"176e20dcce8e418caa5fb4cd786fcfad","resourceName":"3号技师","categoryName":"高级健康管理师","organization":">北京健身会所","description":""}]';
            }

            if ( $query_string ~* ^(.*)type=Equipment(.*)$ ){
                return 200 '[{"resourceId":"176e20dcce8e418caa5fb4cd786fcfad","resourceName":"大音响","categoryName":"器材","organization":"北京音乐组织","description":""}]';
            }

            if ( $query_string ~* ^(.*)type=Room(.*)$ ){
                return 200 '[{"resourceId":"90fba7d22a544e8e90ba7cbadbdc0aab","resourceName":"5号房间","categoryName":"高级房间","organization":"北京健身会所","tag":["静音","有天窗"],"description":""}]';
            }

            return 200 '[{"resourceId":"90fba7d22a544e8e90ba7cbadbdc0aab","resourceName":"5号房间","categoryName":"高级房间","organization":"北京健身会所","tag":["静音","有天窗"],"description":""},{"resourceId":"176e20dcce8e418caa5fb4cd786fcfad","resourceName":"3号技师","categoryName":"高级健康管理师","organization":">北京健身会所","description":""}]';
        }

        location ~ ^/get_attribute {
            default_type application/json;
            return 200 '[{"id":"96beb787ffb440b795e74ba4f7d10922","name":"颜色","attributeType":"List","attributeValueList":[{"id":"a16265f278ed49cfbb12652aa237fff7","name":"红"},{"id":"2e10b9bb9b094860becf7373624151da","name":"黄"},{"id":"87ad4016d7444466a5c38567cc2ebd9f","name":"蓝"}]},{"id":"bf167d81fac145178956a7ecddd07e46","name":"面积","attributeType":"Double"},{"id":"20471db98eba457c8bf4f745eccec46c","name":"重量","attributeType":"Integer"},{"id":"a68902f32d914e1bae6aabbb54d34fe1","name":"制作日期","attributeType":"Date"},{"id":"02852995f87b4066bc737d3c2c7012fd","name":"制作单位","attributeType":"Text"}]';
        }
        location ~ ^/get_position_list {
            default_type application/json;
            return 200 '[{"positionKey":"salesman","postionValue":"销售"},{"positionKey":"SalesManager","postionValue":"销售经理"}]';
        }

        location ~ ^/get_organization {
            default_type application/json;
            return 200 '{"orgName": "生命滙总部","orgId": "69EBAE21B3D347ECA09F82A848AE7E4C","children": [{"orgName": "生命汇上海公司","orgId": "EAF2746C2C444C8B8AFEAA482F5B7164","children": []},{"orgName": "生命滙广州公司","orgId": "FECC04D660E3474FA474CDBA0706F2D1","children": []},{"orgName": "生命滙北京公司","orgId": "280649403EF74F09B583C92E55D7EE45","children": [{"orgName": "生命滙北京公司-市场部","orgId": "665630463717408D96AD66C7BE94F7B4","children": []}]},{"orgName": "生命滙海南公司","orgId": "8CBBE14B384D4E969105AAB56C5E475B","children": []}]}';
        }

        location ~ ^/get_users {
            default_type application/json;
            return 200 '[{"id":" 0003B3F3C5464C62853E05FD72FEDC8A","name":"李颜>粉","position":"销售经理","gender":"M","org":"北京分公司","department":"销售部"},{"id":" 0003B3F3C5464C62853E05FD72FEDC8A","name":"丁秀娟","position":"人力总监","gender":"","org":"总公司","department":"人力资源部"}]';
        }

        location ~ ^/get_roles {
            default_type application/json;
            return 200 '[{"id":"ce4717f895ad40858f0570aad1fc266c","name":"Tijs"},{"id":"2bbe35d54071410cbae6ecbef4629622","name":"Admin"},{"id":"c91334a3fc3d43609af735e3d980f8e1","name":"CEO"}]';
        }

        location ~ ^/get_task_configuration_list {
            default_type application/json;
	    return 200 '[{"key":"UIType","name":"对话框类型","valueOptions":[{"key":"EHRRecord","name":"EHR医疗数据录入"},{"key":"EHRProductConvergenceRecord","name":"EHR医疗数据汇聚录入"},{"key":"HealthReportGeneration","name":"健康管理报>告生成"},{"key":"HealthConsulting","name":"健康问卷录入"},{"key":"CommonTaskProcessDialog","name":"通用任务处理对话框"}]},{"key":"UIContent","name":"输入内容"}]';
	}
        
location ~ ^/get_service_task_url_param {
            default_type application/json;
	    return 200 '[{"key":"MailTemplate","name":"邮件模板","valueOptions":[{"key":"2bbef753961846d2b7a83619b9b34b80","name":"通知"},{"key":"f93c17ef37354f74bbc5f19fd368d701","name":"预定房间"}]},{"key":"filePath","name":"文件路径"}]';
	}

        location ~ ^/get_product_name {
            default_type application/json;
            return 200 '{"productName":"常规五项检查","productDesc":""}';
        }

        location ~ ^/get_candidate_users_list {
            default_type application/json;
            return 200 '["d7ef624720c34b5a92a08a7dc8a2667a","efbe4ba327d34fb7872acc3654b3502c","3c566a01578f4864bef612b94ae9b463","26e0c623194d4a32ab7a5a2e9c1b00eb"]';
        }

        location ~ ^/get_task_execution_listener_response {
            default_type application/json;
            return 200 '{"age":30,"name":{"firstName":"三无","lastName":"张"}}';
        }

}
```

* 找到organization对应接口，修改return里的东西

* 下图为原json

![](../../assets/Nginx/nginx18.png)

* 下图为现json

![](../../assets/Nginx/nginx19.png)

```
'{
    "orgName": "生命滙总部",
    "orgId": "69EBAE21B3D347ECA09F82A848AE7E4C",
    "children": [
        {
            "orgName": "生命汇上海公司",
            "orgId": "EAF2746C2C444C8B8AFEAA482F5B7164",
            "children": []
        },
        {
            "orgName": "生命滙广州公司",
            "orgId": "FECC04D660E3474FA474CDBA0706F2D1",
            "children": []
        },
        {
            "orgName": "生命滙北京公司",
            "orgId": "280649403EF74F09B583C92E55D7EE45",
            "children": [
                {
                    "orgName": "生命滙北京公司-市场部",
                    "orgId": "665630463717408D96AD66C7BE94F7B4",
                    "children": []
                }
            ]
        },
        {
            "orgName": "生命滙海南公司",
            "orgId": "8CBBE14B384D4E969105AAB56C5E475B",
            "children": []
        }
    ]
}';
```

##### 2.3.3.5 重启nginx

```
sudo gitlab-ctl restart nginx
```

### 2.4 最佳实践

#### 2.4.1 未添加`;`导致报错

* 修改前配置文件为下图

![](../../assets/Nginx/nginx13.jpeg)

* 此时报错为下图

![](../../assets/Nginx/nginx12.jpeg)

* 修改后配置文件为下图

![](../../assets/Nginx/nginx14.jpeg)

#### 2.4.2 重启Nginx未加`sudo`导致没有权限报错

* 此时报错为下图

![](../../assets/Nginx/nginx15.jpeg)

#### 2.4.3 代理网址时网址找不到css等文件导致报错

* 此时报错为下图

![](../../assets/Nginx/nginx16.jpeg)
![](../../assets/Nginx/nginx17.jpeg)

* 修改前配置文件如下

```
server {
        listen       80;
        server_name  vr.haomo-tech.com;
        
        location /demo/room/ {
                proxy_pass  https://playcanv.as/p/zi09Xvld/;
        }
        
        location /demo/bmw/ {
                proxy_pass  https://playcanv.as/p/RqJJ9oU9/?overlay=fal;
        }
}
```

* 需要修改配置文件增加代理地址(哪个地址找不到就代理哪个地址)

```
server {
        listen       80;
        server_name  vr.haomo-tech.com;

        location / {
                proxy_pass  https://playcanv.as/;
        }
        
        location /demo/ {
                proxy_pass  https://playcanv.as/p/;
        }
        
        location /css/ {
                proxy_pass  https://playcanv.as/css/;
        }
        
        location /demo/room/ {
                proxy_pass  https://playcanv.as/p/zi09Xvld/;
        }
        
        location /demo/bmw/ {
                proxy_pass  https://playcanv.as/p/RqJJ9oU9/?overlay=fal;
        }
}
```
## 3 同类技术对比

### 3.1 Nginx VS Apache

相同点：

1、都是HTTP服务器软件

2、功能上都采用模块化结构设计

3、都支持通用的语言接口，如PHP、Perl、Python等

4、支持正向、反向代理、虚拟主机、URL重写、压缩传输、SSL加密传输

不同点：

1、Apache处理速度很慢，占用很多内存资源

2、功能上，Apache所有模块支持动静态编译，Nginx模块都是静态编译的

3、对Fcgi的支持：Apache支持的很不好，Nginx支持非常好

4、处理连接方式：Nginx支持epoll,Apache不支持

5、空间使用上：Nginx安装包仅几百K

### 3.2 总结

Nginx是一个高性能的HTTP和反向代理服务器，也是一个IMAP/POP3/SMTP服务器。
工作原理也很简单，通过转发请求，分担压力，从而减轻服务器的压力，达到负载均衡的效果。

