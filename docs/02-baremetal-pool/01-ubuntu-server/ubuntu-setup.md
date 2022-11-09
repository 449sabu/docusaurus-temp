---
---

# セットアップ

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

## 参考
- [【 df 】コマンド――ディスクの空き領域を表示する](https://atmarkit.itmedia.co.jp/ait/articles/1610/24/news017.html)  
- [【 vgs 】コマンド――ボリュームグループの情報を表示する](https://atmarkit.itmedia.co.jp/ait/articles/1908/30/news020.html)  
- [【 lvextend 】コマンド――論理ボリュームのサイズを拡張する](https://atmarkit.itmedia.co.jp/ait/articles/1910/04/news021.html)  
- [第4回　ストレージを柔軟に管理する（LVMの操作1）](https://shell-mag.com/4th_linuxoperations/)  
- [【Linux】Ubuntu20.04でlvmの拡張をする](https://www.myit-service.com/blog/ubuntu-lvm/)  
- [Linuxディスク関連コマンドまとめ](https://qiita.com/aosho235/items/ad9a4764e77ba43c9d76)  
- [Ubuntu 20.04でシステム領域のディスク容量の拡張を行う](https://tech-mmmm.blogspot.com/2020/06/ubuntu-2004.html)  
- [resize2fs - システム管理コマンドの説明](https://kazmax.zpp.jp/cmd/r/resize2fs.8.html#:~:text=resize2fs%20%E3%81%AF%20ext2%20%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%82%B7%E3%82%B9%E3%83%86%E3%83%A0,%E6%96%B0%E3%81%97%E3%81%84%E3%82%B5%E3%82%A4%E3%82%BA%E3%82%92%E6%8C%87%E5%AE%9A%E3%81%99%E3%82%8B%E3%80%82)
