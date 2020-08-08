# bash_command

盡量紀錄有使用過的 bash 操作。

<!-- TOC -->

- [bash_command](#bash_command)
  - [apt](#apt)
  - [Others](#others)
  - [su 最高權限者](#su-最高權限者)
  - [Linux 系統支援語系](#linux-系統支援語系)
  - [顯示目錄下-檔案-編碼-結尾換行符號](#顯示目錄下-檔案-編碼-結尾換行符號)
  - [權限](#權限)
  - [View History](#view-history)
    - [window](#window)
  - [df](#df)
  - [END](#end)

<!-- /TOC -->

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

---

## Others

- `whoami`: 顯示使用者名稱。  <br>
- `hostname`: 顯示主機名稱。  <br>
- `ifconfig`: 查詢、設定網路卡與 IP 網域等相關參數。觀察所有的網路介面。用來獲取網路介面配置資訊並對此進行修改。。

---

## su 最高權限者

-i, --login: run login shell as the target user; a command may also be specified

```{bash}
//下面兩個效果相同。
sudo su
sudo -i
```

---

## Linux 系統支援語系

- Linux 系統預設支援的語系資料：這與 /etc/locale.conf 有關
- 終端介面 (bash) 的語系： 這與 LANG, LC_ALL 這幾個變數有關

---

## 顯示目錄下-檔案-編碼-結尾換行符號

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

## 權限

### 檔案

- [鳥哥的 Linux 私房菜 -- 第五章、Linux 的檔案權限與目錄配置](http://linux.vbird.org/linux_basic/0210filepermission.php)
- -R, --recursive        change files and directories recursively

Linux檔案的基本權限就有九個，分別是 owner/group/others 三種身份各有自己的read/write/execute權限。  <br>
檔案的權限字元為: 『-rwxrwxrwx』，這九個權限是三個三個一組的!  <br>
其中，我們可以使用數字來代表各個權限，各權限的分數對照表: r:4, w:2, x:1。  <br>
每種身份(owner/group/others)各自的三個權限(r/w/x)分數是需要累加的。

還有一個改變權限的方法，基本上就九個權限分別是 user(owner)/group/others 三種身份。  <br>
那麼我們就可以藉由 u, g, o 來代表三種身份的權限，a則代表 all 亦即全部的身份。  <br>
讀、寫、執行的權限就可以寫成r, w, x。

![permission](./image/permission.jpg)

```{bash}
chmod a+x <filename or folder>

//下面兩個語法等價。
chmod a+rwx <filename or folder>
chmod 777 <filename or folder>
```

--

### 目錄之擁有者或群組

- [Linux 更改檔案擁有者與群組，chown 指令使用教學與範例 - G. T. Wang](https://blog.gtwang.org/linux/linux-chown-command-tutorial/)
- -R, --recursive        change files and directories recursively

```{bash}
chown -R ubuntu /data2/bigobject
chown -R ubuntu:ubuntu /data2/bigobject
```

---

## View History

- ubuntu帳號下的指令紀錄放在 /home/ubuntu/.bash_history 中。
- echo $HISTFILE，可以得到上面的路徑。操作歷史紀錄，儲存的檔案位置。(操作歷史紀錄檔)(.bash_history)。
- echo $HISTFILESIZE，操作歷史紀錄檔，最多儲存幾筆。
- echo $HISTSIZE，history 最多列出幾筆(在記憶體中存放的筆數)。

更多設定與操作請參考: [XYZ的筆記本: Linux Bash 刪除 history 指令操作歷史紀錄](https://xyz.cinc.biz/2017/08/linux-bash-history-clear.html)

### window

關於 windows 的 cmd 歷史紀錄，  <br>
請按 F7，或是指令 `doskey /history`

---

## df

磁碟使用的初始狀況。

-h, --human-readable  print sizes in powers of 1024 (e.g., 1023M)

---

## END

應該要塞到權限的section

- [Linux 更改檔案擁有者與群組，chown 指令使用教學與範例 - G. T. Wang](https://blog.gtwang.org/linux/linux-chown-command-tutorial/)

chown -R ubuntu /data2/bigobject
