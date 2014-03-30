---
layout: default
title: ex-server Config
---

# ex-server Config

## Tools

  * dnsmasq
  * samba

## GitLab 

  * http://blog.compunet.co.za/gitlab-installation-on-ubuntu-server-12-04/
  * https://github.com/gitlabhq/gitlabhq/blob/stable/doc/installation.md
  * https://github.com/atomic-penguin/cookbook-gitlab (The correct default password)
  * http://hutusi.com/2012/05/28/install-gitlab-on-self-server.html

## openssh-server

### install

```bash
sudo apt-get install openssh-server
```

### Setup LAN Access

Edit `/etc/ssh/sshd_config`

```
PermitRootLogin No
```

Edit `/etc/hosts.allow`

```
sshd:10.0.0.8:allow
```

Edit `/etc/hosts.deny`

```
sshd:all:deny
```

At the end, type:

```bash
sudo /etc/init.d/ssh restart
```

### Setup ssh-key Access

用 SSH +  憑證 ( Private  / Public Key ) 是更安全的連線，可以有效限制未經上傳 Public Key 的主機透過 SSH 來做侵入的動作，
以下就是用 Ubuntu 透過 SSH +  憑證 ( Private  / Public Key ) 連線前的設定哩 ! ( 就是如何產生憑證 ( Private / Public Key )
以及怎麼把這台電腦的 Public Key 上傳到要連線的主機上。

1. 在本區電腦上執行

    ```bash
    ssh-keygen
    ```

    不輸入存檔路徑時，會把檔案放在家目錄的 .ssh 隱藏資料夾裡，且自動命名為 id_ras 
    passphrase 可以不輸入，若有輸入 passphrase 在用憑證登入後，就需要輸入這一組 passphrase。

2. 上傳憑證檔

    執行下列指令:

    ```bash
    scp ~/.ssh/id_rsa.pub xyz@xxx.com.tw:~/.ssh/uploaded_key.pub
    ssh xyz@xxx.com.tw "echo `cat ~/.ssh/id_rsa.pub` >> ~/.ssh/authorized_keys"
    ```

    xyz 是遠端主機的使用者 `scp ~/.ssh/id_rsa.pub user@hostname.com:~/.ssh/uploaded_key.pub`
    xxx.com.tw 是遠端主機的位址

    這樣檔案會被放到遠端主機的 `/home/xyz/.ssh` 下。

3. 調整 authorized_keys 檔權限

    需要調整 authorized_keys 檔案的權限，不然會出現 Permission denied (publickey) 的錯誤訊息，可以選下列任一執行執行

    chmod 400 /home/xyz/.ssh/authorized_keys
    只有 root 可以修改此檔 

    chmod 600 /home/xyz/.ssh/authorized_keys
    使用者自己可以修改此檔 

4. 取消 SSH 的密碼認證

    vi /etc/ssh/sshd_config > 把 PasswordAuthentication yes 改為 no >  存檔  > reboot 重開

5. SSH 連線

    ssh xyz@xxx.com.tw

    如果有設定 passphrase，這時就會出現要輸入的畫面，不然會會直接登入了。

    Read more: Ubuntu /Linux 如何用 SSH + 憑證 ( Private / Public Key ) 登入遠端主機 ? | 阿舍的隨手記記、隨手寫寫... http://www.arthurtoday.com/2009/11/ssh-linux-client.html#ixzz12XZAjOhn 
    Under Creative Commons License: Attribution No Derivatives

## vsftpd 

### Install

```bash
sudo apt-get install vsftpd
```

### Setup

check exenv project, /etc/vsftp.conf

## Mldonkey server

### Install

```bash
sudo apt-get install mldonkey-server
```

### Setup

1. check your port status: webgui -> help -> porttest.  

    You can make your server as DMZ or just forward the ED2K_port in route.

2. edit the ~/.mldonkey/download.ini file, go to web_infos: 

    Change `Server.met`:

    ```bash
    ("server.met", 0, "https://www.emule.org.cn/server.met");
    ```

    remove guarding.p2p line to prevent some server been baned.

3. add kad nodes. 

    downloading kad node from http://www.emule-inside.net/nodes.dat or http://renololo1.free.fr/e/nodes.dat, 
    copy the file to `~/.mldonkey/web_infos/`, in web-gui type the command in input column: 

    ```bash
    kad_load /home/your_name/.mldonkey/web_infos/nodes.dat
    ```

### Let it start up on booting

The mldonkey-server use path `/var/lib/mldonkey` as its default setting directory. We can't change path to something like ~/,
mldonkey since when system boot up, the user may not login yet, and it will be failed when execute the script in `/etc/init.d` 

**Note:** the CONFIG file of mldonkey-server is store in `/etc/default/mldonkey-server`.

## Rocket-Raid 2640x4

  * official site: http://www.highpoint-tech.com/USA_new/series_rr2600.htm
  * Ubuntu Rocket Raid Help: https://help.ubuntu.com/community/RocketRaid

### Compile From Source

When kernel upgrade, you need to compile the driver from source.

```bash
cd ~/backup/rr2640-linux-src-v1.2/product/rr2640/linux
make
sudo make install
```

### Install Steps

标题: 手动安装HighPoint !RocketRaid 2640x4阵列卡驱动程序

在我安装2640x4卡的时候，官方的debian驱动包还没有出来，只有源码，所以只好自已手动从源码开始安装驱动程序了。我们需把阵列卡驱动编译成内核模块的形式，在系统启动时加载。首先，下载官方驱程源码，在2.6.18-486内核的debian环境下编译，生成一个2.6.18-486版的驱动内核模块。接着在2.6.18-6-686内核的debian系统下再编译生成一个2.6.18-6-686版的驱动内核模块。为什么我要生成两个版本的内核驱动模块呢？这是因为我安装的debian 40r5系统在安装时使用的是2.6.18-486内核，安装完成后的系统内核是为2.6.18-6-686。 而加载的内核模块要与系统的内核匹配才能正常加载，所以我们需要两个内核版本的驱动模块，一个在安装时使用，一个供安装后的新系统使用。如果我们要加载的内核模块与系统的内核版本不一致，则在加载内核模块时会出错，并提示无效格式出错信息。这两个版本的内核模块的使用时机下面会详细介绍。

好了，准备好驱动内核模块后，就可以开始安装了。安装前期的过程就不细讲了，本节内容主要讲述阵列卡的安装。安装到搜索硬盘的时候会报错，提示找不到可以安装的磁盘介质。这时我们选择返回选项返回到安装菜单，选择运行shell进入shell环境，开始安装阵列卡驱动。把装有驱动程序的软盘放到软驱中，挂载软盘，加载驱程。


```bash
mount /dev/fd0 /floppy
insmod /floppy/rr26xx-2.6.18-486.ko
```

成功加载后，退出shell返回安装菜单。再次选择磁盘探测，这时就可以找到磁盘阵列了。接下来按正常步骤分区和安装系统。所有安装步骤完成后系统会提示要重启电脑，先不要按继续按钮，返回到安装菜单，我们要为新系统安装阵列卡驱动，因为我们前面加载的驱程是临时的，系统重启后就会失效。如果这时重启系统就会出错，电脑会因找不到硬盘而停留在加载根文件系统的地方。

要使系统在启动时加载驱动程序，我们需要把驱动编译成内核模块并放到initrd.img始初化内存盘中。initrd.img 在内核启动时会导入内存，生成一个虚拟的文件系统，由该虚拟的文件系统加载系统启动必须的内核模块。通过把阵列卡的内核驱动模块放进initrd.img就可使系统在加载根文件之前加载阵列驱动模块，使内核能找到阵列卡上的硬盘正常启动系统。有关initrd的工作原理请参考本书相关内容。具体操作步骤如下：

首先进入shell，挂载刚安装了新系统的硬盘，把2.6.18-6-686版的驱动模块拷贝到新系统的/lib/modules/2.6.18-6-686/kernel/drivers/scsi/目录。并进入新系统的chroot环境。

```bash
mount /dev/sda1 /mnt
cp /floopy/rr26xx-2.6.18-6-686.ko /mnt/lib/modules/2.6.18-6-686/kernel/drivers/scsi/.
chroot mnt
```

在2.6内核中可以用mkinitramfs工具来生成initrd.img。mkinitramfs工具的配置信息位于/etc/initramfs-tools目录下，其中/etc/initramfs-tools/modules文件包含需添加进initrd.img文件的内核模块列表。我们把驱动模块名rr26xx添加到该文件中，模块名可用lsmod命令查询得到。

运行depmod -a 2.6.18-6-686更新/lib/modules/2.6.18-6-686/modules.dep文件，该文件定义模块依赖关系，使内核模块能按正确的顺序正确加载。更新modules.dep文件后，查询该文件中可以发现rr26xx模块要依赖scsi_mod模块。

运行以下命令生成新的initrd.img，在生成新的initrd.img文件前最好先备份旧文件。

mkinitramfs -o /boot/initrd.img-2.6.18-6-686 2.6.18-6-686
-o参数设定输出文件名，最后的2.6.18-6-686指定生成初始化内存盘的内核版本号，该版本号要与系统启动的内核版本号一致。

检查/boot/grub/menu.1st文件的启动项中initrd参数指定的文件是否正确。如果没问题就可以重启电脑了。在内核启动时会出现如下阵列卡驱动程序加载的信息。

```
...
Nov 20 18:26:18 t022 kernel: SCSI subsystem initialized
Nov 20 18:26:18 t022 kernel: rr26xx: module license 'Proprietary' taints kernel.
Nov 20 18:26:18 t022 kernel: rr26xx:RocketRAID 26xx controller driver v1.0.08.0402 (Nov 15 2008 00:14:59)
Nov 20 18:26:18 t022 kernel: ACPI: PCI Interrupt 0000:01:00.0[A] -> GSI 16 (level, low) -> IRQ 169
Nov 20 18:26:18 t022 kernel: rr26xx:adapter at PCI 1:0:0, IRQ 169
Nov 20 18:26:18 t022 kernel: rr26xx:Start to probe device 1
Nov 20 18:26:18 t022 kernel: rr26xx:Start to probe device 2
Nov 20 18:26:18 t022 kernel: rr26xx:Start to probe device 3
Nov 20 18:26:18 t022 kernel: scsi0 : rr26xx
Nov 20 18:26:18 t022 kernel: Vendor: HPT Model: DISK_0_0 Rev: 4.00
Nov 20 18:26:18 t022 kernel: Type: Direct-Access ANSI SCSI revision: 00
...
```

highpoint为阵列卡提供了方便的管理程序，在官方网站上有三种管理界面可以选择，分别是web界面，gtk界面和cli命令行界面。我选择了web界面管理方式。安装方法如下，首先到官方网站下载软件包。官方网站上只提供rpm格式的软件包，没有deb包。我们可以用alien程序把rpm软件包转换成deb格式，再用dpkg -i命令安装。具体操作步骤如下：

```
debian:~/inst/hpt2640x4WebGUI# alien --scripts hptsvr-https-1.4-8.i386.rpm
hptsvr-https_1.4-9_i386.deb generated
debian:~/inst/hpt2640x4WebGUI# dpkg -i hptsvr-https_1.4-9_i386.deb
Selecting previously deselected package hptsvr-https.
(Reading database ... 21670 files and directories currently installed.)
Unpacking hptsvr-https (from hptsvr-https_1.4-9_i386.deb) ...
Setting up hptsvr-https (1.4-9) ...
Adding system startup for /etc/init.d/hptdaemon ...
/etc/rc0.d/K20hptdaemon -> ../init.d/hptdaemon
/etc/rc1.d/K20hptdaemon -> ../init.d/hptdaemon
/etc/rc6.d/K20hptdaemon -> ../init.d/hptdaemon
/etc/rc2.d/S20hptdaemon -> ../init.d/hptdaemon
/etc/rc3.d/S20hptdaemon -> ../init.d/hptdaemon
/etc/rc4.d/S20hptdaemon -> ../init.d/hptdaemon
/etc/rc5.d/S20hptdaemon -> ../init.d/hptdaemon
Starting hptsvr daemon.
```

安装完成后，打开浏览器输入https://localhost:7402 就可打开.
