# Create Disk Partition

## GET Partition ID
$ sudo fdisk -l
```
Disk /dev/sdc: 894.23 GiB, 960163569664 bytes, 1875319472 sectors
Disk model: LOGICAL VOLUME  
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 262144 bytes / 262144 bytes

```

## Show Disk List
$ sudo lsblk
```
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
loop0    7:0    0  99.4M  1 loop /snap/core/11993
loop1    7:1    0  55.4M  1 loop 
loop2    7:2    0 116.6M  1 loop /snap/docker/1125
loop3    7:3    0  38.7M  1 loop /snap/postgresql10/47
loop4    7:4    0  70.3M  1 loop /snap/lxd/21029
loop5    7:5    0  32.3M  1 loop 
loop6    7:6    0  61.9M  1 loop /snap/core20/1270
loop7    7:7    0   3.6M  1 loop /snap/stress-ng/6356
loop8    7:8    0  91.6M  1 loop /snap/kata-containers/1437
loop9    7:9    0    60M  1 loop /snap/prometheus/53
loop10   7:10   0  68.3M  1 loop /snap/powershell/194
loop11   7:11   0  55.5M  1 loop /snap/core18/2253
loop12   7:12   0  43.3M  1 loop /snap/snapd/14295
loop13   7:13   0  67.2M  1 loop /snap/lxd/21835
loop14   7:14   0  55.5M  1 loop /snap/core18/2284
loop15   7:15   0  91.4M  1 loop /snap/kata-containers/1657
loop16   7:16   0  43.4M  1 loop /snap/snapd/14549
loop17   7:17   0 110.5M  1 loop /snap/core/12603
loop18   7:18   0  61.9M  1 loop /snap/core20/1328
sda      8:0    0 447.1G  0 disk 
├─sda1   8:1    0   512M  0 part /boot/efi
└─sda2   8:2    0 446.6G  0 part /
sdb      8:16   0 894.2G  0 disk 
└─sdb1   8:17   0 894.2G  0 part /home
sdc      8:32   0 894.2G  0 disk
```

## Create Partition
1. Enter partition program
$ sudo parted /dev/sdc
```
GNU Parted 3.3
Using /dev/sdc
Welcome to GNU Parted! Type 'help' to view a list of commands.
```
2. Print Info
(parted) print
```
Error: /dev/sdc: unrecognised disk label
Model: HPE LOGICAL VOLUME (scsi)                                          
Disk /dev/sdc: 960GB
Sector size (logical/physical): 512B/4096B
Partition Table: unknown
Disk Flags:
```

3. Make Label
(parted) mklabel msdos 
(parted) print
```
Model: HPE LOGICAL VOLUME (scsi)
Disk /dev/sdc: 960GB
Sector size (logical/physical): 512B/4096B
Partition Table: msdos
Disk Flags: 

Number  Start  End  Size  Type  File system  Flags
```

4. Make Partition ext4
(parted) mkpart primary ext4 0GB 960GB
(parted) print
```
Model: HPE LOGICAL VOLUME (scsi)
Disk /dev/sdc: 960GB
Sector size (logical/physical): 512B/4096B
Partition Table: msdos
Disk Flags: 

Number  Start   End    Size   Type     File system  Flags
 1      1049kB  960GB  960GB  primary  ext4         lba
```

5. Exit
(parted) quit                                                             
Information: You may need to update /etc/fstab.

6. Show new Info
$ sudo lsblk 
```
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
loop0    7:0    0  99.4M  1 loop /snap/core/11993
loop1    7:1    0  55.4M  1 loop 
loop2    7:2    0 116.6M  1 loop /snap/docker/1125
loop3    7:3    0  38.7M  1 loop /snap/postgresql10/47
loop4    7:4    0  70.3M  1 loop /snap/lxd/21029
loop5    7:5    0  32.3M  1 loop 
loop6    7:6    0  61.9M  1 loop /snap/core20/1270
loop7    7:7    0   3.6M  1 loop /snap/stress-ng/6356
loop8    7:8    0  91.6M  1 loop /snap/kata-containers/1437
loop9    7:9    0    60M  1 loop /snap/prometheus/53
loop10   7:10   0  68.3M  1 loop /snap/powershell/194
loop11   7:11   0  55.5M  1 loop /snap/core18/2253
loop12   7:12   0  43.3M  1 loop /snap/snapd/14295
loop13   7:13   0  67.2M  1 loop /snap/lxd/21835
loop14   7:14   0  55.5M  1 loop /snap/core18/2284
loop15   7:15   0  91.4M  1 loop /snap/kata-containers/1657
loop16   7:16   0  43.4M  1 loop /snap/snapd/14549
loop17   7:17   0 110.5M  1 loop /snap/core/12603
loop18   7:18   0  61.9M  1 loop /snap/core20/1328
sda      8:0    0 447.1G  0 disk 
├─sda1   8:1    0   512M  0 part /boot/efi
└─sda2   8:2    0 446.6G  0 part /
sdb      8:16   0 894.2G  0 disk 
└─sdb1   8:17   0 894.2G  0 part /home
sdc      8:32   0 894.2G  0 disk 
└─sdc1   8:33   0 894.2G  0 part 
```

6. Format Created Partition sdc1
$ sudo mkfs.ext4 /dev/sdc1
```
mke2fs 1.45.5 (07-Jan-2020)
Creating filesystem with 234414592 4k blocks and 58605568 inodes
Filesystem UUID: b6934caa-d5cd-4b49-ac1a-8261325b103a
Superblock backups stored on blocks: 
	32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208, 
	4096000, 7962624, 11239424, 20480000, 23887872, 71663616, 78675968, 
	102400000, 214990848

Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (262144 blocks): done
Writing superblocks and filesystem accounting information: done
```

7. Mount New Partition
## Create Mount Point
$ sudo mkdir /backup

## Mount Partition /dev/sdc1 on /backup
$ sudo mount /dev/sdc1 /backup

8. List All Disk Mounted
$ df -hT
```
Filesystem     Type      Size  Used Avail Use% Mounted on
udev           devtmpfs   16G     0   16G   0% /dev
tmpfs          tmpfs     3.2G  1.9M  3.2G   1% /run
/dev/sda2      ext4      439G   13G  404G   4% /
tmpfs          tmpfs      16G     0   16G   0% /dev/shm
tmpfs          tmpfs     5.0M     0  5.0M   0% /run/lock
tmpfs          tmpfs      16G     0   16G   0% /sys/fs/cgroup
/dev/loop0     squashfs  100M  100M     0 100% /snap/core/11993
/dev/sda1      vfat      511M  5.3M  506M   2% /boot/efi
/dev/loop2     squashfs  117M  117M     0 100% /snap/docker/1125
/dev/loop3     squashfs   39M   39M     0 100% /snap/postgresql10/47
/dev/loop4     squashfs   71M   71M     0 100% /snap/lxd/21029
/dev/loop6     squashfs   62M   62M     0 100% /snap/core20/1270
/dev/loop7     squashfs  3.7M  3.7M     0 100% /snap/stress-ng/6356
/dev/loop8     squashfs   92M   92M     0 100% /snap/kata-containers/1437
/dev/sdb1      ext4      880G   77M  835G   1% /home
/dev/loop9     squashfs   60M   60M     0 100% /snap/prometheus/53
/dev/loop10    squashfs   69M   69M     0 100% /snap/powershell/194
/dev/loop11    squashfs   56M   56M     0 100% /snap/core18/2253
/dev/loop12    squashfs   44M   44M     0 100% /snap/snapd/14295
/dev/loop13    squashfs   68M   68M     0 100% /snap/lxd/21835
/dev/loop14    squashfs   56M   56M     0 100% /snap/core18/2284
/dev/loop15    squashfs   92M   92M     0 100% /snap/kata-containers/1657
/dev/loop16    squashfs   44M   44M     0 100% /snap/snapd/14549
/dev/loop17    squashfs  111M  111M     0 100% /snap/core/12603
/dev/loop18    squashfs   62M   62M     0 100% /snap/core20/1328
tmpfs          tmpfs     3.2G     0  3.2G   0% /run/user/1000
/dev/sdc1      ext4      880G   77M  835G   1% /backup
```

9. Mount on boot
$ sudo nano /etc/fstab

Add the following entry
```
.....
UUID=b6934caa-d5cd-4b49-ac1a-8261325b103a   /backup    ext4          nodev,nosuid       0       3
```
Save and exit

$ sudo mount -av

10. Done - Reboot to test 
$ reboot

