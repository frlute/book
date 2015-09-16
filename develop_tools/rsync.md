# Rsync 远程备份工具的使用
>Rsync是一个远程数据同步工具，可通过LAN 或互联网快速同步多台主机间的文件。Rsync具有快速、安全、占用带宽少以及无需特权等特色。

**注**：下面所有例子中 `– -` 之间实际上是没有空格的，使用时请删除空格。

## 简单介绍
Rsync是一个远程数据同步工具，可通过LAN 或互联网快速同步多台主机间的文件。Rsync 本来是用以取代 rcp的一个工具，它当前由 rsync.samba.org 维护。Rsync 使用所谓的`”Rsync演算法”`来使*本地和远程两个主机之间的文件达到同步*，这个算法只传送两个文件的**不同部分**，而不是每次都整份传送，因此速度相当快。

##Rsync 的特色

 - 快速：第一次同步时 rsync 会复制全部内容，但在下一次只传输修改过的文件。
 - 安全：rsync 允许通过 ssh 协议来加密传输数据。
 - 更少的带宽：rsync 在传输数据的过程中可以实行压缩及解压缩操作，因此可以使用更少的带宽。
 - 特权：安装和执行 rsync 无需特别的权限

## 基本语法

    rsync options source destination
源和目标都可以是本地或远程，在进行远程传输的时候，需要指定**登录名**、**远程服务器**及**文件位置**。

###样例

####在本地机器上对两个目录进行同步

    ➜  ~  rsync -zvt ./code/server/xx ./temp
    xx.py

    sent 3452 bytes  received 42 bytes  6988.00 bytes/sec
    total size is 10852  speedup is 3.11

###参数

 * -z 开启压缩
 * -v 详情输出
 * -r 表示递归

####利用 rsync -a 让同步时保留时间标记

rsync 选项 -a 称为归档模式，执行以下操作:

 - 递归模式
 - 保留符号链接
 - 保留权限
 - 保留时间标记
 - 保留用户名及组名

```
➜  ~  rsync -azv ./code/server/xx.py ./temp
building file list ... done
xx.py

sent 3488 bytes  received 42 bytes  7060.00 bytes/sec
total size is 10852  speedup is 3.07
```

####仅同步一个文件

    ➜  ~  rsync -v ./code/server/xx.py ./temp
    xx.py

    sent 10945 bytes  received 42 bytes  21974.00 bytes/sec
    total size is 10852  speedup is 0.99

####从本地同步文件到远程服务器

    $ rsync -avz /root/temp/ thegeekstuff@192.168.200.10:/home/thegeekstuff/temp/
    Password:
    building file list … done
    ./
    rpm/
    rpm/Basenames
    rpm/Conflictname

    sent 15810261 bytes received 412 bytes 2432411.23 bytes/sec
    total size is 45305958 speedup is 2.87

就像你所看到的，需要在远程目录前加上 ssh 登录方式，格式为 `username@machinename:path`

####同步远程文件到本地

和上面差不多，做个相反的操作

    $ rsync -avz thegeekstuff@192.168.200.10:/var/lib/rpm /root/temp
    Password:
    receiving file list … done
    rpm/
    rpm/Basenames
    .
    sent 406 bytes received 15810230 bytes 2432405.54 bytes/sec
    total size is 45305958 speedup is 2.87

####同步时指定远程 shell

用`-e` 参数可以指定远程 ssh ，比如用 `rsync -e ssh` 来指定为 **ssh**:

    $ rsync -avz -e ssh thegeekstuff@192.168.200.10:/var/lib/rpm /root/temp
    Password:
    receiving file list … done
    rpm/
    rpm/Basenames

    sent 406 bytes received 15810230 bytes 2432405.54 bytes/sec
    total size is 45305958 speedup is 2.87

####不要覆盖被修改过的目的文件

使用 rsync -u 选项可以排除被修改过的目的文件

    $ ls -l /root/temp/Basenames
    total 39088
    -rwxr-xr-x 1 root root 4096 Sep 2 11:35 Basenames

    $ rsync -avzu thegeekstuff@192.168.200.10:/var/lib/rpm /root/temp
    Password:
    receiving file list … done
    rpm/

    sent 122 bytes received 505 bytes 114.00 bytes/sec
    total size is 45305958 speedup is 72258.31

    $ ls -lrt
    total 39088
    -rwxr-xr-x 1 root root 4096 Sep 2 11:35 Basenames

####仅仅同步目录权（不同步文件）

 - 使用 -d 参数

```
    $ rsync -v -d thegeekstuff@192.168.200.10:/var/lib/ .
    Password:
    receiving file list … done
    logrotate.status
    CAM/
    YaST2/
    acpi/

    sent 240 bytes received 1830 bytes 318.46 bytes/sec
    total size is 956 speedup is 0.46
```

#### 查看每个文件的传输进程

使用 – -progress 参数

    $ rsync -avz – -progress thegeekstuff@192.168.200.10:/var/lib/rpm/ /root/temp/
    Password:
    receiving file list …
    19 files to consider
    ./
    Basenames
    5357568 100% 14.98MB/s 0:00:00 (xfer#1, to-check=17/19)
    Conflictname
    12288 100% 35.09kB/s 0:00:00 (xfer#2, to-check=16/19)
    .
    .
    .
    sent 406 bytes received 15810211 bytes 2108082.27 bytes/sec
    total size is 45305958 speedup is 2.87

#### 删除在目的文件夹中创建的文件

用 – -delete 参数

    Source and target are in sync. Now creating new file at the target.
    $ > new-file.txt

    $ rsync -avz – -delete thegeekstuff@192.168.200.10:/var/lib/rpm/ .
    Password:
    receiving file list … done
    deleting new-file.txt
    ./

    sent 26 bytes received 390 bytes 48.94 bytes/sec
    total size is 45305958 speedup is 108908.55

####不要在目的文件夹中创建新文件

有时能只想同步目的地中存在的文件，而排除源文件中新建的文件，可以使用 – -exiting 参数

    $ rsync -avz –existing root@192.168.1.2:/var/lib/rpm/ .
    root@192.168.1.2′s password:
    receiving file list … done
    ./

    sent 26 bytes received 419 bytes 46.84 bytes/sec
    total size is 88551424 speedup is 198991.96

####查看源和目的文件之间的改变情况

用 -i 参数
```
$ rsync -avzi thegeekstuff@192.168.200.10:/var/lib/rpm/ /root/temp/
Password:
receiving file list … done
>f.st…. Basenames
.f….og. Dirnames

sent 48 bytes received 2182544 bytes 291012.27 bytes/sec
total size is 45305958 speedup is 20.76
```

输出结果中在每个文件最前面会多显示 9 个字母，分别表示为
```
> 已经传输 
f 表示这是一个文件 
d 表示这是一个目录 
s 表示尺寸被更改 
t 时间标记有变化 
o 用户被更改 
g 用户组被更改
```

####在传输时启用包含和排除模式

    $ rsync -avz – -include ‘P*’ – -exclude ‘*’ thegeekstuff@192.168.200.10:/var/lib/rpm/ /root/temp/
    Password:
    receiving file list … done
    ./
    Packages
    Providename
    Provideversion
    Pubkeys

    sent 129 bytes received 10286798 bytes 2285983.78 bytes/sec
    total size is 32768000 speedup is 3.19

####不要传输大文件

使用 – - max-size 参数

    $ rsync -avz – -max-size=’100K’ thegeekstuff@192.168.200.10:/var/lib/rpm/ /root/temp/
    Password:
    receiving file list … done
    ./
    Conflictname
    Group
    Installtid
    Name
    Sha1header
    Sigmd5
    Triggername

    sent 252 bytes received 123081 bytes 18974.31 bytes/sec
    total size is 45305958 speedup is 367.35

####传输所有文件

不管有没有改变，再次把所有文件都传输一遍，用 -W 参数
```
# rsync -avzW thegeekstuff@192.168.200.10:/var/lib/rpm/ /root/temp
Password:
receiving file list … done
./
Basenames
Conflictname
Dirnames
Filemd5s
Group
Installtid
Name

sent 406 bytes received 15810211 bytes 2874657.64 bytes/sec
total size is 45305958 speedup is 2.87
```

##强推代码脚本

```
#!/bin/sh

echo 'begin upload code' \
&& rsync  --progress -alrI --exclude='*shell*' --exclude='upload_xls' --exclude='.git' --exclude='logs' --exclude='test/unit/htmlcov/*' --exclude='*.pyc' --exclude='start_dev.sh' --exclude='start.sh' --exclude='dump.rdb' --rsh='ssh -p 8005' ./* root@120.132.57.113:/data/opt/sites/transformer_dev \

&& ssh root@120.132.57.113 -p 8005 supervisorctl -u user -p 123 restart transformer_master:00 transformer_master_timer

&& echo 'end'
```

## 参考文献

[Linux远程备份工具Rsync使用案例](http://os.51cto.com/art/201009/225962.htm "需要详细了解")

[其他参考文档](http://www.thegeekstuff.com/2010/09/rsync-command-examples/ "英文文档")
