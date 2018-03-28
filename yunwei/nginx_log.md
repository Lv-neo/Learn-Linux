# nginx日志分割脚本

```
## 零点执行该脚本
## Nginx 日志文件所在的目录
LOGS_PATH=/usr/local/logs
## 获取昨天的 yyyy-MM-dd
YESTERDAY=$(date -d "yesterday" +%Y-%m-%d)
## 移动文件
mv ${LOGS_PATH}/nginx/nginx_access.log ${LOGS_PATH}/nginx/access_${YESTERDAY}.log
mv ${LOGS_PATH}/nginx/error_nginx.log ${LOGS_PATH}/nginx/error_${YESTERDAY}.log

mv ${LOGS_PATH}/app/admin.mailejifen.com.access.log ${LOGS_PATH}/app/admin.mailejifen.com_${YESTERDAY}.log
mv ${LOGS_PATH}/app/h5.mailejifen.com.access.log ${LOGS_PATH}/app/h5.mailejifen.com_${YESTERDAY}.log
mv ${LOGS_PATH}/app/manage.mailejifen.com.access.log ${LOGS_PATH}/app/manage.mailejifen.com_${YESTERDAY}.log
mv ${LOGS_PATH}/app/upload.mailejifen.com.access.log ${LOGS_PATH}/app/upload.mailejifen.com_${YESTERDAY}.log
mv ${LOGS_PATH}/app/tongji.mailejifen.com.access.log ${LOGS_PATH}/app/tongji.mailejifen.com_${YESTERDAY}.log
## 向 Nginx 主进程发送 USR1 信号。USR1 信号是重新打开日志文件
kill -USR1 $(cat /run/nginx.pid)


find ${LOGS_PATH}/nginx/ -name '*.log' -type f -mtime +15 |xargs rm -f
find ${LOGS_PATH}/app/ -name '*.log' -type f -mtime +15 |xargs rm -f
~                                                                    
```

