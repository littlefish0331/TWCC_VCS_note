# Mount Disk

其實就和在 windows 系統一樣，要新增硬碟。  <br>
而新的硬碟掛上去之前，都是要初始化的，總之就是有一些步驟要做。  <br>
然後 linux 的硬碟格式可能和我們熟知的不太一樣。

- windows 推薦 NTFS
- Linux 推薦 ext4

本節要教學如何在 TWCC-VCS 開啟的 Linux 環境下掛上一個新的硬碟，  <br>
以及如何設定開機就掛載、檢視掛載狀況等等。

--

**Content:**

<!-- TOC -->

- [Mount Disk](#mount-disk)
  - [硬碟-使用狀況、掛載列表、格式化、掛載、卸除、開啟自動掛載](#硬碟-使用狀況掛載列表格式化掛載卸除開啟自動掛載)
  - [分割硬碟](#分割硬碟)
  - [Mount Disk 更改](#mount-disk-更改)
    - [Disk 變換步驟指令](#disk-變換步驟指令)
  - [指定掛載的編碼](#指定掛載的編碼)
  - [END](#end)

<!-- /TOC -->

---

## 硬碟-使用狀況、掛載列表、格式化、掛載、卸除、開啟自動掛載

**reference:**

- [用 Linux blkid 命令查找塊設備詳情 - 每日頭條](https://kknews.cc/zh-tw/tech/3m988oo.html)
- [鳥哥的 Linux 私房菜 -- 第七章、Linux 磁碟與檔案系統管理](http://linux.vbird.org/linux_basic/0230filesystem.php#lsblk)
- [替 Linux 新增硬碟（磁碟分割、格式化與掛載） - G. T. Wang](https://blog.gtwang.org/linux/linux-add-format-mount-harddisk/)
- [mount - 掛載儲存裝置指令 | 凍仁的筆記](http://note.drx.tw/2008/02/ubuntu-mount.html)
- [Linux 檔案系統掛載（mount）使用教學與範例 - G. T. Wang](https://blog.gtwang.org/linux/linux-mount/)
- [How to Mount and Unmount Filesystem in Linux - TecAdmin](https://tecadmin.net/mount-and-unmount-filesystem-in-linux/)

==**注意這邊大部分的操作都需要 sudo su。**==

--

**使用狀況:**

- 磁碟使用的初始狀況。  <br>
- -h, --human-readable: print sizes in powers of 1024 (e.g., 1023M)

```{bash}
df -h
```

**掛載列表:**

- 列出所有(掛載中)磁碟。

> 利用 blkid 這個指令，它可以列出所有掛載中磁碟的 UUID。  <br>
> blk: 是指 block device，即儲存裝置。

```{bash}
//只會顯示有掛載的
blkid

//顯示所有硬碟
lsblk
```

**格式化、掛載:**

先格式化，再掛載~

```{bash}
mkfs -t ext4 /dev/vdb
mount -t ext4 /dev/vdb /datamount
df -h
```

**卸除磁碟:**

可以通過設備名卸除，或是通過掛載點卸除。

- umount
  - -v, --verbose           say what is being done

```{bash}
// 通過設備名卸除
umount -v /dev/vdb

// 通過掛載點卸除
umount -v /datamount

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

資深的工程師都會直接說你有沒有 fstable，菜鳥常常被呼巄聽不懂XD

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

## Mount Disk 更改

**情境:**

比如說今天發現掛的硬碟其實太大了，所以想重新掛小一點的，從 200GB 換成 80GB。  
那步驟要怎麼完成呢?

**步驟:**

- (如果有 container 運行中) docker stop all container。
- (建議) 先做備份，將所有資料複製到 backup_littlefish/。
- unmount old-disk and remove it on TWCC。
- apply New Disk and format it。
- remove mount-folder and create a new one。
- mount New Disk to mount-folder。
- set fstable and reboot。
- copy all data from backup_littlefish/ to mount-folder/。
- docker start all container。

### Disk 變換步驟指令

```{bash}
// 全部在 root 使用者下進行。
sudo su

// (如果有 container 運行中) docker stop all container。
docker stop $(docker ps -a -q)

// (建議) 先做備份，將所有資料複製到 backup_littlefish/。
// -R: copy directories recursively
// -u: copy only when the SOURCE file is newer than the destination file or when the destination file is missing
// -p:  preserve the specified attributes (default:mode,ownership,timestamps)
// -v: explain what is being done
mkdir backup_littlefish/
cp -Rup datamount/. backup_littlefish/

// unmount old-disk and remove it on TWCC。
df -h  // 也可以用 `fdisk  -l`，來得知磁碟路徑。
umount -v /dev/vdb  // `umount -v /datamount` 作用相同。

// apply New Disk and format it。
mkfs -t ext4 /dev/vdb

// remove mount-folder and create a new one。
rm -r datamount/
mkdir datamount

// mount New Disk to mount-folder。
// 有時候掛載完 df -h 沒有出現，這我也不知道為什麼，但是設定開機掛載再 reboot 之後應該要可以成功。
mount -t ext4 /dev/vdb /datamount
df -h

// set fstable and reboot。
blkid
vi /etc/fstab

// copy all data from backup_littlefish/ to mount-folder/。
cp -Rup backup_littlefish/. datamount/

// docker start all container。
docker start $(docker ps -a -q)

// check
docker ps -a
```

---

## 指定掛載的編碼

假設有一個 USB 隨身碟裝置 /dev/sdb，如何將其掛載到 /datamount，且設定其字元編碼方式為 big5？

```{bash}
mount -o iocharset=big5 /dev/sdb /datamount

// 如果要設定編碼為 utf8
mount -o iocharset=utf8 /dev/sdb /datamount
```

---

## END
