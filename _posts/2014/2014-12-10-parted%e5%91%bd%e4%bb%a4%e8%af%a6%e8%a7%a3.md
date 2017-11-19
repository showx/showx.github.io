---
layout: post
title: parted命令详解
date: 2014-12-10 10:14
author: admin
comments: true
categories: []
---
<div>parted命令详解</div>
<div></div>
<div>用法：parted [选项]... [设备 [命令 [参数]...]...]
</div>
<div>将带有“参数”的命令应用于“设备”。如果没有给出“命令”，则以交互模式运行.</div>
<div>帮助选项：</div>
<div>-h, --help                    显示此求助信息</div>
<div>-l, --list                    列出所有设别的分区信息</div>
<div>-i, --interactive             在必要时，提示用户</div>
<div>-s, --script                  从不提示用户</div>
<div>-v, --version                 显示版本</div>
<div>操作命令：   www.2cto.com</div>
<div>检查 MINOR                           #对文件<a href="http://www.2cto.com/os/" target="_blank">系统</a>进行一个简单的检查</div>
<div>cp [FROM-DEVICE] FROM-MINOR TO-MINOR #将文件系统复制到另一个分区</div>
<div>help [COMMAND]                       #打印通用求助信息，或关于 COMMAND 的信息</div>
<div>mklabel 标签类型                      #创建新的磁盘标签 (分区表)</div>
<div>mkfs MINOR 文件系统类型               #在 MINOR 创建类型为“文件系统类型”的文件系统</div>
<div>mkpart 分区类型 [文件系统类型] 起始点 终止点    #创建一个分区</div>
<div>mkpartfs 分区类型 文件系统类型 起始点 终止点    #创建一个带有文件系统的分区</div>
<div>move MINOR 起始点 终止点              #移动编号为 MINOR 的分区</div>
<div>name MINOR 名称                      #将编号为 MINOR 的分区命名为“名称”</div>
<div>print [MINOR]                        #打印分区表，或者分区</div>
<div>quit                                 #退出程序</div>
<div>rescue 起始点 终止点                  #挽救临近“起始点”、“终止点”的遗失的分区</div>
<div>resize MINOR 起始点 终止点            #改变位于编号为 MINOR 的分区中文件系统的大小</div>
<div>rm MINOR                             #删除编号为 MINOR 的分区</div>
<div>select 设备                          #选择要编辑的设备</div>
<div>set MINOR 标志 状态                   #改变编号为 MINOR 的分区的标志
</div>
<div>操作实例：</div>
<div>(parted)表示在parted中输入的命令，其他为自动打印的信息</div>
<div>1、首先类似fdisk一样，先选择要分区的硬盘，此处为/dev/hdd：</div>
<div>[root@10.10.90.97 ~]# parted /dev/hdd</div>
<div>GNU Parted 1.8.1</div>
<div>Using /dev/hdd</div>
<div>Welcome to GNU Parted! Type 'help' to view a list of commands.
</div>
<div>2、选择了/dev/hdd作为我们操作的磁盘，接下来需要创建一个分区表(在parted中可以使用help命令打印帮助信息)：</div>
<div>(parted) mklabel  www.2cto.com</div>
<div>Warning: The existing disk label on /dev/hdd will be destroyed and all data on this disk will be lost. Do you want to continue?</div>
<div>Yes/No?(警告用户磁盘上的数据将会被销毁，询问是否继续，我们这里是新的磁盘，输入yes后回车) yes</div>
<div>New disk label type? [ms<a href="http://www.2cto.com/os/dos/" target="_blank">dos</a>]? (默认为msdos形式的分区，我们要正确分区大于2TB的磁盘，应该使用gpt方式的分区表，输入gpt后回车)gpt
</div>
<div>3、创建好分区表以后，接下来就可以进行分区操作了，执行mkpart命令，分别输入分区名称，文件系统和分区 的起止位置</div>
<div>(parted) mkpart</div>
<div>Partition name? []? dp1</div>
<div>File system type? [ext2]? ext3</div>
<div>Start? 0</div>
<div>End? 500GB
</div>
<div>4、分好区后可以使用print命令打印分区信息，下面是一个print的样例</div>
<div>(parted) print</div>
<div>Model: VBOX HARDDISK (ide)</div>
<div>Disk /dev/hdd: 2199GB</div>
<div>Sector size (logical/physical): 512B/512B</div>
<div>Partition Table: gpt</div>
<div>Number Start End Size File system Name Flags</div>
<div>1 17.4kB 500GB 500GB dp1
</div>
<div>5、如果分区错了，可以使用rm命令删除分区，比如我们要删除上面的分区，然后打印删除后的结果</div>
<div>(parted)rm 1 #rm后面使用分区的号码</div>
<div>(parted) print</div>
<div>Model: VBOX HARDDISK (ide)</div>
<div>Disk /dev/hdd: 2199GB</div>
<div>Sector size (logical/physical): 512B/512B</div>
<div>Partition Table: gpt  www.2cto.com</div>
<div>Number Start End Size File system Name Flags
</div>
<div>6、按照上面的方法把整个硬盘都分好区，下面是一个分完后的样例</div>
<div>(parted) mkpart</div>
<div>Partition name? []? dp1</div>
<div>File system type? [ext2]? ext3</div>
<div>Start? 0</div>
<div>End? 500GB</div>
<div>(parted) mkpart</div>
<div>Partition name? []? dp2</div>
<div>File system type? [ext2]? ext3</div>
<div>Start? 500GB</div>
<div>End? 2199GB</div>
<div>(parted) print</div>
<div>Model: VBOX HARDDISK (ide)</div>
<div>Disk /dev/hdd: 2199GB</div>
<div>Sector size (logical/physical): 512B/512B</div>
<div>Partition Table: gpt</div>
<div>Number Start End Size File system Name Flags</div>
<div>1 17.4kB 500GB 500GB dp1</div>
<div>2 500GB 2199GB 1699GB dp2
</div>
<div>7、由于parted内建的mkfs还不够完善，所以完成以后我们可以使用quit命令退出parted并使用 系统的mkfs命令对分区进行格式化了，此时如果使用fdisk -l命令打印分区表会出现警告信息，这是正常的  www.2cto.com</div>
<div>[root@10.10.90.97 ~]# fdisk -l</div>
<div>WARNING: GPT (GUID Partition Table) detected on '/dev/hdd'! The util fdisk doesn't support GPT. Use GNU Parted.</div>
<div>Disk /dev/hdd: 2199.0 GB, 2199022206976 bytes</div>
<div>255 heads, 63 sectors/track, 267349 cylinders</div>
<div>Units = cylinders of 16065 * 512 = 8225280 bytes</div>
<div>Device Boot Start End Blocks Id System</div>
<div>/dev/hdd1 1 267350 2147482623+ ee EFI GPT</div>
<div>[root@10.10.90.97 ~]# mkfs.ext3 /dev/hdd1</div>
<div>[root@10.10.90.97 ~]# mkfs.ext3 /dev/hdd2</div>
<div>[root@10.10.90.97 ~]# mkdir /dp1 /dp2</div>
<div>[root@10.10.90.97 ~]# mount /dev/hdd1 /dp1</div>
<div>[root@10.10.90.97 ~]# mount /dev/hdd2 /dp2</div>
