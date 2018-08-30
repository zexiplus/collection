# linux

> 关于linux 系统 的基本概念和操作总结



### 文件结构

| 目录                 | 描述                                                         |
| -------------------- | ------------------------------------------------------------ |
| `/`                  | *主层次* 的根，也是整个文件系统层次结构的根目录              |
| `/bin`               | 存放在单用户模式可用的必要命令二进制文件，所有用户都可用，如 cat、ls、cp等等 |
| `/boot`              | 存放引导加载程序文件，例如kernels、initrd等                  |
| `/dev`               | 存放必要的设备文件，例如`/dev/null`                          |
| `/etc`               | 存放主机特定的系统级配置文件。                               |
| `/etc/opt`           | 存储着新增包的配置文件 `/opt/`.                              |
| `/etc/sgml`          | 存放配置文件，比如 catalogs，用于那些处理SGML(译者注：标准通用标记语言)的软件的配置文件 |
| `/etc/X11`           | X Window 系统11版本的的配置文件                              |
| `/etc/xml`           | 配置文件，比如catalogs，用于那些处理XML(译者注：可扩展标记语言)的软件的配置文件 |
| `/home`              | 用户的主目录，包括保存的文件，个人配置，等等                 |
| `/lib`               | `/bin/` 和 `/sbin/`中的二进制文件的必需的库文件              |
| `/lib<架构位数>`     | 备用格式的必要的库文件。 这样的目录是可选的，但如果他们存在的话肯定是有需要用到它们的程序 |
| `/media`             | 可移动的多媒体(如CD-ROMs)的挂载点。(出现于 FHS-2.3)          |
| `/mnt`               | 临时挂载的文件系统                                           |
| `/opt`               | 可选的应用程序软件包                                         |
| `/proc`              | 以文件形式提供进程以及内核信息的虚拟文件系统，在Linux中，对应进程文件系统（procfs ）的挂载点 |
| `/root`              | 根用户的主目录                                               |
| `/sbin`              | 必要的系统级二进制文件，比如， init, ip, mount               |
| `/srv`               | 系统提供的站点特定数据                                       |
| `/tmp`               | 临时文件 (另见 `/var/tmp`). 通常在系统重启后删除             |
| `/usr`               | *二级层级*存储用户的只读数据； 包含(多)用户主要的公共文件以及应用程序 |
| `/usr/bin`           | 非必要的命令二进制文件 (在单用户模式中不需要用到的)；用于所有用户 |
| `/usr/include`       | 标准的包含文件                                               |
| `/usr/lib`           | 库文件，用于`/usr/bin/` 和 `/usr/sbin/`中的二进制文件        |
| `/usr/lib<架构位数>` | 备用格式库(可选的)                                           |
| `/usr/local`         | *三级层次* 用于本地数据，具体到该主机上的。通常会有下一个子目录, *比如*, `bin/`, `lib/`, `share/`. |
| `/usr/local/sbin`    | 非必要系统的二进制文件，比如用于不同网络服务的守护进程       |
| `/usr/share`         | 架构无关的 (共享) 数据.                                      |
| `/usr/src`           | 源代码，比如内核源文件以及与它相关的头文件                   |
| `/usr/X11R6`         | X Window系统，版本号:11，发行版本：6                         |
| `/var`               | 各式各样的（Variable）文件，一些随着系统常规操作而持续改变的文件就放在这里，比如日志文件，脱机文件，还有临时的电子邮件文件 |
| `/var/cache`         | 应用程序缓存数据. 这些数据是由耗时的I/O(输入/输出)的或者是运算本地生成的结果。这些应用程序是可以重新生成或者恢复数据的。当没有数据丢失的时候，可以删除缓存文件 |
| `/var/lib`           | 状态信息。这些信息随着程序的运行而不停地改变，比如，数据库，软件包系统的元数据等等 |
| `/var/lock`          | 锁文件。这些文件用于跟踪正在使用的资源                       |
| `/var/log`           | 日志文件。包含各种日志。                                     |
| `/var/mail`          | 内含用户邮箱的相关文件                                       |
| `/var/opt`           | 来自附加包的各种数据都会存储在 `/var/opt/`.                  |
| `/var/run`           | 存放当前系统上次启动以来的相关信息，例如当前登入的用户以及当前运行的[daemons(守护进程)](http://en.wikipedia.org/wiki/Daemon_%28computing%29). |
| `/var/spool`         | 该spool主要用于存放将要被处理的任务，比如打印队列以及邮件外发队列 |
| `/var/mail`          | 过时的位置，用于放置用户邮箱文件                             |
| `/var/tmp`           | 存放重启后保留的临时文件                                     |



### 操作

##### ssh连接服务器

```shell
# 登录
ssh username@ipAddress

# 登出
logout

# 重启
sudo reboot
sudo init 6

# 关机
sudo power off
sudo init 0

# 编辑ssh配置文件
vim /etc/ssh/sshd_config

# 设置 sshd_config 文件的客户端链接时长,心跳
# 增加下列字段
# 1、客户端每隔多少秒向服务发送一个心跳数据
# 2、客户端多少秒没有相应，服务器自动断掉连接

ClientAliveInterval 30
ClientAliveCountMax 86400

# 重启sshd服务
service sshd reload

```



##### scp从服务器上传/下载文件

> ftp默认端口 22

```shell
# 从服务器向本地下载文件
scp username@ip:/remotePath/folterName/fileName /localPath/folderName

# 从本地向服务器上传文件
scp /localPath/folderName/fileName username@ip:/remotePath/folderName

# 从服务器下载目录
scp -r username@ip:/remotePath/folderName /localPath/

# 将整个目录同步到服务器
scp -r /localPath/folderName username@ip:/remotePath/folderName

```



##### 注册开机启动程序

> 使用 upstart 或者 systemd , 取决于你的系统使用的哪种服务管理

* **systemd**

  * **命令行工具 systemctl**

    ```bash
    # 查看systemctl 版本
    systemctl --version
    
    # 增加配置文件
    touch /etc/systemd/system/nodeserver.service
    ```

  * **Create the service file**

    > nodeserver.service

    ```shell
    [Unit]
    Description=Node.js Example Server
    #Requires=After=mysql.service       # Requires the mysql service to run first
    
    [Service]
    ExecStart=/usr/bin/node /opt/nodeserver/server.js
    # Required on some systems
    #WorkingDirectory=/opt/nodeserver
    Restart=always
    # Restart service after 10 seconds if node service crashes
    RestartSec=10
    # Output to syslog
    StandardOutput=syslog
    StandardError=syslog
    SyslogIdentifier=nodejs-example
    #User=<alternate user>
    #Group=<alternate group>
    Environment=NODE_ENV=production PORT=1337
    
    [Install]
    WantedBy=multi-user.target
    ```

  * **enable the service**

    ```bash
    systemctl enable nodeserver.service
    ```

  * **start the service**

    ```bash
    systemctl start nodeserver.service
    ```

  * **verify it's running**

    ```bash
    systemctl status nodeserver.service
    ```


* **upstart**

  ```shell
  
  ```




##### 软件源配置文件

```shell
# 文件名
/etc/apt/sources.list

# 文件内容 code 为系统版本代号 ubuntu 14 为 trusty， ubuntu 16 为 xenial ,ubuntu 17 为 artful ， ubuntu 18为 bionic
deb http://nginx.org/packages/debian/ codename nginx
deb-src http://nginx.org/packages/debian/ codename nginx
```



##### 查找文件

```shell
# 在etc文件夹下查找名为 nginx.conf 的文件
find /etc -name nginx.conf

# 在根目录/下查找名为usr的文件夹
find / -name usr -type d
```



##### grep 文本搜索

```shell
grep /regExp/ fileName [options]

#### [options] ###
#
# －c：只输出匹配行的计数。
# －I：不区分大 小写(只适用于单字符)。
# －h：查询多文件时不显示文件名。
# －l：查询多文件时只输出包含匹配字符的文件名。
# －n：显示匹配行及 行号。
# －s：不显示不存在或无匹配文本的错误信息。
# －v：显示不包含匹配文本的所有行。
## *********** ###

# demo: 

# 显示所有以d开头的文件中包含 test的行
grep ‘test’ d*

# 显示在aa，bb，cc文件中匹配test的行。
$ grep ‘test’ aa bb cc

# 显示所有包含每个字符串至少有5个连续小写字符的字符串的行。
$ grep ‘[a-z]\{5\}’ aa


```



##### 测量网络服务商的拓扑结构和速度

> tarceroute 可以列出分组经过的路由节点, 计算并返回每一跳的延时

```bash
traceroute bing.com
```



##### 改变文件/文件夹所有者

```shell
chown -R pi:pi Software    #把software文件夹拥有者改为pi
```



##### 显示当前路径

```shell
pwd
```



##### 创建软连接

```shell
ln -s /usr/node/bin/node /usr/local/bin/node
```



##### 显示命令细节指示

```shell
man ls, man sudo
```



##### 新建并执行shell文件

```shell
touch demo.sh
chmod +x demo.sh
./demo.sh           #执行
```



##### 新建文件并写入内容

```shell
echo "aaaa" > foo.txt
```



##### 按cpu用量查看进程

```shell
top -o cpu

# 杀死进程 2200 为进程id
sudo kill -9 2200
```



##### 按端口占用查看进程

 ```bash
 # 查看3000端口对应的进程
 lsof -i :3000
 
 kill -9 pid
 ```



### 其他软件配置

##### nginx

**nginx默认静态html目录**

> /usr/share/nginx/html

##### 配置文件

> / etc/nginx/nginx.conf

```shell
#运行用户
user nobody;
#启动进程,通常设置成和cpu的数量相等
worker_processes  1;

#全局错误日志及PID文件
#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;

#工作模式及连接数上限
events {
    #epoll是多路复用IO(I/O Multiplexing)中的一种方式,
    #仅用于linux2.6以上内核,可以大大提高nginx的性能
    use   epoll; 

    #单个后台worker process进程的最大并发链接数    
    worker_connections  1024;

    # 并发总数是 worker_processes 和 worker_connections 的乘积
    # 即 max_clients = worker_processes * worker_connections
    # 在设置了反向代理的情况下，max_clients = worker_processes * worker_connections / 4  为什么
    # 为什么上面反向代理要除以4，应该说是一个经验值
    # 根据以上条件，正常情况下的Nginx Server可以应付的最大连接数为：4 * 8000 = 32000
    # worker_connections 值的设置跟物理内存大小有关
    # 因为并发受IO约束，max_clients的值须小于系统可以打开的最大文件数
    # 而系统可以打开的最大文件数和内存大小成正比，一般1GB内存的机器上可以打开的文件数大约是10万左右
    # 我们来看看360M内存的VPS可以打开的文件句柄数是多少：
    # $ cat /proc/sys/fs/file-max
    # 输出 34336
    # 32000 < 34336，即并发连接总数小于系统可以打开的文件句柄总数，这样就在操作系统可以承受的范围之内
    # 所以，worker_connections 的值需根据 worker_processes 进程数目和系统可以打开的最大文件总数进行适当地进行设置
    # 使得并发总数小于操作系统可以打开的最大文件数目
    # 其实质也就是根据主机的物理CPU和内存进行配置
    # 当然，理论上的并发总数可能会和实际有所偏差，因为主机还有其他的工作进程需要消耗系统资源。
    # ulimit -SHn 65535

}


http {
    #设定mime类型,类型由mime.type文件定义
    include    mime.types;
    default_type  application/octet-stream;
    #设定日志格式
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  logs/access.log  main;

    #sendfile 指令指定 nginx 是否调用 sendfile 函数（zero copy 方式）来输出文件，
    #对于普通应用，必须设为 on,
    #如果用来进行下载等应用磁盘IO重负载应用，可设置为 off，
    #以平衡磁盘与网络I/O处理速度，降低系统的uptime.
    sendfile     on;
    #tcp_nopush     on;

    #连接超时时间
    #keepalive_timeout  0;
    keepalive_timeout  65;
    tcp_nodelay     on;

    #开启gzip压缩
    gzip  on;
    gzip_disable "MSIE [1-6].";

    #设定请求缓冲
    client_header_buffer_size    128k;
    large_client_header_buffers  4 128k;


    #设定虚拟主机配置
    server {
        #侦听80端口
        listen    80;
        #定义使用 www.nginx.cn访问
        server_name  www.nginx.cn;

        #定义服务器的默认网站根目录位置
        root html;

        #设定本虚拟主机的访问日志
        access_log  logs/nginx.access.log  main;

        #默认请求
        location / {
            
            #定义首页索引文件的名称
            index index.php index.html index.htm;   

        }

        # 定义错误提示页面
        error_page   500 502 503 504 /50x.html;
        location = /50x.html {
        }

        #静态文件，nginx自己处理
        location ~ ^/(images|javascript|js|css|flash|media|static)/ {
            
            #过期30天，静态文件不怎么更新，过期可以设大一点，
            #如果频繁更新，则可以设置得小一点。
            expires 30d;
        }

        #PHP 脚本请求全部转发到 FastCGI处理. 使用FastCGI默认配置.
        location ~ .php$ {
            fastcgi_pass 127.0.0.1:9000;
            fastcgi_index index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include fastcgi_params;
        }

        #禁止访问 .htxxx 文件
            location ~ /.ht {
            deny all;
        }

    }
}
```



##### vim

>  强行退出不保存修改 ctrl + c =>  :q!



##### 解压文件

```shell
.tar 
解包：tar xvf FileName.tar
打包：tar cvf FileName.tar DirName
（注：tar是打包，不是压缩！）
———————————————
.gz
解压1：gunzip FileName.gz
解压2：gzip -d FileName.gz
压缩：gzip FileName

.tar.gz 和 .tgz
解压：tar zxvf FileName.tar.gz
压缩：tar zcvf FileName.tar.gz DirName
———————————————

.tar.xz
解压：tar -Jxvf FileName.tar.xz
压缩：tar -Jcvf FileName.tar.xz DirName
———————————————

.bz2
解压1：bzip2 -d FileName.bz2
解压2：bunzip2 FileName.bz2
压缩： bzip2 -z FileName
———————————————

.tar.bz2
解压：tar jxvf FileName.tar.bz2
压缩：tar jcvf FileName.tar.bz2 DirName
———————————————
.bz
解压1：bzip2 -d FileName.bz
解压2：bunzip2 FileName.bz
压缩：未知
———————————————

.tar.bz
解压：tar jxvf FileName.tar.bz
压缩：未知
———————————————
.Z
解压：uncompress FileName.Z
压缩：compress FileName
.tar.Z
———————————————

解压：tar Zxvf FileName.tar.Z
压缩：tar Zcvf FileName.tar.Z DirName
———————————————
.zip
解压：unzip FileName.zip
压缩：zip FileName.zip DirName
———————————————
.rar
解压：rar x FileName.rar
压缩：rar a FileName.rar DirName
———————————————
.lha
解压：lha -e FileName.lha
压缩：lha -a FileName.lha FileName
———————————————
.rpm
解包：rpm2cpio FileName.rpm | cpio -div
———————————————
.deb
解包：ar p FileName.deb data.tar.gz | tar zxf -
  

```

