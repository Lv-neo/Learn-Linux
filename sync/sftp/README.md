# 安全文件传送协议
sftp是Secure File Transfer Protocol的缩写，安全文件传送协议。可以为传输文件提供一种安全的加密方法。sftp 与 ftp 有着几乎一样的语法和功能。SFTP 为 SSH的一部分，是一种传输档案至 Blogger 伺服器的安全方式。其实在SSH软件包中，已经包含了一个叫作SFTP(Secure File Transfer Protocol)的安全文件传输子系统，SFTP本身没有单独的守护进程，它必须使用sshd守护进程（端口号默认是22）来完成相应的连接操作，所以从某种意义上来说，SFTP并不像一个服务器程序，而更像是一个客户端程序。SFTP同样是使用加密传输认证信息和传输的数据，所以，使用SFTP是非常安全的。但是，由于这种传输方式使用了加密/解密技术，所以传输效率比普通的FTP要低得多，如果您对网络安全性要求更高时，可以使用SFTP代替FTP。

##连接方法
windows中可以使用Core FTP，FileZilla, WinSCP，Xftp来连接SFTP进行上传，下载文件，建立，删除目录等操作。
linux下直接在终端中输入：sftp username@remote ip(or remote host name)。出现验证时，只需填入正确的密码即可实现远程链接。登入成功后终端呈现出:sftp>....
在sftp的环境下的操作就和一般ftp的操作类似了，ls,rm,mkdir,dir,pwd,等指令都是对远端进行操作，如果要对本地操作，只需在上述的指令上加‘l’变为：lls,lcd, lpwd等。当然既然是ftp，当然得说它的上传和下载咯！
上传：put /path/filename(本地主机) /path/filename(远端主机)；
下载：get /path/filename(远端主机) /path/filename(本地主机)。
另外提一下sftp在非正规端口（正规的是22号端口）登录：sftp -o port=1000 username@remote ip.


