# Linux学习笔记（Rhel 6）

by元子

[TOC]

## Yum

#### 配置本地Yum源

1. 虚拟机设置好原版镜像链接到虚拟机
2. 虚拟机系统中， iso 文件是 /dev/sr0 设备,系统会默认把iso文件挂载到 /run/media/$username/ 目录下。

```ruby
[root@localhost ~]# df -Th
Filesystem     Type     Size  Used Avail Use% Mounted on
/dev/sda2      ext3     6.8G  2.6G  3.8G  41% /
tmpfs          tmpfs    503M   76K  503M   1% /dev/shm
/dev/sda1      ext4     194M   28M  157M  15% /boot
/dev/sda5      ext4     975M   18M  908M   2% /home
/dev/sr0       iso9660  3.1G  3.1G     0 100% /media/RHEL_6.5 i386 Disc 1
```

2. 将iso文件挂载到 /mnt/cdrom 下

```ruby
[root@localhost mnt]# mount /dev/sr0 /mnt/cdrom
mount: block device /dev/sr0 is write-protected, mounting read-only(只读方式挂载)
```

3. 挂载好iso文件之后，修改源的配置文件 /etc/yum.repos.d/ ，它默认有一个文件，把它删除，然后自己新建以 .repo 结尾的文件，用vim编辑器打开。输入以下的配置，保存。

```ruby
[name]               #括号中的名称为仓库源名称，通常为字母和数字，必须填写
name=my new repo     #对yum的描述，可写可不写
baseurl=file:///mnt/cdrom    #baseurl表示声明yum可以管理并使用的rpm包路径,必须填写
enabled=1            #enabled表示当前仓库是否开启，1为开启，0为关闭，此项不写默认为开启
gpgcheck=0           #gpgcheck表示安装rpm包时，是否基于公私钥对匹配包的安全信息，1表示开启，0表示关闭，此项不写默认为验证
```

4. 清除缓存

```ruby
[root@localhost /]# yum clean all
```

5. 查看包数量

```ruby
[root@localhost /]# yum list | wc -l
2902
```



####  配置网络yum源

1. 查看yum包并删除

```ruby
[root@localhost ~]# rpm -qa |grep yum 
[root@localhost ~]# rpm -qa |grep yum |xargs rpm -e --nodeps
```

2. 下载rpm包

   ​       python-iniparse-0.3.1-2.1.el6.noarch.rpm

   ​       yum-metadata-parser-1.1.2-14.1.el6.x86_64.rpm （注意系统位数）

   ​       yum-3.2.29-69.el6.centos.noarch.rpm

   ​       yum-plugin-fastestmirror-1.1.30-30.el6.noarch.rpm

   ​      python-urlgrabber-3.9.1-11.el6.noarch.rpm（报错装）

3. 按顺序安装

```ruby
[root@localhost yum-install]# rpm -ivh python-iniparse-0.3.1-2.1.el6.noarch.rpm 
warning: python-iniparse-0.3.1-2.1.el6.noarch.rpm: Header V3 RSA/SHA256 Signature, key ID c105b9de: NOKEY
Preparing...                ########################################### [100%]
	package python-iniparse-0.3.1-2.1.el6.noarch is already installed

[root@localhost yum-install]# rpm -ivh yum-metadata-parser-1.1.2-16.el6.i686.rpm 
warning: yum-metadata-parser-1.1.2-16.el6.i686.rpm: Header V3 RSA/SHA1 Signature, key ID c105b9de: NOKEY
Preparing...                ########################################### [100%]
   1:yum-metadata-parser    ########################################### [100%]

[root@localhost yum-install]# rpm -ivh yum-3.2.29-81.el6.centos.noarch.rpm  yum-plugin-fastestmirror-1.1.30-41.el6.noarch.rpm 
warning: yum-3.2.29-81.el6.centos.noarch.rpm: Header V3 RSA/SHA1 Signature, key ID c105b9de: NOKEY
Preparing...                ########################################### [100%]
   1:yum-plugin-fastestmirro########################################### [ 50%]
   2:yum                    ########################################### [100%]

```

​	*如果有错误提示安装这个缺少的依赖包*

```ruby
[root@localhost ~]# rpm -ivh python-urlgrabber-3.9.1-11.el6.noarch.rpm
```

3. 遇到旧版yum可以先删除

```
rpm -e
```

4. 再次输入yum得到如下提示

```ruby
[root@Yuanzi ~]# yum
Loaded plugins: refresh-packagekit, security
You need to give some command
Usage: yum [options] COMMAND

List of Commands:

check          Check for problems in the rpmdb
check-update   Check for available package updates
clean          Remove cached data
deplist        List a package's dependencies
distribution-synchronization Synchronize installed packages to the latest available versions
downgrade      downgrade a package
erase          Remove a package or packages from your system
groupinfo      Display details about a package group
groupinstall   Install the packages in a group on your system
grouplist      List available package groups
groupremove    Remove the packages in a group from your system
```



4. 修改配置文件

```ruby
[root@localhost ~]# vim /etc/yum.repos.d/yuanzi.repo

[name]               #括号中的名称为仓库源名称，通常为字母和数字，必须填写
name=my new repo     #对yum的描述，可写可不写
baseurl=http://mirrors.163.com/centos/6/os/$basearch/    #baseurl表示声明yum可以管理并使用的rpm包路径,必须填写
enabled=1            #enabled表示当前仓库是否开启，1为开启，0为关闭，此项不写默认为开启
gpgcheck=0           #gpgcheck表示安装rpm包时，是否基于公私钥对匹配包的安全信息，1表示开启，0表示关闭，此项不写默认为验证
```

5. 清除yum缓存并查看包数量

```ruby
[root@localhost ~]# yum clean all
[root@localhost ~]# yum list |wc -l
5648
```



## 安装GNOME、KDE图形界面

1.进root

2.首先安装X(X Window System)，　//注意有引号

```shell
yum groupinstall "X Window System" 
```

   在这里我们可以检查一下我们已经安装的软件以及可以安装的软件

```shell
yum grouplist
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
Available Environment Groups:
   Minimal Install
   Compute Node
   Infrastructure Server
   File and Print Server
   Cinnamon Desktop
   MATE Desktop
   Basic Web Server
   Virtualization Host
   Server with GUI
   GNOME Desktop
   KDE Plasma Workspaces
   Development and Creative Workstation
Available Groups:
   Cinnamon
   Compatibility Libraries
   Console Internet Tools
   Development Tools
   Educational Software
   Electronic Lab
   Fedora Packager
   General Purpose Desktop
   Graphical Administration Tools
   Haskell
   LXQt Desktop
   Legacy UNIX Compatibility
   MATE
   Milkymist
   Scientific Support
   Security Tools
   Smart Card Support
   System Administration Tools
   System Management
   TurboGears application framework
   Xfce
Done
```

3.安装图形界面软件 GNOME 注意名称对应！！

```shell
yum groupinstall "GNOME Desktop" "Graphical Administration Tools"
```

### 安装VNC远程管理

1.安装vnc server

```shell
yum install tigervnc-server -y
```

2.从VNC备份库中复制service文件到系统service服务管理目录下

```shell
 cp/lib/systemd/system/vncserver@.service/etc/systemd/system/vncserver@:1.service
```

3.修改vncserver@:1.service文件



##  Samba

#### 安装&卸载

1. 查看是否安装

```shell
[root@localhost ~]# rpm -qa |grep samba
```

2. 卸载已安装的

```shell
[root@localhost ~]# rpm -qa |grep samba |xargs rpm -e --nodeps
```

3. 安装samba

```shell
[root@localhost ~]# yum clean all
[root@localhost ~]# yum install samba -y
```

4. 查看

```shell
[root@localhost ~]# rpm -qa |grep samba 
samba-winbind-3.6.23-51.el6.i686
samba-winbind-clients-3.6.23-51.el6.i686
samba-3.6.23-51.el6.i686
samba-common-3.6.23-51.el6.i686
```

#### samba软件结构

`/etc/samba/smb.conf` 　　　　　　 　　　　　　     　#samba服务的主要配置文件 

`/etc/samba/lmhosts`     　　　　　　　　　　　　　    #samba服务的域名设定，主要设置IP地址对应的域名，类似linux系统的/etc/hosts 

`/etc/samba/smbusers`    　　　　　　　　　　　  　　 #samba服务设置samba虚拟用户的配置文件 

`/var/log/samba`         　　　　　　　　　　　　　　　  #samab服务存放日志文件

`/var/lib/samba/private/{passdb.tdb,secrets.tdb} `#存放samba的用户账号和密码数据库文档



#### samba基本命令

```shell
[root@localhost ~]# service smb start/stop/restart/reload
或者
[root@localhost ~]# /etc/rc.d/init.d/smb start/stop/restart/reload
```

- 去掉注释行

```shell
[root@Yuanzi ~]# cp /etc/samba/smb.conf /etc/samba/smb.conf.bak ;egrep -v "#|^$" /etc/grep -v "^;">/etc/samba/smb.conf
```

#### samba配置流程

```shell
vim /etc/samba/smb.conf
```



## DHCP



```shell
[root@Yuanzi dhcp]# cp /usr/share/doc/dhcp-4.1.1/dhcpd.conf.sample  /etc/dhcp/dhcpd.conf
```

## Swap分区

### 创建swap文件

在根目录（`/`）下创建一个名叫`swapfile`的文件，当然你也可以选择你喜欢的文件名。该文件分配的空间将等于我们需要的swap空间。

最快捷的创建方式是`fallocate`命令，该命令能够创建一个预分配指定大小空间的文件。输入如下指令创建一个4GB的文件：

```shell
sudo fallocate -l 4G /swapfile
```

输入密码后，该swap文件将立即创建完毕。我们可以用`ls`命令检查文件大小：

```shell
ls -lh /swapfile
-rw-r--r-- 1 root root 4.0G Oct 30 11:00 /swapfile
```

至此，我们的swap文件就创建完毕了。

### 启用swap文件

现在我们已经有了swap文件，但系统还不知道应该使用该文件作为swap，这就需要我们告知系统将该文件格式化为swap并启用起来。

首先我们需要更改swap文件的权限，确保只有root才可读，否则会有很大的安全隐患。使用`chmod`命令进行权限操作：

```shell
sudo chmod 600 /swapfile
```

如此，该文件的读写都只有root才能操作。使用`ls -lh`命令检查一下：

```shell
ls -lh /swapfile
-rw------- 1 root root 4.0G Oct 30 11:00 /swapfile
```

然后，使用如下命令告知系统将该文件用于swap：

```shell
sudo mkswap /swapfile
Setting up swapspace version 1, size = 4194300 KiB
no label, UUID=b99230bb-21af-47bc-8c37-de41129c39bf
```

现在，这个swap文件就可以作为swap空间使用了。输入如下命令开始使用该swap：

```shell
sudo swapon /swapfile
```

输入如下命令来确认一下设置是否已经生效：

```shell
swapon -s
Filename                Type        Size    Used    Priority
/swapfile               file        4194300 0     -1
```

可以看到返回的结果中已经有我们刚才设置的swap。再使用`free`工具确认一下：

```shell
free -m


             total       used       free     shared    buffers     cached
Mem:          3953        315       3637          8         11        107
-/+ buffers/cache:        196       3756
Swap:         4095          0       4095
```

### 使Swap文件永久生效

通过修改`fstab`文件来实现系统重启后依然生效（这是一个管理文件系统和分区的表）

用`sudo`权限打开该文件编辑：

```shell
sudo nano /etc/fstab
```

在文件末尾加入下面这行内容，告诉操作系统自动使用刚才创建的swap文件：

```shell
/swapfile   swap    swap    sw  0   0
```

### swap配置

#### Swappiness

`swappiness`参数决定了系统将数据从内存交换到swap空间的频率，数值设置在0到100之间，代表系统将数据从内存交换到swap空间的力度。

该数值越接近于0，系统越倾向于不进行swap，仅在必要的时候进行swap操作。由于swap要比内存慢很多，因此减少对swap的依赖意味着更高的系统性能。

该数值越接近于100，系统越倾向于多进行swap。有些应用的内存使用习惯更适合于这种情况，这也于服务器的用途有关。

输入如下命令查看当前的swappiness数值：

```shell
cat /proc/sys/vm/swappiness
0
```

使用`sysctl`命令可以修改swappiness。比如将swappiness设为10：

```shell
sudo sysctl vm.swappiness=10
```

本次修改将一直生效到下次重启前。如果希望永久修改该数值，则需要编辑`sysctl`配置文件：

```shell
sudo nano /etc/sysctl.conf
```

将以下内容粘贴到文件末尾：

```shell
vm.swappiness = 10
```





## 常用命令

``df -Th`` #查看驱动器

``getconf LONG_BIT`` #查看系统位数

`arch` #查看系统

### 网络测速

speedtest-cli是一个用Python编写的轻量级Linux命令行工具，在Python2.4至3.4版本下均可运行。它基于Speedtest.net的基础架构来测量网络的上/下行速率。安装speedtest-cli很简单——只需要下载其Python脚本文件。

```shell
wget [https://raw.githubusercontent.com/sivel/speedtest-cli/master/speedtest.py]

chmod +rx speedtest.py

sudo mv speedtest.py /usr/local/bin/speedtest-cli

sudo chown root:root /usr/local/bin/speedtest-cli

speedtest.py
```





