---
---

### セットアップ

## LVM

```
$ df -h --total
Filesystem                         Size  Used Avail Use% Mounted on
udev                               7.7G     0  7.7G   0% /dev
tmpfs                              1.6G  1.4M  1.6G   1% /run
/dev/mapper/ubuntu--vg-ubuntu--lv   98G   93G   52M 100% /
tmpfs                              7.8G     0  7.8G   0% /dev/shm
tmpfs                              5.0M     0  5.0M   0% /run/lock
tmpfs                              7.8G     0  7.8G   0% /sys/fs/cgroup
tmpfs                              7.8G     0  7.8G   0% /run/shm
/dev/loop0                          64M   64M     0 100% /snap/core20/1695
/dev/loop1                          62M   62M     0 100% /snap/core20/1611
/dev/sda2                          2.0G  107M  1.7G   6% /boot
/dev/sda1                          1.1G  5.3M  1.1G   1% /boot/efi
/dev/loop2                          47M   47M     0 100% /snap/snapd/16292
/dev/loop4                          48M   48M     0 100% /snap/snapd/17336
/dev/loop3                          68M   68M     0 100% /snap/lxd/22753
tmpfs                              1.6G     0  1.6G   0% /run/user/1000
total                              135G   94G   37G  72% -
```
```
$ lsblk
NAME                      MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
loop0                       7:0    0  63.2M  1 loop /snap/core20/1695
loop1                       7:1    0    62M  1 loop /snap/core20/1611
loop2                       7:2    0    47M  1 loop /snap/snapd/16292
loop3                       7:3    0  67.8M  1 loop /snap/lxd/22753
loop4                       7:4    0    48M  1 loop /snap/snapd/17336
sda                         8:0    0   477G  0 disk 
├─sda1                      8:1    0   1.1G  0 part /boot/efi
├─sda2                      8:2    0     2G  0 part /boot
└─sda3                      8:3    0 473.9G  0 part 
  └─ubuntu--vg-ubuntu--lv 253:0    0   100G  0 lvm  /
```
```
$ sudo vgs
  VG        #PV #LV #SN Attr   VSize    VFree   
  ubuntu-vg   1   1   0 wz--n- <473.89g <373.89g
```
```
$ sudo lvextend -L 200G /dev/ubuntu-vg/ubuntu-lv
> Size of logical volume ubuntu-vg/ubuntu-lv changed from 100.00 GiB (25600 extents) to 200.00 GiB (51200 extents).
> Logical volume ubuntu-vg/ubuntu-lv successfully resized.

$ sudo resize2fs /dev/ubuntu-vg/ubuntu-lv
> resize2fs 1.45.5 (07-Jan-2020)
> Filesystem at /dev/ubuntu-vg/ubuntu-lv is mounted on /; on-line resizing required
> old_desc_blocks = 13, new_desc_blocks = 25
> The filesystem on /dev/ubuntu-vg/ubuntu-lv is now 52428800 (4k) blocks long.
```
```
$ df -h --total
Filesystem                         Size  Used Avail Use% Mounted on
udev                               7.7G     0  7.7G   0% /dev
tmpfs                              1.6G  1.4M  1.6G   1% /run
/dev/mapper/ubuntu--vg-ubuntu--lv  197G   93G   95G  50% /
tmpfs                              7.8G     0  7.8G   0% /dev/shm
tmpfs                              5.0M     0  5.0M   0% /run/lock
tmpfs                              7.8G     0  7.8G   0% /sys/fs/cgroup
tmpfs                              7.8G     0  7.8G   0% /run/shm
/dev/loop0                          64M   64M     0 100% /snap/core20/1695
/dev/loop1                          62M   62M     0 100% /snap/core20/1611
/dev/sda2                          2.0G  107M  1.7G   6% /boot
/dev/sda1                          1.1G  5.3M  1.1G   1% /boot/efi
/dev/loop2                          47M   47M     0 100% /snap/snapd/16292
/dev/loop4                          48M   48M     0 100% /snap/snapd/17336
/dev/loop3                          68M   68M     0 100% /snap/lxd/22753
tmpfs                              1.6G     0  1.6G   0% /run/user/1000
total                              234G   94G  132G  42% -
```