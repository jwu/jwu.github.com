---
layout: default
title: Terminal
---

# Terminal

## Useful Commands 

### System Control

```bash
reboot
poweroff
```

### Disk

```bash
# check disk size
df -h

# check the directory size under current path.
du -h

# check partitions
cat /proc/partitions
lshw -C disk

# partition disk
fdisk /dev/sda
 : n
 : w

# gpt partition disk (fdisk do not support gpt)
parted /dev/sda
 : mklabel gpt
 : unit mb
 : p              <-- looks for size of drive in MB
 : mkpart primary 0 <insert size of drive in MB>
 : quit

# format disk (after you use fdisk partition the hardware disk)
mkfs.ext4 /dev/sda1

# mount device
mkdir /mnt/db
mount /dev/sda1 /mnt/db

#unmount drive
umount /mnt/db
```

### Raid

```bash
# check the raid device status
cat /proc/scsi/rr26xx/6
```

### FTP

```bash
# login
lftp user:passward@server-name

# upload file
put filepath

# upload directory
mirror -R dirpath
```

### Symbolic Link

```bash
# symlink /foo/bar/ as foobar                                            
ln -s /foo/bar/ foobar                                                   
                                                                         
# list all files, the symlinked file will display as /foo/bar/ -> foobar 
ls -la                                                                   
                                                                         
# delete a symlinked file.                                               
unlink foobar                                                            
# or you can use rm,                                                     
# NOTE: don't use rm foobar/                                             
rm foobar                                                                
```

### Show Current Directory Path

```bash
# show current directory path
pwd

# get current directory value
$PWD

# copy current directory to clipboard
pwd | pbcopy # NOTE: you can use pbpaste to paste content from clipboard
```

### Compress/Uncompress files

```bash
# compress to .tar
tar -cvf file.tar inputfile1 inputfile2

# uncompress .tar
tar -xvf file.tar

# compress to .tar.gz
tar -cvzf file.tar.gz inputfile1 inputfile2

# uncompress .tar.gz
tar -xvzf file.tar.gz
```

### Synchronize Two Folders

```bash
rsync --delete -alzvv SRC_FOLDER TARGET_FOLDER
rsync --delete -alzvv $HOME/.kde/ /mnt/backups/kde
rsync --delete -alzvv $HOME/Mail/ /mnt/backups/mail
```

### Package install

```bash
# install redhat pacakge
rpm -i foobar.rpm

# convert rpm to deb
alien foobar.rpm

# install debian pacakge
dpkg -i foobar.deb
```
