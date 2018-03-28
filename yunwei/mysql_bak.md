# mysql 备份脚本

```
#!/usr/bin
/usr/local/mysql/bin/mysqldump -uroot -p'mysqlYy1024@60' credits >/data/bak/credits_$(date +%Y%m%d).dump

find /data/bak/ -name '*.dump' -type f -mtime +15 |xargs rm -f
```

