# Initial Status

如果建立一個 ubuntu VM，以下是TWCC預設會先執行的 script，我盡可能註解。  <br>
另外也註記一開始會安裝哪些東西，或是有哪些設定。

<!-- TOC -->

- [Initial Status](#initial-status)
  - [ubuntu script history](#ubuntu-script-history)
    - [.bash_history](#bash_history)
    - [sudo su](#sudo-su)
  - [apt list](#apt-list)
  - [df -h](#df-h)
  - [END](#end)

<!-- /TOC -->

---

## ubuntu script history

### .bash_history

> NTP 服務（ntpd）本身就有自動校時的功能，若啟用 NTP 服務後，就不可以使用 ntpdate 的方式校時，兩者僅能擇一使用。

```{bach}
ls  <br>
sudo vim /etc/ssh/  --對該資料夾vim，則會顯示該資料夾下的所有檔案列表。  <br>
sudo su  --進入超級權限管理者。  <br>
exit  --退出超級權限管理者
ls  <br>
vim /etc/hosts  --顯示該資料夾下的所有檔案列表。  <br>
sudo vim /etc/hosts  --顯示該資料夾下的所有檔案列表。這和網路有相關。  <br>
exit  --退出 ubuntu 這個VM，之後再進來  <br>
sudo vim /etc/hosts  <br>
vim /etc/sysctl.conf  --網路相關的設定，最後面加了三行。(我猜啦)
sudo vim /etc/sysctl.conf

  > net.ipv6.conf.all.disable_ipv6 = 1  <br>
  > net.ipv6.conf.default.disable_ipv6 = 1  <br>
  > net.ipv6.conf.lo.disable_ipv6 = 1

sudo reboot  --重新開機  <br>
sysctl  -- Linux 下的 sysctl 指令可以用作檢視及修改執行中的 Kernel 變數。  <br>
sysctl -a|grep "ipv6\|disable"  --grep列出上述列表符合條件的Kernel變數。  <br>
sysctl -a|grep "ipv6\|disable\| 1"  <br>
sudo vim /etc/apt/sources.list  --apt的resource來源，加上 http://free.nchc.org.tw/ubuntu。  <br>
sudo apt-get install zsh  --安裝zsh模組。不知道下面為何要新安裝卸除好幾次。  <br>
sudo apt-get remove zsh
sudo vim /etc/apt/sources.list
sudo apt-get install zsh
sudo apt-get update
sudo apt-get install zsh
sudo apt-get remove zsh
sudo apt-get install zsh
sudo apt-get remove zsh
exit
date
exit
apt-get install ntp
sudo apt-get install ntp  --安裝ntp、ntpd模組。ntp: Network Time Protoco。提供網路上可以進行網路校時的主機功能。  <br>
sudo apt-get install ntpd  <br>
sudo apt-get install ntp  <br>
sudo apt-get update  <br>
sudo apt-get install ntp  <br>
ls  <br>
vim /etc/apt/sources.list  <br>
exit  <br>
ping free.nchc.org.tw  <br>
vim /etc/apt/sources.list  <br>
sudo vim /etc/apt/sources.list  <br>
sudo apt-get update  <br>
sudo apt-get install ntp  <br>
sudo ntpdate 140.110.16.1  --手動校正時間。  <br>
sudo apt-get install ntpdate  <br>
sudo ntpdate 140.110.16.1  <br>
systemctl stop ntp  --停止自動校正時間。  <br>
sudo systemctl stop ntp  <br>
sudo ntpdate 140.110.16.1  <br>
sudo yum update -y  <br>
tmux  --tmux 終端機管理工具，分割視窗、同時開啟多個終端機。  <br>
exit  <br>
sudo apt-get upgrade -y  <br>
date  <br>
sudo dpkg-reconfigure tzdata  --改變timezone  <br>
date  <br>
vim /etc/ntp.conf  --設定ntp對準時間的相關參數與來源。  <br>
sudo vim /etc/ntp.conf  <br>
sudo systemctl restart ntp  <br>
sudo systemctl status ntp  <br>
vim /etc/hosts  <br>
hostname  --知道本VM的名稱  <br>
vim /etc/hosts  --知道前網路的設定  <br>
sudo vim /etc/hosts  <br>
ntpq -p  --列出目前我們的 NTP 與相關的上層 NTP 的狀態。列印出該伺服器已知的節點列表和它們的狀態概要信息。  <br>
cat /etc/hosts  <br>
sudo vim /etc/apt/sources.list  <br>
sudo su  <br>
cat /etc/apt/sources.list  <br>
exit  <br>
```

### sudo su

> 指令 `systemctl 操作指令 服務名稱.service`
>  <br>
> 在 Systemd 中每一個系統服務就稱為一個服務單位（unit），而服務單位又可以區分為 service、socket、target、path、snapshot、timer 等多種不同的類型（type），我們可以從設定檔的附檔名來判斷該服務單位所屬的類型，最常見的就是以 .service 結尾的系統服務，大部分的伺服器都是屬於這種。  <br>
>  <br>
> 如果要管理 Systemd 中的各種服務，可以使用 systemctl 這個指令，配合各種操作指令來進行各種操作。  <br>

```{bach}
 1  vim /etc/ssh/sshd_config  --修改ssh連線的config。  <br>
 2  systemctl |grep ssh  --列出 ssh 連線這個服務的狀態。  <br>
 3  systemctl restart ssh  --重啟 ssh 連線服務。
 4  exit
 5  apt upgrade
 6  apt-get upgrade
 7  apt-get install zip
 8  apt-get update
 9  vim /etc/apt/sources.list
10  apt-get update
11  exit
12  sudo vi /etc/systemd/system/rc-local.service
13  sudo vi /etc/rc.local
14  sudo chmod +x /etc/rc.local
15  sudo systemctl enable rc-local
16  sudo systemctl start rc-local.service  --重啟服務
17  sudo systemctl status rc-local.service
18  sudo vi /etc/rc.local
19  sudo systemctl status rc-local.service
20  sudo systemctl start rc-local.service
21  sudo systemctl status rc-local.service
22  sudo vi /etc/rc.local
23  sudo systemctl start rc-local.service
24  sudo systemctl status rc-local.service
25  reboot
```

---

## apt

**list:**

列出所有的套件。  <br>
list packages based on package names

```{bash}
apt list
apt list --installed  //列出所有安裝的套件。
```

靈活運用

```{bash}
apt list | wc -l
apt list --installed | wc -l
apt list | grep ^docker  //dcoker 開頭的套件。
```

一開始安裝的套件有 516 個。  <br>
其中並沒有 docker。  <br>
比較重點有的是 vim, zsh, python3.6, perl, openssl, ntp, nano, gzip, git, ftp, gcc-8-base, dpkg, fdisk, curl, apt。

- [apt list - apt 使用筆記](https://foreachsam.github.io/book-util-apt/book/content/command/apt/apt-list/)
- [apt - How to list all installed packages - Ask Ubuntu](https://askubuntu.com/questions/17823/how-to-list-all-installed-packages)

```{bash}
//列出可安裝的套件，共 67371 個。
apt list
apt list | wc -l

//列出有安裝的套件，並計算個數，共 516 個。
apt list --installed | wc -l
```

**更新:**

```{bash}
apt-get update
```

**安裝vim:**

```{bash}
// 一定要先更新 apt-get 這個工具，不然後面其實會出問題。
apt-get update

// 安裝
apt-get install vim
```

---

## df -h

磁碟使用的初始狀況、可以看容量

-h, --human-readable  print sizes in powers of 1024 (e.g., 1023M)

```{bash}
df -h

Filesystem      Size  Used Avail Use% Mounted on
udev            7.9G     0  7.9G   0% /dev
tmpfs           1.6G  740K  1.6G   1% /run
/dev/vda1        97G  3.1G   94G   4% /
tmpfs           7.9G     0  7.9G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           7.9G     0  7.9G   0% /sys/fs/cgroup
/dev/vda15      105M  3.6M  101M   4% /boot/efi
tmpfs           1.6G     0  1.6G   0% /run/user/1000
```

---

## END
