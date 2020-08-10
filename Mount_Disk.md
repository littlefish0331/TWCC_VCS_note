# Mount Disk

其實就和在 windows 系統一樣，要新增硬碟，也就是掛上去之前，都是要初始化的。  <br>
然後 linux 的硬碟格式可能和我們熟知的不太一樣。

- windows 推薦 NTFS
- Linux 推薦 ext4

**reference:**

<!-- TOC -->

- [Mount Disk](#mount-disk)
  - [硬碟-掛載狀況、格式化、掛載、卸除](#硬碟-掛載狀況格式化掛載卸除)
  - [分割硬碟](#分割硬碟)
  - [END](#end)

<!-- /TOC -->

---

## 硬碟-掛載狀況、格式化、掛載、卸除

- [用 Linux blkid 命令查找塊設備詳情 - 每日頭條](https://kknews.cc/zh-tw/tech/3m988oo.html)
- [鳥哥的 Linux 私房菜 -- 第七章、Linux 磁碟與檔案系統管理](http://linux.vbird.org/linux_basic/0230filesystem.php#lsblk)
- [替 Linux 新增硬碟（磁碟分割、格式化與掛載） - G. T. Wang](https://blog.gtwang.org/linux/linux-add-format-mount-harddisk/)
- [mount - 掛載儲存裝置指令 | 凍仁的筆記](http://note.drx.tw/2008/02/ubuntu-mount.html)
- [Linux 檔案系統掛載（mount）使用教學與範例 - G. T. Wang](https://blog.gtwang.org/linux/linux-mount/)
- [How to Mount and Unmount Filesystem in Linux - TecAdmin](https://tecadmin.net/mount-and-unmount-filesystem-in-linux/)

注意這邊大部分的操作都需要 sudo su。

**掛載狀況:**

- 磁碟使用的初始狀況。  <br>
- -h, --human-readable: print sizes in powers of 1024 (e.g., 1023M)

```{bash}
df -h
```

- 列出所有(掛載中)磁碟

> 利用 blkid 這個指令，它可以列出所有掛載中磁碟的 UUID。  <br>
> blk: 是指 block device，即儲存裝置。

```{bash}
//只會顯示有掛載的
blkid

//顯示所有硬碟
lsblk
```

**格式化、掛載:** 先格式化，再掛載~

```{bash}
mkfs -t ext4 /dev/vdb
mount -t ext4 /dev/vdb /datamount
df -h
```

卸除磁碟

```{bash}
umount ext4 /dev/vdb
umount ext4 /datamount  //這兩個都可以。
df -h
```

**設定開機自動掛載:**

各欄說明：`<file system> <mount point>   <type> <options> <dump> <pass>`  <br>

- file system：磁碟裝置代號或該裝置的 Label。
- mount point：掛載點。
- type：磁碟分割區的檔案系統。
- options：檔案系統參數。
- dump：能否被 dump 備份指令作用。
- pass：是否以 fsck 檢驗磁區。

```{bash}
vi /etc/fstab

//給一個之前用好的範例結果
# <file system> <mount point>   <type> <options> <dump> <pass>  <--我加的註解。
LABEL=cloudimg-rootfs   /        ext4   defaults        0 0  <--原本就有。
LABEL=UEFI      /boot/efi       vfat    defaults        0 0  <--原本就有。
UUID="67370358-c856-468b-b4d9-452bb3741ec3"     /datamount  ext4    defaults        0       0  <--新加的。
```

---

## 分割硬碟

由於 MBR 分割表不支援超過 2TB 的磁碟，如果您的硬碟大小超過 2TB，就無法使用 fdisk 分割硬碟，請改用 parted 以 GPT 的方式分割。

- [Linux 的 Parted 指令教學：建立、變更與修復磁碟分割區 - G. T. Wang](https://blog.gtwang.org/linux/parted-command-to-create-resize-rescue-linux-disk-partitions/)

詳細步驟我自己還沒有遇到需要做的情境，所以我也不清楚。

---

## END
