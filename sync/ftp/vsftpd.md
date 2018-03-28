# 安装vsftpd

##在安装前查看是否已安装vsftpd
```
[root@localhost ~]# rpm -q vsftpd
vsftpd-2.2.2-21.el6.x86_64
```
##使用yum安装
```
[root@localhost ~]# yum -y install vsftpd
```

##文件配置
安装完之后在/etc/vsftpd/路径下会存在三个配置文件。
```
vsftpd.conf: 主配置文件
ftpusers: 指定哪些用户不能访问FTP服务器,这里的用户包括root在内的一些重要用户。
user_list: 指定的用户是否可以访问ftp服务器，通过vsftpd.conf文件中的userlist_deny的配置来决定配置中的用户是否可以访问，userlist_enable=YES ，userlist_deny=YES ，userlist_file=/etc/vsftpd/user_list 这三个配置允许文件中的用户访问FTP。
```

###查看主配置文件的默认配置
```
[root@localhost ~]# cat /etc/vsftpd/vsftpd.conf |grep -v '^#';
anonymous_enable=NO #是否允许匿名用户
local_enable=YES  #允许使用本地用户账号登陆
write_enable=YES #允许ftp用户写数据
local_umask=022
dirmessage_enable=YES
xferlog_enable=YES
connect_from_port_20=YES #通过20端口传输数据
xferlog_std_format=YES
idle_session_timeout=600
ftpd_banner=Welcome to FTP service.
chroot_local_user=YES
local_root=/data/liyu
listen=YES

pam_service_name=vsftpd
userlist_enable=YES
tcp_wrappers=YES
[root@localhost ~]# 
```

###参数说明
```
ftpd_banner=welcome to ftp service ：设置连接服务器后的欢迎信息

idle_session_timeout=60 ：限制远程的客户机连接后，所建立的控制连接，在多长时间没有做任何的操作就会中断(秒)

data_connection_timeout=120 ：设置客户机在进行数据传输时,设置空闲的数据中断时间

accept_timeout=60 设置在多长时间后自动建立连接

connect_timeout=60 设置数据连接的最大激活时间，多长时间断开，为别人所使用;

max_clients=200 指明服务器总的客户并发连接数为200

max_per_ip=3 指明每个客户机的最大连接数为3

local_max_rate=50000(50kbytes/sec)  本地用户最大传输速率限制

anon_max_rate=30000匿名用户的最大传输速率限制

pasv_min_port=端口

pasv-max-prot=端口号 定义最大与最小端口，为0表示任意端口;为客户端连接指明端口;

listen_address=IP地址 设置ftp服务来监听的地址，客户端可以用哪个地址来连接;

listen_port=端口号 设置FTP工作的端口号，默认的为21

local_root=path 无论哪个用户都能登录的用户，定义登录帐号的主目录, 若没有指定，则每一个用户则进入到个人用户主目录;

chroot_local_user=yes/no 是否锁定本地系统帐号用户主目录(所有);锁定后，用户只能访问用户的主目录/home/user;
chroot_list_enable=yes/no 启用不锁定用户在主目录的名单

chroot_list_file=/etc/vsftpd/chroot_list指定列表文件

userlist_enable=YES/NO 是否加载用户列表文件;

userlist_deny=YES 表示上面所加载的用户允许登录;

userlist_file=/etc/vsftpd/user_list 指定列表文件
```

##创建FTP连接用户
```
#创建用户ftpuser
useradd ftpusr

#设置用户只能ftp不能登入
usermod -s /sbin/nologin ftpuser

#设置用户密码
passwd ftpusr

#修改用户的家目录位/mnt
usermod -d /mnt ftpuser

#启动FTP服务
service vsftpd start

```

##解决普通的FTP无法登入问题
linux默认是带安全机制，使用普通的ftp 21端口无法连接到ftp服务器，使用sftp就可以。这个时候需要关闭selinux，修改配置文件需要重启服务器。

```
vim /etc/sysconfig/selinux
改成selinux=disabled
```

不重启服务器的方法：

```
//setenforce 1:设置SELinux 成为enforcing模式
//setenforec 2:设置SELinux 成为permissive模式
setenforce 0
```

查看SELinux状态
```

/usr/sbin/sestatus -v
```

##附录
http://www.cnblogs.com/xiongpq/p/3384759.html
http://www.cnblogs.com/chenmh/p/5365274.html

