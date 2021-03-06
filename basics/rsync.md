# rsync

#### 作者：高天阳
#### 邮箱：13683265113@163.com

```
更改历史

* 2018-08-30	    高天阳     初始化文档

```

## 1 简介和特点

### 1.1 简介

rsync是linux系统下的数据镜像备份工具。使用快速增量备份工具Remote Sync可以远程同步，
支持本地复制，或者与其他SSH、rsync主机同步。

### 1.2 特点

可以镜像保存整个目录树和文件系统。

可以很容易做到保持原来文件的权限、时间、软硬链接等等。

无须特殊权限即可安装。

快速：第一次同步时 rsync 会复制全部内容，但在下一次只传输修改过的文件。
rsync 在传输数据的过程中可以实行压缩及解压缩操作，因此可以使用更少的带宽。

安全：可以使用scp、ssh等方式来传输文件，当然也可以通过直接的socket连接。

支持匿名传输，以方便进行网站镜象。

## 2 安装和使用

### 2.1 安装

#### 2.1.1 Linux安装

Ubuntu安装: 

```
sudo apt-get install rsync
```
RedHat: 

```
yum install rsync
```

编译安装
rsync的编译安装非常简单，只需要以下简单的几步：

```
[root@www rsync-2.4.6]# ./configure
[root@www rsync-2.4.6]# make
[root@www rsync-2.4.6]# make install
```

但是需要注意的是必须在服务器A和B上都安装rsync，其中A服务器上是以服务器模式运行rsync，而B上则以客户端方式运行rsync。
这样在web服务器A上运行rsync守护进程，在B上定时运行客户程序来备份web服务器A上需要备份的内容。

#### 2.1.2 Windows安装

##### 2.1.2.1 ~~rsync的客户端官方版本(cwRsync)目前官方版本已收费~~

cwRsync是Windows 客户端GUI的一个包含Rsync的包装。您可以使用cwRsync快速远程文件备份和同步。

1 [官网下载地址](https://www.itefix.net/cwrsync)

下载方法：

1.1 点击面页中的get，获取Free（免费版本）

1.2 转跳后点击  Download cwRsync Free Edition! 进行下载

![](../../assets/rsyncDownload1.png)

![](../../assets/rsyncDownload2.png)

2 下载完成后得到一个cwRsync_5.5.0_x86_Free.zip的压缩包

包内容如下：

```
[root@backup backup]# tree cwRsync_5.5.0_x86_Free

cwRsync_5.5.0_x86_Free
├── bin
│   ├── cygcrypto-1.0.0.dll
│   ├── cyggcc_s-1.dll
│   ├── cygiconv-2.dll
│   ├── cygintl-8.dll
│   ├── cygpopt-0.dll
│   ├── cygssp-0.dll
│   ├── cygwin1.dll
│   ├── cygz.dll
│   ├── rsync.exe
│   ├── ssh.exe
│   └── ssh-keygen.exe
├── cwrsync.cmd
├── README.cwrsync.txt
└── README.rsync.txt
```

2 rsync的安装(cwRsync)

2.1 将压缩包解压出来

双击 `cwrsync[.cmd]`进行安装

![](../../assets/rsyncInstall1.png)

2.2 安装完成会有多一个home目录

该目录下存放的是ssh认证信息

![](../../assets/rsyncInstall2.png)

到此安装完成。

3 cwrsync的使用

3.1 在windows上打开cmd（命令提示符）

打开后，将cmd的路径切换到cwrysnc的安装目录的bin目录下，作为工作目录。

![](../../assets/rsyncUse.png)

3.2 window上的推送测试

```
C:\Users\Administrator\Desktop\cwRsync_5.5.0_x86_Free\bin>rsync.exe -avzP ./cwRs

ync_5.5.0_x86_Free.zip rsync_backup@172.16.1.41::backup --password-file=./rsync.

password

sending incremental file list

cwRsync_5.5.0_x86_Free.zip

      3,486,341 100%   21.11MB/s    0:00:00 (xfr#1, to-chk=0/1)

 

sent 3,475,491 bytes  received 34 bytes  2,317,016.67 bytes/sec

total size is 3,486,341  speedup is 1.00
```

在windows上rsync的命令与在linux上基本类似。

3.3 服务端上检查

```
[root@backup backup]# ll cwRsync_5.5.0_x86_Free.zip

-rwxrwx--- 1 rsync rsync 3486341 Oct 12 13:25 cwRsync_5.5.0_x86_Free.zip
```

至此windows上的rsync的客户端可以正常使用。

##### 2.1.2.2 rsync的客户端三方版本(cwRsync)

1 [第三方下载地址](http://dl.pconline.com.cn/download/402117.html)

2 下载完成后得到一个cwRsync的安装包

安装后包内容如下：

```
[root@backup backup]# tree cwRsync

cwRsync
├── bin
│   ├── chmod.exe
│   ├── cygcrypto-0.9.8.dll
│   ├── cyggcc_s-1.dll
│   ├── cygiconv-2.dll
│   ├── cygintl-3.dll
│   ├── cygintl-8.dll
│   ├── cygpopt-0.dll
│   ├── cygminires.dll
│   ├── cygpath.exe
│   ├── cygssp-0.dll
│   ├── cygwin1.dll
│   ├── cygz.dll
│   ├── plink.exe
│   ├── puttygen.exe
│   ├── rsync.exe
│   ├── sechaw.exe
│   ├── SecureChanne|Wizard.exe
│   ├── ssh.exe
│   └── ssh-keygen.exe
│   └── ssh-keyscan.exe
├── doc
│   ├── plink.html
│   ├── puttygen.html
│   ├── rsync.html
│   ├── sechaw.html
│   ├── ssh.html
│   └── ssh-keygen.html
├── cwrsync.cmd
├── license.txt
└── uninstall.exe
```

2 环境变量配置

右键我的电脑->属性->高级系统设置->环境变量->环境变量选择`PATH`系统变量选择`PATH`->编辑->在结尾添加`cwRsync`的`bin`目录

例如：

```
C:\Program Files (x86)\cwRsync\bin
```

* 注意：windows版本目前尚未找到方式使用`git bash`调用，且在`cmd`中无法调用`sh`命令。目前暂时只能通过`cmd`直接复制部署命令进行部署。

| \ | cmd | git bash |
|:---:|:----:|:----:|
| sh | X | √ |
| rsync | √ | X |
| expect | X | ? |

### 2.2 使用

#### 2.2.1 服务器端启动

```
usr/bin/rsync --daemon --config=/etc/rsyncd/rsyncd.conf
```

可能需要root权限运行.

`/etc/rsyncd/rsyncd.conf`是你刚才编辑的`rsyncd.conf`的位置.

也可以在`/etc/rc.d/rc.local`里加入让系统自动启动等.

#### 2.2.2 客户端同步

rsync -参数 用户名@同步服务器的IP::rsyncd.conf中那个方括号里的内容 本地存放路径 如:

rsync -avzP nemo@192.168.10.1::nemo /backup

说明：

- -a 参数，相当于-rlptgoD，-r 是递归 -l 是链接文件，意思是拷贝链接文件；-p 表示保持文件原有权限；-t 保持文件原有时间；-g 保持文件原有用户组；-o 保持文件原有属主；-D 相当于块设备文件；
- -z 传输时压缩；
- -P 传输进度；
- -v 传输时的进度等信息，和-P有点关系，自己试试。可以看文档；

#### 2.2.3 参数详解

-v, --verbose 详细模式输出

-q, --quiet 精简输出模式

-c, --checksum 打开校验开关，强制对文件传输进行校验

-a, --archive 归档模式，表示以递归方式传输文件，并保持所有文件属性，等于-rlptgoD

-r, --recursive 对子目录以递归模式处理

-R, --relative 使用相对路径信息

-b, --backup 创建备份，也就是对于目的已经存在有同样的文件名时，将老的文件重新命名为~filename。可以使用--suffix选项来指定不同的备份文件前缀。

--backup-dir 将备份文件(如~filename)存放在在目录下。

-suffix=SUFFIX 定义备份文件前缀

-u, --update 仅仅进行更新，也就是跳过所有已经存在于DST，并且文件时间晚于要备份的文件。(不覆盖更新的文件)

-l, --links 保留软链结

-L, --copy-links 像对待常规文件一样处理软链接

--copy-unsafe-links 仅仅拷贝指向SRC路径目录树以外的链接

--safe-links 忽略指向SRC路径目录树以外的链接

-H, --hard-links 保留硬链接

-p, --perms 保持文件权限

-o, --owner 保持文件属主信息

-g, --group 保持文件属组信息

-D, --devices 保持设备文件信息

-t, --times 保持文件时间信息

-S, --sparse 对稀疏文件进行特殊处理以节省DST的空间

-n, --dry-run显示哪些文件将被传输

-W, --whole-file 拷贝文件，不进行增量检测

-x, --one-file-system 不要跨越文件系统边界

-B, --block-size=SIZE 检验算法使用的块尺寸，默认是700字节

-e, --rsh=COMMAND 指定使用rsh、ssh方式进行数据同步

--rsync-path=PATH 指定远程服务器上的rsync命令所在路径信息

-C, --cvs-exclude 使用和CVS一样的方法自动忽略文件，用来排除那些不希望传输的文件

--existing 仅仅更新那些已经存在于DST的文件，而不备份那些新创建的文件

--delete 删除那些DST中SRC没有的文件

--delete-excluded 同样删除接收端那些被该选项指定排除的文件

--delete-after 传输结束以后再删除

--ignore-errors 即使出现IO错误也进行删除

--max-delete=NUM 最多删除NUM个文件

--partial 保留那些因故没有完全传输的文件，以是加快随后的再次传输

--force 强制删除目录，即使不为空

--numeric-ids 不将数字的用户和组ID匹配为用户名和组名

--timeout=TIME IP超时时间，单位为秒

-I, --ignore-times 不跳过那些有同样的时间和长度的文件

--size-only 当决定是否要备份文件时，仅仅察看文件大小而不考虑文件时间

--modify-window=NUM 决定文件是否时间相同时使用的时间戳窗口，默认为0

-T --temp-dir=DIR 在DIR中创建临时文件

--compare-dest=DIR 同样比较DIR中的文件来决定是否需要备份

-P 等同于 --partial

--progress 显示备份过程

-z, --compress 对备份的文件在传输时进行压缩处理

--exclude=PATTERN 指定排除不需要传输的文件模式

--include=PATTERN 指定不排除而需要传输的文件模式

--exclude-from=FILE 排除FILE中指定模式的文件

--include-from=FILE 不排除FILE指定模式匹配的文件

--version 打印版本信息

--address 绑定到特定的地址

--config=FILE 指定其他的配置文件，不使用默认的rsyncd.conf文件

--port=PORT 指定其他的rsync服务端口

--blocking-io 对远程shell使用阻塞IO

-stats 给出某些文件的传输状态

--progress 在传输时现实传输过程

--log-format=formAT 指定日志文件格式

--password-file=FILE 从FILE中得到密码

--bwlimit=KBPS 限制I/O带宽，KBytes per second

-h, --help 显示帮助信息

### 2.3 示例

#### 2.3.1 SSH方式

首先在服务端启动ssh服务：

```
service sshd start
启动 sshd： [确定]
```

#### 2.3.2 使用rsync进行同步

接下来就可以在客户端使用rsync命令来备份服务端上的数据了，SSH方式是通过系统用户来进行备份的，如下：

```
rsync -vzrtopg --progress -e ssh --delete work@172.16.78.192:/www/* /databack/experiment/rsync
work@172.16.78.192's password:
receiving file list ...
5 files to consider
test/
a
0 100% 0.00kB/s 527:35:41 (1, 20.0% of 5)
b
67 100% 65.43kB/s 0:00:00 (2, 40.0% of 5)
c
0 100% 0.00kB/s 527:35:41 (3, 60.0% of 5)
dd
100663296 100% 42.22MB/s 0:00:02 (4, 80.0% of 5)
sent 96 bytes received 98190 bytes 11563.06 bytes/sec
total size is 100663363 speedup is 1024.19
```

上面的信息描述了整个的备份过程，以及总共备份数据的大小。

#### 2.3.3 后台服务方式

启动rsync服务，编辑`/etc/xinetd.d/rsync`文件，将其中的`disable=yes`改为`disable=no`，并重启xinetd服务，如下：

```
vi /etc/xinetd.d/rsync

#default: off
# description: The rsync server is a good addition to an ftp server, as it \
# allows crc checksumming etc.
service rsync {
    disable = no
    socket_type = stream
    wait = no
    user = root
    server = /usr/bin/rsync
    server_args = --daemon
    log_on_failure += USERID
}
```

```
/etc/init.d/xinetd restart
停止 xinetd： [确定]
启动 xinetd： [确定]
```

创建配置文件，默认安装好rsync程序后，并不会自动创建rsync的主配置文件，需要手工来创建，
其主配置文件为“/etc/rsyncd.conf”，创建该文件并插入如下内容：

```
vi /etc/rsyncd.conf

uid=root
gid=root
max connections=4
log file=/var/log/rsyncd.log
pid file=/var/run/rsyncd.pid
lock file=/var/run/rsyncd.lock
secrets file=/etc/rsyncd.passwd
hosts deny=172.16.78.0/22

[www]
comment= backup web
path=/www
read only = no
exclude=test
auth users=work
```

创建密码文件，采用这种方式不能使用系统用户对客户端进行认证，所以需要创建一个密码文件，其格式为“username:password”，
用户名可以和密码可以随便定义，最好不要和系统帐户一致，同时要把创建的密码文件权限设置为600，这在前面的模块参数做了详细介绍。

```
echo "work:abc123" > /etc/rsyncd.passwd
chmod 600 /etc/rsyncd.passwd
```

备份，完成以上工作，现在就可以对数据进行备份了，如下：

```
rsync -avz --progress --delete work@172.16.78.192::www /databack/experiment/rsync

Password:
receiving file list ...
6 files to consider
./ files...
a
0 100% 0.00kB/s 528:20:41 (1, 50.0% of 6)
b
67 100% 65.43kB/s 0:00:00 (2, 66.7% of 6)
c
0 100% 0.00kB/s 528:20:41 (3, 83.3% of 6)
dd
100663296 100% 37.49MB/s 0:00:02 (4, 100.0% of 6)
sent 172 bytes received 98276 bytes 17899.64 bytes/sec
total size is 150995011 speedup is 1533.75
```

恢复，当服务器的数据出现问题时，那么这时就需要通过客户端的数据对服务端进行恢复，
但前提是服务端允许客户端有写入权限，否则也不能在客户端直接对服务端进行恢复，使用rsync对数据进行恢复的方法如下：

```
rsync -avz --progress /databack/experiment/rsync/ work@172.16.78.192::www

Password:
building file list ...
6 files to consider
./
a
b
67 100% 0.00kB/s 0:00:00 (2, 66.7% of 6)
c
sent 258 bytes received 76 bytes 95.43 bytes/sec
total size is 150995011 speedup is 452080.87
```

### 2.4 最佳实践

#### 2.4.1 部署后属主、属组变化

> 方案一(失败)

```
rsync -avz --delete page/ root@172.16.78.192:www/page
```

本地传输文件至服务器后，属主、属组发生了变化，需要登录服务器操作更改文件夹的属主、属组。
在查看rsync文档中了解到有`-o`（保持文件属主信息）、`-g`（保持文件属组信息）两个参数。

```
rsync -avzog --delete page/ root@172.16.78.192:www/page
```

尝试后并未生效

[rsync命令介绍](https://blog.csdn.net/u010339879/article/details/54987880)

> 方案二(失败)

使用ftp用户上传

```
rsync -avz --delete page/ ftp@172.16.78.192:www/page
```

ftp账号无登录权限或不知道ftp账号密码

> 方案三(成功)

将机械性修改操作写在脚本中执行

[使用脚本远程登录服务器并执行操作](https://www.jianshu.com/p/d4c1ac10204d?utm_campaign)

```
# deploy.sh
rsync -avz --delete page/ root@172.16.78.192:www/page
expect chown.exp
```

```
# chown.exp
# 修改用户组权限 chown -R www:ftp page
spawn ssh root@172.16.78.192 "
cd www;
chown -R www:ftp page"
expect eof
```

成功

#### 2.4.2 同步服务器文件至本地

同步服务器www/page下的文件内容至本地page文件夹下

```bash
rsync -avz root@172.16.78.192:www/page page
```

## 3 同类型技术比较

> Rsync、Unison及DRBD的比较

| \ | Rsync | Unison | DRBD |
|:---|:----|:----|:----|
| 简介 | 远程数据同步工具，可通过LAN/WAN快速同步多台主机间的文件。 目前由rsync.samba.org维护 | Unison是windows和unix平台下都可以使用的双向文件同步工具，它能使两个文件夹（本地或网络 上的）保持内容的一致。目前已停止开发。 | DRBD，是分布式复制块设备，是一种通过TCP/IP网络实现块设备数据实时镜像的方案。,可以被用于高可用(HA)之中.它类似于一个网络RAID-1功能. |
| 功能及特点 | 1.镜像保存整个目录树和文件系统；<br>2.保持原来文件的权限、时间、软硬链接等；<br>3.无须特殊权限即可安装；<br>4.优化的流程，文件传输效率高；使用“Rsync算法”同步两个文件的不同部分，而不是每次都整份传送；<br>5.可以使用rsh、ssh等方式来传输文件，也可以通过直接的socket连接；<br>6.支持匿名传输。 | 1、跨平台使用；<br>2、对内核和用户权限没有特别要求；<br>3、支持双向同步，能自动处理两份拷贝中更新没有冲突的部分，有冲突的部分将会显示出来让用户选择更新策略；<br>4、可以直接使用`socket`连接或安全的`ssh`连接方式，对带宽的要求不高，使用类似`rsync`的压缩传输协议。 | 1、DRBD是一种块设备<br>2、可用于高可用集群（HA）<br>3、通过网络实现块设备数据实时镜像<br>4、类似于一个网络RAID-1 |
| 使用平台 | 类unix平台(Linux、Solaris、BSD) Windows平台相应版本：cwRsync | Windows平台 Unix/Linux平台 | Linux平台 |
| 依赖性 | 无 | 无 | 需要内核支持 |
| 传输方式 | rsh、ssh、socket连接、daemon模式 | socket 连接或安全的 ssh 连接方式 | 通过TCP/IP网络实现块设备数据实时镜像 |
| 稳定性 | 高 | 一般（超时严重） | 高 |
| 同步速度 | 快 | 小文件同步速度：快<br>大文件同步速度：一般 | 小文件同步速度：快<br>大文件同步速度：慢 |
| 缺点 | 不支持双向传输 | 超时严重 | 不支持双向传输，传输速度慢 |

* 总结：

    * Rsync、Unison和DRBD三个软件，虽然都是同步备份数据的工具，但是各有其功能。
    * Rsync只支持单向同步，但是其同步速度是三者之中最快的，正确的说其同步速度是其他两者所无法比拟的。同时，rsync+inotify的搭配使用可以达到实时触控同步更新文件。
    * Unison支持双向同步，对内核和用户权限没有要求，但是其同步速度特别慢，经常出现超时情况。
    * DRBD用于高可用（HA），类似于一个网络RAID－1功能，速度也比较慢。
    * 通过安装配置rsync、unison和drbd三个软件，同时测试其功能，发现三者各有其优势。不能单纯的评价谁优谁劣，应该针对用户的用途进行选择使用工具。

## 参考资料

* [rsync百度百科](https://baike.baidu.com/item/rsync/80863389)
* [rsync(在window端使用常见错误)](http://blog.51cto.com/cold2076goddess/1634825)
* [Rsync、Unison及DRBD的比较](http://qq85609655.iteye.com/blog/1879416)