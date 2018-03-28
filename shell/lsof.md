# lsof查看打开文件的数量以及统计TCP连接状态 2011-09-08 16:49:26

用netstat命令去统计服务器目前的网络连接状态
netstat -n|awk '/^tcp/{++S[$NF]}END{for(a in S) print a,S[a]}'
 
netstat -an | awk '/:80/{print $6}' | sort | uniq -c

查找打开文件最多的信息如下： 
lsof -n|awk '{print $2}'|sort|uniq -c |sort -nr|more

查看各个进程打开的文件数据量：
lsof -n |awk '{print $2} " " $3'|sort|uniq -c |sort -nr|more


