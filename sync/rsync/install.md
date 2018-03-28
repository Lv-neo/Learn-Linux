#安装教程

###检查rsync 是否已经安装

```
rpm -qa|grep rsync
```

若已经安装，则使用rpm -e 命令卸载。

```
yum install rsync
```

###配置 rsync 服务
* 首先要选择服务器启动方式
	* 对于负荷较重的 rsync 服务器应该使用独立运行方式
	* 对于负荷较轻的 rsync 服务器可以使用 xinetd 运行方式
* 创建配置文件 rsyncd.conf
* 对于非匿名访问的 rsync 服务器还要创建认证口令文件

####以 xinetd 运行 rsync 服务

CentOS 默认以 xinetd 方式运行 rsync 服务。rsync 的 xinetd 配置文件
在 /etc/xinetd.d/rsync。要配置以 xinetd 运行的 rsync 服务需要执行如下的命令：

```
chkconfig rsync on
service xinetd restart
```

管理员可以修改 /etc/xinetd.d/rsync 配置文件以适合您的需要。例如，您可以修改配置行

```
server_args = --daemon
```
在后面添加 rsync 的服务选项。

####独立运行 rsync 服务
最简单的独立运行 rsync 服务的方法是执行如下的命令：
# /usr/bin/rsync --daemon

您可以将上面的命令写入 /etc/rc.local 文件以便在每次启动服务器时运行 rsync 服务。当然，您也可以写一个脚本在开机时自动启动 rysnc 服务。

####配置文件 rsyncd.conf
两种 rsync 服务运行方式都需要配置 rsyncd.conf，其格式类似于 samba 的主配置文件。
配置文件 rsyncd.conf 默认在 /etc 目录下。为了将所有与 rsync 服务相关的文件放在单独的目录下，可以执行如下命令：


```
###yum 安装跳过本步骤
mkdir /etc/rsyncd
touch /etc/rsyncd/rsyncd.conf
ln -s /etc/rsyncd/rsyncd.conf /etc/rsyncd.conf
```

配置文件 rsyncd.conf 由全局配置和若干模块配置组成。配置文件的语法为：

* 模块以 [模块名] 开始
* 参数配置行的格式是 name = value ，其中 value 可以有两种数据类型：
	* 字符串（可以不用引号定界字符串）
	*	布尔值（1/0 或 yes/no 或 true/false）
* 以 # 或 ; 开始的行为注释
* \ 为续行符

####全局参数

在文件中 [module] 之外的所有配置行都是全局参数。当然也可以在全局参数部分定义模块参数，这时该参数的值就是所有模块的默认值。

| 参数 |	说明	| 默认值 |
| ------------- |:-------------:| -----:|
|address	 |在独立运行时，用于指定的服务器运行的 IP 地址。由 xinetd 运行时将忽略此参数，使用命令行上的 –address 选项替代。|	本地所有IP|
|port|	指定 rsync 守护进程监听的端口号。 由 xinetd 运行时将忽略此参数，使用命令行上的–port 选项替代。	|873|
|motd file|	指定一个消息文件，当客户连接服务器时该文件的内容显示给客户。|	无|
|pid file|	rsync 的守护进程将其 PID 写入指定的文件。|	无|
|log file|	指定 rsync 守护进程的日志文件，而不将日志发送给 syslog。	|无|
|syslog facility|	指定 rsync 发送日志消息给 syslog 时的消息级别。	|daemon|
|socket options	|指定自定义 TCP 选项。	|无|

####模块参数
模块参数主要用于定义 rsync 服务器哪个目录要被同步。模块声明的格式必须为 [module] 形式，这个名字就是在 rsync 客户端看到的名字，类似于 Samba 服务器提供的共享名。而服务器真正同步的数据是通过 path 来指定的。可以根据自己的需要，来指定多个模块，模块中可以定义以下参数：

* 基本模块参数

|参数	|说明	|默认值|
| ------------- |:-------------| -----:|
|path	|指定当前模块在 rsync 服务器上的同步路径，该参数是必须指定的。	|无|
|comment|给模块指定一个描述，该描述连同模块名在客户连接得到模块列表时显示给客户。|	无|


* 模块控制参数

|参数	|说明	|默认值|
| ------------- |:-------------| -----:|
|use chroot	|若为 true，则 rsync 在传输文件之前首先 chroot 到 path 参数所指定的目录下。这样做的原因是实现额外的安全防护，但是缺点是需要 root 权限，并且不能备份指向 path 外部的符号连接所指向的目录文件。	|true|
|uid	|指定该模块以指定的 UID 传输文件。|	nobody|
|gid	|指定该模块以指定的 GID 传输文件。	|nobody|
|max connections|	指定该模块的最大并发连接数量以保护服务器，超过限制的连接请求将被告知随后再试。	|0（没有限制）|
|lock file	|指定支持 max connections 参数的锁文件。	|/var/run/rsyncd.lock|
|list	|指定当客户请求列出可以使用的模块列表时，该模块是否应该被列出。如果设置该选项为 false，可以创建隐藏的模块。|	true|
|read only	|指定是否允许客户上传文件。若为 true 则不允许上传；若为 false 并且服务器目录也具有读写权限则允许上传。	|true|
|write only	|指定是否允许客户下载文件。若为 true 则不允许下载；若为 false 并且服务器目录也具有读权限则允许下载。	|false|
|ignore errors	|指定在 rsync 服务器上运行 delete 操作时是否忽略 I/O 错误。一般来说 rsync 在出现 I/O 错误时将将跳过 –delete 操作，以防止因为暂时的资源不足或其它 I/O 错误导致的严重问题。	|true|
|ignore nonreadable	|指定 rysnc 服务器完全忽略那些用户没有访问权限的文件。这对于在需要备份的目录中有些不应该被备份者获得的文件时是有意义的。|	false|
|timeout	|该选项可以覆盖客户指定的 IP 超时时间。从而确保 rsync 服务器不会永远等待一个崩溃的客户端。对于匿名 rsync 服务器来说，理想的数字是 600（单位为秒）。	|0 (未限制)|
|dont compress	|用来指定那些在传输之前不进行压缩处理的文件。该选项可以定义一些不允许客户对该模块使用的命令选项列表。必须使用选项全名，而不能是简称。当发生拒绝某个选项的情况时，服务器将报告错误信息然后退出。例如，要防止使用压缩，应该是：”dont compress = *”。|	*.gz *.tgz *.zip *.z *.rpm *.deb *.iso *.bz2 *.tbz|

