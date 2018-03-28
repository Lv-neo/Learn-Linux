## 概述
系统性能一直是一个受关注的话题，如何通过最简单的设置来实现最有效的性能调优，如何在有限资源的条件下保证程序的运作，ulimit 是我们在处理这些问题时，经常使用的一种简单手段。ulimit 是一种 linux 系统的内键功能，它具有一套参数集，用于为由它生成的 shell 进程及其子进程的资源使用设置限制。

##ulimit 的功能和用法
###ulimit 功能简述
假设有这样一种情况，当一台 Linux 主机上同时登陆了 10 个人，在系统资源无限制的情况下，这 10 个用户同时打开了 500 个文档，而假设每个文档的大小有 10M，这时系统的内存资源就会受到巨大的挑战。

而实际应用的环境要比这种假设复杂的多，例如在一个嵌入式开发环境中，各方面的资源都是非常紧缺的，对于开启文件描述符的数量，分配堆栈的大小，CPU 时间，虚拟内存大小，等等，都有非常严格的要求。资源的合理限制和分配，不仅仅是保证系统可用性的必要条件，也与系统上软件运行的性能有着密不可分的联系。这时，ulimit 可以起到很大的作用，它是一种简单并且有效的实现资源限制的方式。

ulimit 用于限制 shell 启动进程所占用的资源，支持以下各种类型的限制：所创建的内核文件的大小、进程数据块的大小、Shell 进程创建文件的大小、内存锁住的大小、常驻内存集的大小、打开文件描述符的数量、分配堆栈的最大大小、CPU 时间、单个用户的最大线程数、Shell 进程所能使用的最大虚拟内存。同时，它支持硬资源和软资源的限制。

作为临时限制，ulimit 可以作用于通过使用其命令登录的 shell 会话，在会话终止时便结束限制，并不影响于其他 shell 会话。而对于长期的固定限制，ulimit 命令语句又可以被添加到由登录 shell 读取的文件中，作用于特定的 shell 用户。


###ulimit 的使用
 

在下面的章节中，将详细介绍如何使用 ulimit 做相应的资源限制。

* 如何使用 ulimit
ulimit 通过一些参数选项来管理不同种类的系统资源。在本节，我们将讲解这些参数的使用。

```
ulimit 命令的格式为：ulimit [options] [limit]
```

具体的 options 含义以及简单示例可以参考以下表格。


表 1. ulimit 参数说明

选项 [options]
含义
例子

```
-H 设置硬资源限制，一旦设置不能增加。
ulimit – Hs 64；限制硬资源，线程栈大小为 64K。

-S 设置软资源限制，设置后可以增加，但是不能超过硬资源设置。
ulimit – Sn 32；限制软资源，32 个文件描述符。

-a 显示当前所有的 limit 信息。
ulimit – a；显示当前所有的 limit 信息。

-c 最大的 core 文件的大小， 以 blocks 为单位。
ulimit – c unlimited； 对生成的 core 文件的大小不进行限制。

-d 进程最大的数据段的大小，以 Kbytes 为单位。
ulimit -d unlimited；对进程的数据段大小不进行限制。

-f 进程可以创建文件的最大值，以 blocks 为单位。
ulimit – f 2048；限制进程可以创建的最大文件大小为 2048 blocks。

-l 最大可加锁内存大小，以 Kbytes 为单位。
ulimit – l 32；限制最大可加锁内存大小为 32 Kbytes。

-m 最大内存大小，以 Kbytes 为单位。
ulimit – m unlimited；对最大内存不进行限制。

-n 可以打开最大文件描述符的数量。
ulimit – n 128；限制最大可以使用 128 个文件描述符。

-p 管道缓冲区的大小，以 Kbytes 为单位。
ulimit – p 512；限制管道缓冲区的大小为 512 Kbytes。

-s 线程栈大小，以 Kbytes 为单位。
ulimit – s 512；限制线程栈的大小为 512 Kbytes。

-t 最大的 CPU 占用时间，以秒为单位。
ulimit – t unlimited；对最大的 CPU 占用时间不进行限制。

-u 用户最大可用的进程数。
ulimit – u 64；限制用户最多可以使用 64 个进程。

-v 进程最大可用的虚拟内存，以 Kbytes 为单位。
ulimit – v 200000；限制最大可用的虚拟内存为 200000 Kbytes。
```


	其中 "open files (-n) 1024 "是Linux操作系统对一个进程打开的文件句柄数量的限制(也包含打开的SOCKET数量，可影响MySQL的并发连接数目)。这个值可用ulimit命令来修改,但ulimit命令修改的数值只对当前登录用户的目前使用环境有效,系统重启或者用户退出后就会失效(在布署Nginx+FastCGI我就遇到这个问题，将ulimit -SHn 65535放到/etc/rc.d/rc.local也没起什么作用)

系统总限制是在这里，/proc/sys/fs/file-max。可以通过cat查看目前的值，修改/etc/sysctl.conf 中也可以控制。

另外还有一个，/proc/sys/fs/file-nr，可以看到整个系统目前使用的文件句柄数量。

查找文件句柄问题的时候，还有一个很实用的程序lsof。可以很方便看到某个进程开了那些句柄，也可以看到某个文件/目录被什么进程占用了。

修改方法
若要令修改ulimits的数值永久生效，则必须修改配置文档，可以给ulimit修改命令放入/etc/profile里面，这个方法实在是不方便，还有一个方法是修改/etc/sysctl.conf。我修改了，测试过，但对用户的ulimits -a 是不会改变的，只是/proc/sys/fs/file-max的值变了。

注意：这个当中的硬限制是实际的限制，而软限制，是warnning限制，只会做出warning；其实ulimit命令本身就有分软硬设置，加-H就是硬，加-S就是软
默认显示的是软限制，如果运行ulimit命令修改的时候没有加上的话，就是两个参数一起改变。

生效
因为我平时工作最多的是部署web环境(Nginx+FastCGI外网生产环境和内网开发环境)，重新登陆即可(reboot其实也行)我分别用root和www用户登陆，用ulimit -a分别查看确认，做这之前最好是重启下ssh服务，service sshd restart。

```
cat /etc/security/limits.conf
# /etc/security/limits.conf
#
#This file sets the resource limits for the users logged in via PAM.
#It does not affect resource limits of the system services.
#
#Also note that configuration files in /etc/security/limits.d directory,
#which are read in alphabetical order, override the settings in this
#file in case the domain is the same or more specific.
#That means for example that setting a limit for wildcard domain here
#can be overriden with a wildcard setting in a config file in the
#subdirectory, but a user specific setting here can be overriden only
#with a user specific setting in the subdirectory.
#
#Each line describes a limit for a user in the form:
#
#<domain>        <type>  <item>  <value>
#
#Where:
#<domain> can be:
#        - a user name
#        - a group name, with @group syntax
#        - the wildcard *, for default entry
#        - the wildcard %, can be also used with %group syntax,
#                 for maxlogin limit
#
#<type> can have the two values:
#        - "soft" for enforcing the soft limits
#        - "hard" for enforcing hard limits
#
#<item> can be one of the following:
#        - core - limits the core file size (KB)
#        - data - max data size (KB)
#        - fsize - maximum filesize (KB)
#        - memlock - max locked-in-memory address space (KB)
#        - nofile - max number of open files
#        - rss - max resident set size (KB)
#        - stack - max stack size (KB)
#        - cpu - max CPU time (MIN)
#        - nproc - max number of processes
#        - as - address space limit (KB)
#        - maxlogins - max number of logins for this user
#        - maxsyslogins - max number of logins on the system
#        - priority - the priority to run user process with
#        - locks - max number of file locks the user can hold
#        - sigpending - max number of pending signals
#        - msgqueue - max memory used by POSIX message queues (bytes)
#        - nice - max nice priority allowed to raise to values: [-20, 19]
#        - rtprio - max realtime priority
#
#<domain>      <type>  <item>         <value>
#

#*               soft    core            0
#*               hard    rss             10000
#@student        hard    nproc           20
#@faculty        soft    nproc           20
#@faculty        hard    nproc           50
#ftp             hard    nproc           0
#@student        -       maxlogins       4

# End of file
* soft nproc 65535
* hard nproc 65535
* soft nofile 65535
* hard nofile 65535
```

