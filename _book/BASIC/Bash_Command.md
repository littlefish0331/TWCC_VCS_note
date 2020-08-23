# Bash Command

紀錄 BASIC 資料夾中，所以做過的操作，大部分是 bash command 的指令。  
有點類似屬於我的 cheatsheet，為了簡潔，所以會把原有小節就有的 reference 都先去除。

> - 有些比較小的指令，不足以成為一個小節的，我會直接塞在這邊XD~  
> - 從小節複製過來的內容，基本上只會做刪減，沒有新增。

--

**Content:**

<!-- TOC -->

- [Bash Command](#bash-command)
  - [apt](#apt)
  - [Others](#others)
    - [su 最高權限者](#su-最高權限者)
    - [Linux 系統支援語系](#linux-系統支援語系)
    - [顯示目錄下-檔案-編碼-結尾換行符號](#顯示目錄下-檔案-編碼-結尾換行符號)
  - [Permission](#permission)
  - [View History](#view-history)
    - [window](#window)
  - [硬碟-使用狀況、掛載列表、格式化、掛載、卸除、開啟自動掛載](#硬碟-使用狀況掛載列表格式化掛載卸除開啟自動掛載)
  - [SSH Connect](#ssh-connect)
  - [TWCC-Port Connect](#twcc-port-connect)
  - [Environment Variable](#environment-variable)
  - [VPN](#vpn)
  - [END](#end)

<!-- /TOC -->

---

## apt

**list:**

列出所有的套件。  <br>
list packages based on package names.

```{bash}
// 列出可安裝的套件，共 67371 個。
apt list
apt list | wc -l

// 列出有安裝的套件，並計算個數，共 516 個。
apt list --installed | wc -l

// 列出 dcoker 開頭的套件。
apt list | grep ^docker
```

--

**更新:**

```{bash}
apt-get update
```

--

**安裝vim:**

```{bash}
// 一定要先更新 apt-get 這個工具，不然後面其實會出問題。
apt-get update

// 安裝
apt-get install vim
```

---

## Others

- `whoami`: 顯示使用者名稱。  <br>
- `hostname`: 顯示主機名稱。  <br>
- `ifconfig`: 查詢、設定網路卡與 IP 網域等相關參數。觀察所有的網路介面。用來獲取網路介面配置資訊並對此進行修改。

--

### su 最高權限者

-i, --login: run login shell as the target user; a command may also be specified

```{bash}
//下面兩個效果相同。
sudo su
sudo -i
```

--

### Linux 系統支援語系

- Linux 系統預設支援的語系資料：這與 /etc/locale.conf 有關。
- 終端介面 (bash) 的語系： 與 LANG, LC_ALL 這幾個變數有關。

--

### 顯示目錄下-檔案-編碼-結尾換行符號

```{bash}
ls
ll
ls -al  <-- 和ll功能相同

  > 可以在後面加路徑，比如 ll /mnt/e/
```

顯示該檔案的編碼與結尾換行符號類型。

```{bash}
file <filename>
```

---

## Permission

**檔案:**

- -R, --recursive        change files and directories recursively

```{bash}
chmod a+x <filename or folder>

//下面兩個語法等價。
chmod a+rwx <filename or folder>
chmod 777 <filename or folder>
```

--

**目錄之擁有者或群組:**

- -R, --recursive        change files and directories recursively

```{bash}
chown -R ubuntu /data2/bigobject
chown -R ubuntu:ubuntu /data2/bigobject
```

---

## View History

- ubuntu 帳號下的指令紀錄放在 /home/ubuntu/.bash_history 中。
- echo $HISTFILE，可以得到上面的路徑。操作歷史紀錄，儲存的檔案位置。(操作歷史紀錄檔)(.bash_history)。
- echo $HISTFILESIZE，操作歷史紀錄檔，最多儲存幾筆。
- echo $HISTSIZE，history 最多列出幾筆(在記憶體中存放的筆數)。

更多設定與操作請參考: [XYZ的筆記本: Linux Bash 刪除 history 指令操作歷史紀錄](https://xyz.cinc.biz/2017/08/linux-bash-history-clear.html)

### window

關於 windows 的 cmd 歷史紀錄，  <br>
請按 F7，或是指令 `doskey /history`。

---

## 硬碟-使用狀況、掛載列表、格式化、掛載、卸除、開啟自動掛載

==**注意這邊大部分的操作都需要 sudo su。**==

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

## SSH Connect

但是有些人不習慣使用金鑰的方式做登入，那要怎麼把 SSH 的設定做修改呢?  <br>
首先要先建立帳號。

> 不建立新帳號的方法也有，但是比較複雜也麻煩。  
> 二來不同的登入方式使用不同帳號做管理，在資安上也比較合理。

```{bash}
sudo su  --切換到超級使用者。
adduser <new_username>  --新增要用密碼登入的使用者名稱。

  > 系統接下來就會指示你要輸入使用者名稱、密碼、fullname...等等。  
  > 使用者名稱、密碼最重要，其他直接按 Enter 就可以了(會使用預設的)。
```

```{bash}
sudo vim /etc/sudoers  --修改文件

  > 找 「# User privilege specification」的區塊
  > root    ALL=(ALL:ALL) ALL  <--原本就有的。
  > <new_username> ALL=(ALL:ALL) ALL  <--新增這一行。
```

```{bash}
sudo vim /etc/ssh/sshd_config  --修改 ssh 服務的設定。

  > 將 PasswordAuthentication no 改為 yes。  <br>
  > PasswordAuthentication yes
```

```{bash}
service sshd restart  --重啟sshd服務。
```

--

設定完成後，就可以利用 cmd 做帳號密碼登入的連線了!!

```{bash}
ssh -V  --確認有無 ssh 服務
ssh new_username@hostip

  > 按照指示輸入密碼。
```

也可利用 PuTTY.exe 連線。

```{bash}
putty.exe -ssh new_username@hostip -pw password
或是
start putty.exe -ssh new_username@hostip -pw password
```

---

## TWCC-Port Connect

**CIDR設定:**

如果要限制IP連線，可如下設定。以下舉例。

- 111.222.333.444/32: 表示限制只能IP為 111.222.333.444。  <br>
- 111.222.333.444/24: 表示限制IP可為以下網域 111.222.333.0。  <br>
- 111.222.333.444/16: 表示限制IP可為以下網域 111.222.0.0。  <br>
- 111.222.333.444/8: 表示限制IP可為以下網域 111.0.0.0。  <br>
- 111.222.333.444/0: 表示限制IP可為以下網域 0.0.0.0。其實就是大家都可以連進來。  <br>
- 0.0.0.0/0: 表示大家都可以連進來。(效果同上)

通常全開我們都會笑稱是在網路上裸奔，是很危險的，因為現在網路上有很多掃 ip 攻擊。

---

## Environment Variable

**檢視環境變數:**

```{bash}
env
printenv

// 要看哪一個環境變數。
// env <environment_variable_name>
env PATH
```

**新增環境變數:**

only for this time.

```{bash}
// export <env_variable>=<env_value>
export testenv=littlefish
env
```

**permanently:**

大概就是直接去文件新增 $HOME/.bashrc、/etc/environment (more globally)。

---

## VPN

**安裝與設定 openfortivpn:**

```{bash}
sudo su
apt-get update
apt-get install openfortivpn
apt-get install ppp // 這個記得也要安裝。
```

接著去 openfortivpn 那邊做設定，不要讓它成為預設路由，  <br>
否則一啟動 openfortivpn，VM 的遠端連線就會立馬斷線。(因為你的 VPN 改變了阿XDD~)

```{bash}
sudo su
vim /etc/openfortivpn/config
```

這邊以國網中心為例，  
參數設定 host = 140.110.22.100 也可以。

如果不知道 trusted-cert 要填甚麼，可以先不填寫，  <br>
等等連線後會自然報錯，然後跟你說要加上這一段。

> script 註解是用 `#`，但是 markdown 會被 header 語法影響，所以我才用 `//`。

```{openfortivpn config}
// Fortinet VPN 伺服器
host = vpn-hc.nchc.org.tw
port = 443

// 使用者名稱與密碼
username =
password =

// 不要使用預設路由
set-routes = 0

// 信任指定憑證
trusted-cert = fcf8010a3c12408e44fc3d949843b9fb16b46f26dcc5d179beff8f9600541bb5
```

--

**啟動 openfortivpn:**

`&`是指在背景執行。  <br>
要等它一陣子，連線需要時間。

```{bash}
sudo su
openfortivpn &
```

**view vpn:**

```{bash}
// 可以用 fg 觀看背景執行的程式。
// 按下Ctrl+z可跳出。  <br>
fg

    > Ctrl+z

// 類似 ps -l 的指令，可以看目前有哪些程序正在執行。
ps aux | grep fort
```

--

**手動查找要連的服務主機IP並加入:**

但是我們剛剛設定它不是預設路由，所以要和 openfortivpn 說哪一個網站(IP)連線進來要走這條路。

```{bash}
nslookup gitlab.nchc.org.tw
```

**add to vpn route:**

```{bash}
sudo su
route add -host 140.110.144.105/32 dev ppp0
```

--

**gitconfig設定:**

會生成一個 .gitconfig 的設定檔。  <br>
不同使用者，系統建立 .gitconfig 的路徑不相同。

- ubuntu 使用者: /home/ubuntu/
- root 使用者: /root

```{git}
// 環境變數一次性使用。
// 讓 Git Client 忽略 SSL 憑證檢查。
export GIT_SSL_NO_VERIFY=1
```

```{git}
// 設定 .gitconfig 檔。
// 設定讓 Git Client 永久忽略 SSL 憑證檢查。
git config --global http.sslverify false
```

--

**切斷 VPN 連線:**

有兩種做法，我自己推薦用 pkill的方式。

```{bash}
// fg 方法有用僅限於你過程中沒有 exit。
// 因為每次登入會視為一個新的背景。(我目前的理解啦)
fg

    > Ctrl+c
```

```{bash}
sudo su
pkill openfortivpn
```

---

**設定好之後的流程:**

以上設定都完成，就可以成功 git clone 中心的 gitlab 專案囉!!  
以下程式碼，完整地走一遍流程。

```{bash}
// 切換 root 使用者
// VPN 連線。
// 設定特定網域使用 VPN 連線。
// 登出 root 使用者。
sudo su
openfortivpn &
route add -host 140.110.144.105/32 dev ppp0
exit

// 下載公司資源。
git clone https://gitlab.nchc.org.tw/cp4d/das_tutorial_fish.git

// 切換 root 使用者
// 切斷 VPN 連線
// 登出 root 使用者。
sudo su
pkill openfortivpn
exit
```

---

## END
