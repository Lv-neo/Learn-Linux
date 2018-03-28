# 常用命令

##df

* df 查看已挂载磁盘的总容量、使用容量、剩余容量等，可以不加任何参数，默认是按k为单位显示的

```
[root@localhost ~]# df 
Filesystem              1K-blocks      Used Available Use% Mounted on
/dev/mapper/centos-root   8775680   3531316   5244364  41% /
devtmpfs                   228052         0    228052   0% /dev
tmpfs                      234316         0    234316   0% /dev/shm
tmpfs                      234316      4404    229912   2% /run
tmpfs                      234316         0    234316   0% /sys/fs/cgroup
/dev/sda1                  508588     84492    424096  17% /boot
none                    244277768 122642516 121635252  51% /vagrant
```

> Filesystem 表示扇区，也就是你划分磁盘时所分的区；
> 1K-blocks/1M-blocks表示以1K/1M为单位；
> Used 和 Available 分别是已使用和剩余；
> Use% 就是已经使用的百分比，如果这个值大于90% 那么你就应该注意了，磁盘很有可能马上就会变满的；
> Mounted on 则表示该分区（扇区）所挂载的地方。

* df常用参数有 –i -h -k –m等

```
-i 使用inodes 显示结果
-h 使用合适的单位显示，例如G
-k -m 分别为使用K，M为单位显示
```

##du

* du 用来查看某个目录所占空间大小

```
语法：du [-abckmsh] [文件或者目录名] 常用的参数有：

-a：全部文件与目录大小都列出来。如果不加任何选项和参数只列出目录（包含子目录）大小。

-b：列出的值以bytes为单位输出，默认是以Kbytes

-c：最后加总

-k：以KB为单位输出

-m：以MB为单位输出

-s：只列出总和

-h：系统自动调节单位，例如文件太小可能就几K，那么就以K为单位显示，如果大到几G，则就以G为单位显示。笔者习惯用 du –sh filename 这样的形式。


```


