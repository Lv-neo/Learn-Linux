# 磁盘的分区

##fdisk 

```
语法： fdisk [-l ] [设备名称]

-l ：后边不跟设备名会直接列出系统中所有的磁盘设备以及分区表，加上设备名会列出该设备的分区表。
```

```
[root@localhost ~]# fdisk -l

Disk /dev/sda: 999.7 GB, 999653638144 bytes
255 heads, 63 sectors/track, 121534 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x000e8a27

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *           1          26      204800   83  Linux
Partition 1 does not end on cylinder boundary.
/dev/sda2              26       65297   524288000   83  Linux
/dev/sda3           65297       69474    33554432   82  Linux swap / Solaris
/dev/sda4           69474      121535   418176000    5  Extended
/dev/sda5           69474      121535   418174976   83  Linux
```

* 如果不加-l 则进入另一个模式，在该模式下，可以对磁盘进行分区操作。

```
Command action
   a   toggle a bootable flag
   b   edit bsd disklabel
   c   toggle the dos compatibility flag
   d   delete a partition
   l   list known partition types
   m   print this menu
   n   add a new partition
   o   create a new empty DOS partition table
   p   print the partition table
   q   quit without saving changes
   s   create a new empty Sun disklabel
   t   change a partition's system id
   u   change display/entry units
   v   verify the partition table
   w   write table to disk and exit
   x   extra functionality (experts only)
```

n：重新建立一个新的分区。

w：保存操作。

q：退出。

d：删除一个分区


```
[root@iZ94h4rblbvZ ~]# fdisk /dev/vdb 
Welcome to fdisk (util-linux 2.23.2).

Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table
Building a new DOS disklabel with disk identifier 0x857f5681.

Command (m for help): p

Disk /dev/vdb: 107.4 GB, 107374182400 bytes, 209715200 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x857f5681

   Device Boot      Start         End      Blocks   Id  System

Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): 
Using default response p
Partition number (1-4, default 1): 
First sector (2048-209715199, default 2048): 
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-209715199, default 209715199): 
Using default value 209715199
Partition 1 of type Linux and of size 100 GiB is set

Command (m for help): p

Disk /dev/vdb: 107.4 GB, 107374182400 bytes, 209715200 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x857f5681

   Device Boot      Start         End      Blocks   Id  System
/dev/vdb1            2048   209715199   104856576   83  Linux

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
```

* 用n创建一个新的分区
* 会提示要建立e （extended 扩展分区）或者p （primary partition主分区）
这里笔者选择主分区，所以按了p回车后
* 由于这个是数据盘，笔者直接选择默认全挂载
* 一路回车之后，按p 查看硬盘分区情况
* 确认无误后，按w保存分区该模式自动退出，如果你不想保存分区信息直接按q即可退出。

>其他扩展分区参考：http://www.92csz.com/study/linux/8.htm




