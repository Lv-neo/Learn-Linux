# sftp在centos6下的安装

###查看ssh版本

```
ssh -V  
```
来查看openssh的版本，如果低于4.8p1，需要自行升级安装

####创建sftp组

```
groupadd sftp
```

####创建一个sftp用户	

```
useradd -G sftp -s /bin/false test
passwd test
```

####指定home目录

```
mkdir -p /data/sftp/test
usermod -d /data/sftp/test test
```

####配置sshd_config
```
vim /etc/ssh/sshd_config
#找到如下这行，并注释掉
#Subsystem      sftp    /usr/libexec/openssh/sftp-server  

#增加
Subsystem       sftp    internal-sftp  

#在文件末尾处增加
#也可以Match User test
Match Group sftp  
#％u代表用户，指定用户的根目录
ChrootDirectory /data/sftp/%u  
#指定sftp命令
ForceCommand    internal-sftp  
#这两行，如果不希望该用户能使用端口转发的话就加上，否则删掉
AllowTcpForwarding no  
X11Forwarding no  
```

####设定目录权限

```
#设定Chroot目录权限
#有的说法是chown root:test，笔者在6.5下实践是指定给root才生效
chown root:root /data/sftp/test
chmod 755 /data/sftp/test
```

####重启sshd

```
service sshd restart  
```

####测试

```
sftp test@localhost
Connecting to localhost...
test@localhost's password: 
sftp> ls
test.txt  
sftp>

ssh test@localhost
test@localhost's password: 
This service allows sftp connections only.
Connection to localhost closed.
```

