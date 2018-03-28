# sudo
sudo是linux系统管理指令，是允许系统管理员让普通用户执行一些或者全部的root命令的一个工具，如halt，reboot，su等等。这样不仅减少了root用户的登录 和管理时间，同样也提高了安全性。sudo不是对shell的一个代替，它是面向每个命令的。

通过sudo，我们能把某些超级权限有针对性的下放，并且不需要普通用户知道root密码，所以sudo 相对于权限无限制性的su来说，还是比较安全的，所以sudo 也能被称为受限制的su ；另外sudo 是需要授权许可的，所以也被称为授权许可的su；

###特性
* sudo能够限制用户只在某台主机上运行某些命令。
* sudo提供了丰富的日志，详细地记录了每个用户干了什么。它能够将日志传到中心主机或者日志服务器。
* sudo使用时间戳文件来执行类似的“检票”系统。当用户调用sudo并且输入它的密码时，用户获得了一张存活期为5分钟的票（这个值可以在编译的时候改变）。
* sudo的配置文件是sudoers文件，它允许系统管理员集中的管理用户的使用权限和使用的主机。它所存放的位置默认是在/etc/sudoers，属性必须为0440。


###安装
检查是否安装了sudo

```
[root@local ~]# rpm -q sudo
sudo-1.8.6p7-16.el7.x86_64
```

###配置

编辑配置文件命令:visudo

>注意：编辑sudo的配置文件/etc/sudoers是一般不要直接使用vi（vi /etc/sudoers）去编辑，因为sudoers配置有一定的语法，直接用vi编辑保存系统不会检查语法，如有错也保存了可能导致无法使用sudo工具，最好使用visudo命令去配置。虽然visudo也是调用vi去编辑，但是保存时会进行语法检查，有错会有提示。

默认配置文件位置:/etc/sudoers


####第一部分：用户定义，将用户分为FULLTIMERS、PARTTIMERS和WEBMASTERS三类。
User_Alias FULLTIMERS = millert, mikef, dowdy
User_Alias PARTTIMERS = bostley, jwfox, crawl
User_Alias WEBMASTERS = will, wendy, wim
####第二部分，将操作类型分类。
Runas_Alias OP = root, operator
Runas_Alias DB = oracle, sybase
####第三部分，将主机分类。这些都是随便分得，目的是为了更好地管理。
Host_Alias SPARC = bigtime, eclipse, moet, anchor :\
SGI = grolsch, dandelion, black :\
ALPHA = widget, thalamus, foobar :\
HPPA= boa, nag, python
Host_Alias CUNETS = 128.138.0.0/255.255.0.0
Host_Alias CSNETS = 128.138.243.0, 128.138.204.0/24, 128.138.242.0
Host_Alias SERVERS = master, mail, www, ns
Host_Alias CDROM = orion, perseus, hercules
####第四部分，定义命令和命令地路径。命令一定要使用绝对路径，避免其他目录的同名命令被执行，造成安全隐患 ,因此使用的时候也是使用绝对路径!
Cmnd_Alias DUMPS = /usr/bin/mt, /usr/sbin/dump, /usr/sbin/rdump,\
/usr/sbin/restore, /usr/sbin/rrestore
Cmnd_Alias KILL = /usr/bin/kill
Cmnd_Alias PRINTING = /usr/sbin/lpc, /usr/bin/lprm
Cmnd_Alias SHUTDOWN = /usr/sbin/shutdown
Cmnd_Alias HALT = /usr/sbin/halt, /usr/sbin/fasthalt
Cmnd_Alias REBOOT = /usr/sbin/reboot, /usr/sbin/fastboot
Cmnd_Alias SHELLS = /usr/bin/sh, /usr/bin/csh, /usr/bin/ksh, \
/usr/local/bin/tcsh, /usr/bin/rsh, \
/usr/local/bin/zsh
Cmnd_Alias SU = /usr/bin/su
#### 这里是针对不同的用户采用不同地策略，比如默认所有的syslog直接通过auth 输出。FULLTIMERS组不用看到lecture（第一次运行时产生的消息）；用户millert使用sudo时不用输入密码；以及logfile的 路径在/var/log/sudo.log而且每一行日志中必须包括年。
Defaults syslog=auth
Defaults:FULLTIMERS !lecture
Defaults:millert !authenticate
Defaults@SERVERS log_year, logfile=/var/log/sudo.log
####root和wheel组的成员拥有任何权利。 如果想对一组用户进行定义，可以在组名前加上%，对其进行设置.
root ALL = (ALL) ALL
%wheel ALL = (ALL) ALL
####FULLTIMERS可以运行任何命令在任何主机而不用输入自己的密码
FULLTIMERS ALL = NOPASSWD: ALL
####PARTTIMERS可以运行任何命令在任何主机，但是必须先验证自己的密码。
PARTTIMERS ALL = ALL
####jack可以运行任何命令在定义地CSNET（128.138.243.0, 128.138.242.0和128.138.204.0/24的子网）中，不过注意前两个不需要匹配子网掩码，而后一个必须匹配掩码。
jack CSNETS = ALL
####lisa可以运行任何命令在定义为CUNETS（128.138.0.0）的子网中主机上。
lisa CUNETS = ALL
####用户operator可以运行DUMPS,KILL,PRINTING,SHUTDOWN,HALT,REBOOT以及在/usr/oper/bin中的所有命令。
operator ALL = DUMPS, KILL, PRINTING, SHUTDOWN, HALT, REBOOT,\
/usr/oper/bin/
####joe可以运行su operator命令
joe ALL = /usr/bin/su operator
####pete可以为除root之外地用户修改密码。
peteHPPA= /usr/bin/passwd [A-z]*, !/usr/bin/passwd root
####bob可以在SPARC和SGI机器上和OP用户组中的root和operator一样运行任何命令。
bob SPARC = (OP) ALL : SGI = (OP) ALL
####jim可以运行任何命令在biglab网络组中。Sudo默认“+”是一个网络组地前缀。
jim +biglab = ALL
####在secretaries中地用户帮助管理打印机，并且可以运行adduser和rmuser命令。
+secretaries ALL = PRINTING, /usr/bin/adduser, /usr/bin/rmuser
####fred能够直接运行oracle或者sybase数据库。
fred ALL = (DB) NOPASSWD: ALL
####john可以在ALPHA机器上，su除了root之外地所有人。
john ALPHA = /usr/bin/su [!-]*, !/usr/bin/su *root*
####jen可以在除了SERVERS主机组的机器上运行任何命令。
jen ALL, !SERVERS = ALL
####jill可以在SERVERS上运行/usr/bin/中的除了su和shell命令之外的所有命令。
jill SERVERS = /usr/bin/, !SU, !SHELLS
####steve可以作为普通用户运行在CSNETS主机上的/usr/local/op_commands/内的任何命令。
steve CSNETS = (operator) /usr/local/op_commands/
####matt可以在他的个人工作站上运行kill命令。
matt valkyrie = KILL
####WEBMASTERS用户组中的用户可以以www的用户名运行任何命令或者可以su www。
WEBMASTERS www = (www) ALL, (root) /usr/bin/su www
####任何用户可以mount或者umount一个cd-rom在CDROM主机上，而不用输入密码。
ALL CDROM = NOPASSWD: /sbin/umount /CDROM,\
/sbin/mount -o nosuid\,nodev /dev/cd0a /CDROM

