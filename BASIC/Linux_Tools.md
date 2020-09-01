# Linux Tools

除了主題式的 mount, permission(chmod, chown) 之外，  
大多數的 linux 指令，其實都是一個一個的小工具，  
利用這些指令，就可以組成強大的功能。

所以本節就是將使用過的 linux 小指令，做一個收集與整理。

p.s. 主題式的指令學習，另外建立 .md 檔。

--

**收集指令如下:**

有想過要分門別類，但是想一想，如果可以分門別類的時候，再建一個 .md 檔就好。  
之所以現在把這些都整理在此，就是因為他們都是小工具，但其實功能強大很常用到。

關於指令的使用，通常可以用下面兩個指令做功能查詢。

```{bash}
<linux tool> --help
man <linux tool>
```

- wc
- find
- grep
- tr
- sed
- awk
- mkdir
- mv
- cp
- renmae
- cut
- tree
- sort
- uniq

--

**Content:**












---

## others

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

--

### 歷史資訊清除

```{bash}
history -c
history -w
exit
history
```

--

### 重開機

```{bash}
sudo reboot
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

## ll, ls -al

List information about the FILEs (the current directory by default).  
列出目前目錄下的檔案以及資料夾之訊息。

---

## apt

**reference:**

- [apt list - apt 使用筆記](https://foreachsam.github.io/book-util-apt/book/content/command/apt/apt-list/)
- [apt - How to list all installed packages - Ask Ubuntu](https://askubuntu.com/questions/17823/how-to-list-all-installed-packages)

--

### apt 參數: list

**list:**

列出所有的套件。  <br>
list packages based on package names

```{bash}
apt list  //列出所有可安裝的套件。
apt list --installed  //列出所有安裝的套件。
```

### apt 使用情境

```{bash}
// 列出可安裝的套件。
apt list
apt list | wc -l

// 列出有安裝的套件，並計算個數。
apt list --installed | wc -l

// 列出 dcoker 開頭的套件。
apt list | grep ^docker
```

```{bash}
// 列出可安裝的套件。
apt list
apt list | wc -l
```

**更新:**

```{bash}
apt-get update
```

**安裝 vim:**

```{bash}
// 一定要先更新 apt-get 這個工具，不然後面其實會出問題。
apt-get update

// 安裝
apt-get install vim
```

---

## mkdir

Create the DIRECTORY(ies), if they do not already exist.  
建立資料夾。

## 參數: -m, -p, -v

- mkdir: make directory
- -m, --mode=MODE   set file mode (as in chmod), not a=rwx - umask
- -p, --parents     no error if existing, make parent directories as needed
- -v, --verbose     print a message for each created directory

**翻譯:**

- -m: 可以用 chmod mode (e.g.777) 的方式決定檔案權限。
- -p: 檢查要建立的路徑，如果母目錄不存在，就建立一個。
- -v: 顯示建立資訊。

==**注意到 -m 的權限，只有路徑中的最後一個資料夾有效，不然就要把給個路徑都打出來。**==

```{bash}
mkdir -m 777 -v folderA/folderB
mkdir -m 777 -v folderA folderA/folderB
```

---

## mv

Rename SOURCE to DEST, or move SOURCE(s) to DIRECTORY.  
移動 檔案/目錄，其實和 檔案/目錄 重新命名的概念很像。

**資料夾結構:**

- folderA/
  fileA1.txt
  fileA2.txt
- folderB/

==**mv --help 有簡述的指令說明。man mv 則可以看到更多的解釋**==

--

### mv參數: -f, -i, -n, -u, -v, -t, -T

- -f, --force, do not prompt before overwriting
- -i, --interactive, prompt before overwrite
- -n, --no-clobber, do not overwrite an existing file

  > If you specify more than one of -i, -f, -n, only the final one takes effect.
  
- -u, --update                 move only when the SOURCE file is newer than the destination file or when the destination file is missing
- -v, --verbose                explain what is being done
- -t, --target-directory=DIRECTORY  move all SOURCE arguments into DIRECTORY
- -T, --no-target-directory    treat DEST as a normal file

**來點中文翻譯:**

- -f, --force: 如果目標檔案已經存在，不會提出詢問，而會直接強制覆蓋檔案。無條件覆蓋已經存在的檔案。
- -i, --interactive: 如果目標檔案已經存在，會先提出詢問，不會直接強制覆蓋檔案。互動模式。
- -n, –-no-clobber： 不會覆蓋任何檔案。不要覆蓋任何已存在的檔案。
- -u, –-update: 目標檔案已經存在時，會比較來源檔案及目標檔案的時間，如果來源檔案較新才會覆蓋。
- -v, --verbose: 顯示移動資訊。

--

### mv 使用情境

移動的目的地，如果是資料夾，要先建立好才行。

(1) 一般使用: 移動+更名

```{bash}
// 移動 folderA/fileA1.txt 到 folderB/ 之下
// folderA/fileA1.txt 會不見，整個檔案轉移到 folderB/ 之下。  
// 檔案的位置變成 folderB/fileA1.txt
mv <file path> <folder path>
mv folderA/fileA1.txt folderB/

// 移動 fileA1.txt 到 fileA1_new.txt。即更新名稱。
// 所以 folderA/ 底下就只有 fileA1_new.txt 這個檔案了。
mv <file> <new file>
mv fileA1.txt fileA1_new.txt

// 重新命名目錄。  
// 所以 folderA/ 就不見，被改成 folderC/。
mv folderA/ folderC/

// 移動 folderA/fileA1.txt 到 folderB/fileB1.txt。即移動檔案且更新名稱。
// folderA/fileA1.txt 會不見，整個檔案轉移到 folderB/ 之下，且更新名稱為 fileB1.txt。  
// 所以檔案的位置變成 folderB/fileB1.txt
mv <file path> <new file path>
mv folderA/fileA1.txt folderA/fileA1_new.txt

// 移動多個檔案。
// 移動多個檔案就先不考慮改名的問題，所以就是有哪些檔案，要移動到哪個資料夾這樣。  
// 如果有多個來源檔案或目錄，則最後一個目標檔一定是「目錄」!!
// 也就是將所有的資料移動到該目錄的意思！
mv <file1 path> <file2 path> <folder path> ... <folder path>
mv folderA/fileA1.txt folderA/fileA2.txt folderB/ folderC/

// 如果檔案有規律可循的話那麼可以使用正規表達式。  
// 比如，為了移動所有以.txt為副檔名的檔案，我們可以用下面的命令。
mv folderA/*.txt folderB/
```

(2) 參數使用。

**mv -v:**

```{bash}
mv -v folderA/ folderC/
// renamed 'folderA/' -> 'folderC/'

mv -v folderA/fileA1.txt folderB/
// renamed 'folderA/fileA1.txt' -> 'folderB/fileA1.txt'

mv -v folderA/fileA2.txt folderB/fileB2.txt
// renamed 'folderA/fileA2.txt' -> 'folderB/fileB2.txt'
```

將檔案移動到其它位置，而那個位置恰好有同樣名稱的檔案，這時 mv 命令**會覆蓋掉原來的檔案**。  
對於mv的這行指令，一般來說不會有什麼提示。如果想產生一個關於覆蓋檔案的提示，可以使用-i選項。  
(p.s. 通常發行版會通過 alias 命令，將 -i 作為預設選項，所以會有提示。但一般我使用的經驗，就沒有要自己打參數。)  

**mv -i:**

假設我們想將 folderA/fileA1.txt 移動到 folderB/。  
同時，folderB/ 目錄下已經有 fileA1.txt 檔案了。

```{bash}
mv -v -i folderA/fileA1.txt folderB/
// mv: overwrite 'folderB/fileA1.txt'? n
// 這個提示會讓我們知道目標位置處file_1.txt的存在。如果我們按y鍵，那麼那個檔案將會被刪除，否則不會。
```

**mv -u:**

假如 file_1.txt 和 file_2.txt 有如下特點：  
fikeA1.txt has 84 bytes file size and it last modified time is 12:00  
fikeA2.txt has 0 bytes file size and it last modified time is 11:59  

想將它們移動到 folderB/ 目錄下。但是目標地址已經有 fikeA1.txt 和 fikeA2.txt 了。  
用下面的命令將 fikeA1.txt 和 fikeA2.txt 從當前目錄移動到 folderB/。

```{bash}
mv -u folderA/*.txt folderB/
// 可以看到這些檔案被移動了。能移動這些檔案是因為它們最近的修改時間戳比 folderB/ 目錄中的檔案新。
```

**mv -n:**

將 -u 換成 -n 同時加上 -v 選項，會看到沒有任何檔案移動到了 folderB/ 目錄下。

```{bash}
mv -vn *.txt folderB/
```

**mv -b:**

預設情況下，移動檔案將會覆蓋已存在的目標檔案。  
但是如果我們移動錯了檔案而目標檔案已經被新的檔案覆蓋了，這時應該怎麼辦才好呢？  
有沒有一種方法可以恢復之前的檔案呢？  
Well, 我們可以用 -b 選項。該選項會在新檔案覆蓋舊檔案時將舊檔案做備份。

```{bash}
mv -bv folderA/*.txt folderB/fileA1.txt
// renamed 'folderA/fileA1.txt' -> 'folderB/fileA1.txt' (backup: 'folderB/fileA1.txt~')
```

在 folderB/ 目錄下出現了名為 fileA1.txt~。  
那個波浪符號（～）意味著這些檔案是備份檔案。從它們的屬性中我們可以看到，這些檔案比 fileA1.txt 要舊。

**mv -f:**

希望無論如何都覆蓋已經存在的檔案或目錄時，你可以使用 -f 選項。  
如果同時指定了 -f 選項和 -i 或 -n 選項，則 -f 選項會覆蓋它們。即不進行任何提示而覆蓋。  
所以，在使用此引數時，要知道自己在做什麼。

```{bash}
mv -fv folderA/fileA1.txt folderB/fileA2.txt
// renamed 'folderA/fileA1.txt' -> 'folderB/fileA2.txt'
```

---

## cp

cp 的指令可簡單可複雜，在 windows PC 上操作"複製"的動作可能沒有感覺，  
但其實複製一個東西的時候，我們還要注意到權限、複製檔案還是捷徑等問題。  
根據鳥哥的教學，至少有四件事是必須考慮的。

- 是否需要完整的保留來源檔案的資訊？權限問題。
- 來源檔案是否為連結檔 (symbolic link file)? 這個太複雜，先暫時跳過。
- 來源檔是否為特殊的檔案，例如 FIFO, socket 等? 短時間內沒有實際案例，所以先不討論。
- 來源檔是否為目錄? 目錄的話要多一個參數 -r。

總結來說，cp 可以做到複製目錄、檔案，以及複製順便更新名稱。  
本小節只會 cover 最簡單的使用情境。

Copy SOURCE to DEST, or multiple SOURCE(s) to DIRECTORY.

==**cp --help 有簡述的指令說明。man cp 則可以看到更多的解釋**==

如果來源檔有兩個以上，則最後一個目的檔一定要是『目錄』才行!!  
另外，不同身份者執行這個指令會有不同的結果產生，尤其是那個 -p 的選項， 對於不同身份來說，差異則非常的大!!  
在預設的條件中， cp 的來源檔與目的檔的權限是不同的，目的檔的擁有者通常會是指令操作者本身。

--

**reference:**

- [Day 14 Linux 複製目錄、檔案或更名cp相關指令 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10159879)

--

### cp 常見參數

- -b, like --backup but does not accept an argument
- -f, --force, if an existing destination file cannot be opened, remove it and try again (this optionis ignored when the -n option is also used)
- -i, --interactive, prompt before overwrite (overrides a previous -n option)
- -R, -r, --recursive, copy directories recursively
- -n, --no-clobber, do not overwrite an existing file (overrides a previous -i option)
- -u, --update, copy only when the SOURCE file is newer than the destination file or when the destination file is missing
- -p, same as --preserve=mode,ownership,timestamps --preserve[=ATTR_LIST] preserve the specified attributes (default:mode,ownership,timestamps), if possible additional attributes: context, links, xattr, all.

**來點中文翻譯:**

- -f 複製時檔案或目錄名稱一樣時，直接覆蓋不會警告。為強制(force)的意思，若目標檔案已經存在且無法開啟，則移除後再嘗試一次。
- -i 複製時檔案或目錄名稱一致時，會提出警告。若目標檔(destination)已經存在時，在覆蓋時會先詢問動作的進行(常用)，
- -R, -r 遞迴持續複製，用於目錄的複製行為。複製時包含目錄及目錄下的檔案。
- -p 連同檔案的屬性(權限、用戶、時間)一起複製過去，而非使用預設屬性(備份常用)。
- -u destination 比 source 舊才更新 destination，或 destination 不存在的情況下才複製。
- --preserve=all 除了 -p 的權限相關參數外，還加入 SELinux 的屬性, links, xattr 等也複製了。

--

### cp 使用情境

(1) 複製檔案+更改名稱

```{bash}
// 複製檔案
cp folderA/fileA1.txt folderB/

// 更改名稱
cp fileA1.txt newfileA1.txt

// 複製檔案+更改名稱
cp folderA/fileA1.txt folderB/newfileA1.txt

// 複製多個檔案
cp folderA/fileA1.txt folderA/fileA2.txt folderB/
```

(2) 複製目錄(包含下面所有的檔案)

如果目的地的資料夾不存在就是複製，如果目的地資料夾存在就是整個放到目錄下。
母目錄皆一起複製。  
**如果是複製多個來源，則要先建立好資料夾。**

```{bash}
cp -vr folderA folderC
// 'folderA' -> 'folderC'

mkdir folderC/
cp -vr folderA folderC
// 'folderA' -> 'folderC/folderA'

mkdir folderC/
cp -vR folderA/ folderB/ folderC/
// 'folderA/' -> 'folderC/folderA'
// 'folderA/fileA2.txt' -> 'folderC/folderA/fileA2.txt'
// 'folderA/fileA1.txt' -> 'folderC/folderA/fileA1.txt'
// 'folderB/' -> 'folderC/folderB'
// 'folderB/fileB1.txt' -> 'folderC/folderB/fileB1.txt'
```

母目錄不複製。

```{bash}
cp -vR folderA/. folderC/

mkdir folderC/
cp -vR folderA/. folderB/. folderC/
// 'folderA/./fileA2.txt' -> 'folderC/./fileA2.txt'
// 'folderA/./fileA1.txt' -> 'folderC/./fileA1.txt'
// 'folderB/./fileB1.txt' -> 'folderC/./fileB1.txt'
```

(3) 複製特定檔案、複製權限

```{bash}
mkdir folderC/
cp -vr folderA/*.txt folderC/
// 'folderA/fileA1.txt' -> 'folderC/fileA1.txt'
// 'folderA/fileA2.txt' -> 'folderC/fileA2.txt'
```

```{bash}
mkdir folderC/
cp -vrp folderA/. folderC/
// 'folderA/./fileA2.txt' -> 'folderC/./fileA2.txt'
// 'folderA/./fileA1.txt' -> 'folderC/./fileA1.txt'
// 'folderA/./fileA3' -> 'folderC/./fileA3'

ll
// total 16
// drwxrwxr-x  2 ubuntu ubuntu 4096 Aug 26 10:39 ./
// drwxr-xr-x 11 ubuntu ubuntu 4096 Aug 26 10:42 ../
// -rw-rw-r--  1 ubuntu ubuntu   16 Aug 26 09:29 fileA1.txt
// -rw-rw-r--  1 ubuntu ubuntu   16 Aug 26 09:32 fileA2.txt
// -rwxrwxrwx  1 ubuntu ubuntu    0 Aug 26 10:39 fileA3*
```

---

## rename

重新命名資料或是資料夾。

--

**reference:**

- [Linux Script：mv, rename 單次及批次修改檔案名稱 @ 符碼記憶](https://www.ewdna.com/2012/04/linux-scriptmv-rename.html)
- [[Linux] 使用 mv、rename 來替檔案重新命名 - Clay-Technology World](https://clay-atlas.com/blog/2020/04/27/linux-cn-note-use-mv-rename-instruction/)

**安裝:**

```{bash}
// 請先更新 apt
sudo su
apt-get update
exit

// install
sudo su
apt install rename
exit
```

該指令專職進行多個檔名的同時更名，並非針對單一檔名變更，與 mv 不同。請man rename。

--

### rename 參數: -d, -n, -f, -h, -m, -V, -e, -E

-v, -verbose, Verbose: print names of files successfully renamed.
-n, -nono, No action: print names of files to be renamed, but don't rename.
-f, -force, Over write: allow existing files to be over-written.
-h, -help, Help: print SYNOPSIS and OPTIONS.
-m, -man, Manual: print manual page.
-V, -version, Version: show version number.
-e, Expression: code to act on files name. May be repeated to build up code (like "perl -e"). If no -e, the first argument is used as code.
-E, Statement: code to act on files name, as -e but terminated by ';'.

**中文翻譯:**

-v: 顯示執行成功 rename 的檔案。
-n: 顯示要被 rename 的檔案，但是還不執行。
-f: 強制 rename，原本存在的檔案會被複寫。
-h: 和 rename --help 一樣。
-m: 和 man rename 一樣。
-V: 顯示 rename 模組的版本。
-e: rename 的程式碼。
-E: rename 的程式碼，但結尾會有;。

--

### rename 使用情境

用於大量批次改名。

(1) 改名

```{bash}
rename -n 's/A/B/g' *

// 不可以用雙引號。
// 最後面的 g 可以不加，但斜線`/`要保留。
rename -n 's/A/B/g' *.txt
```

---

## wc

## find

## grep

## tr

## sed

## awk

## cut

---

## tree

顯示資料夾的結構。  

**安裝:**

```{bash}
// 請先更新 apt
sudo su
apt-get update
exit

// install
sudo su
apt install tree
exit
```

--

### tree 參數: -d, -D, -c, -a, -f, -L, -n, -C, -u, -g

- -d            List directories only.
- -D            Print the date of last modification or (-c) status change.
- -c            Sort files by last status change time.
- -t            Sort files by last modification time.
- -a            All files are listed.
- -f            Print the full path prefix for each file.
- -L level      Descend only level directories deep.
- -n            Turn colorization off always (-C overrides).
- -C            Turn colorization on always.
- -u            Displays file owner or UID number.
- -g            Displays file group owner or GID number.

**翻譯:**

- -d: 只顯示資料夾。
- -D: 顯示檔案最後修改或是狀態改變的時間。
- -c: 依照狀態改變時間做排序(舊到新)。
- -t: 依照最後修改時間做排序(舊到新)。
- -a: 列出所有檔案，包含隱藏檔案與資料夾。
- -f: 顯示完整路徑。
- -L: 最多顯示到幾層。
- -n: 不要有顏色顯示
- -C: 要有顏色顯示。
- -u: 顯示檔案擁有者或是UID.
- -g: 顯示檔案群組或是UID.

--

### tree 使用情境

(1) 檢視結構

- 一般使用下不會顯示隱藏資料夾。
- 會將底下所有的資料夾全部以樹狀圖的方式展開。

```{bash}
tree
.
├── folderA
│   ├── fileA1.txt
│   ├── fileA2.txt
│   ├── fileA3
│   ├── t0
│   │   └── t1
│   │       └── t2
│   └── t1
│       ├── t1file
│       └── t2

6 directories, 4 files
```

---

## sort

## uniq

## xargs

- [Linux 系統 xargs 指令範例與教學 - G. T. Wang](https://blog.gtwang.org/linux/xargs-command-examples-in-linux-unix/)

## exec

## blkid

## lsblk

## fdisk

## df

磁碟使用的初始狀況、可以看容量

--

**refernce:**

- [Day 15 ubuntu 查詢硬碟使用量df指令 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10160087)

---

## df 使用情境

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

## rsync

- [Linux - How To Copy a Folder [ Command Line Option ] - nixCraft](https://www.cyberciti.biz/faq/copy-folder-linux-command-line/)
- [How to Copy Directory/Folder in Linux via Command Line!](https://www.webservertalk.com/copy-directory-folder-linux-cmd)

---

## END


















# README

Unix 指令，或是精準一點說 bash 指令學習。

- 情境描述
- 目標
- 方法

## tr

**tr 可以想程式 sed 的精簡版。**
所以要作難一點的操作，可以學習指令 sed。

下面簡單介紹了幾個情境，  
還有很多的功用，比如:

- 刪除空行，tr -d '\n'，也可以用tr -s '[\012]'
- 所有英文以外的字元(補集)，轉換為空白，tr -c '[a-z][A-Z]' ' ' < filename > filename2.txt
- 所有英文，轉換為空白，且連續的英文只會有一個空白，tr -s '[a-z][A-Z]' '[ *]' < filename > filename2.txt
- 所有英文以外的字元(補集)，轉換為換行，並排除所有換行，tr -c '[a-z][A-Z]' '\n' < filename | tr -d '\n' > filename2.txt
- 所有英文以外的字元(補集)，轉換為換行，並排除多餘的換行，tr -c '[a-z][A-Z]' '\n' < filename | tr -s '\n' > filename2.txt。可簡寫為tr -cs '[a-z][A-Z]' '\n' < filename > filename2.txt

操作補充:  
輸入資料除了用指令 < filename之外，  
也可以在一開始 cat filename | tr ...，  
直接用 filename是不行的，那樣只會開啟資料而已。

### 情境01

- 情境描述: 因為 windows系統的換行符號為\r\n，但是unix系統的換行符號為\n
- 目標: 利用bash指令，將文字檔中的\r\n轉換為\n，並另存新檔
- 方法: tr -d '\r' < CRLF.txt > LF.txt

處理完之後，可以用 cat 指令稍微看一下兩個檔案。  

- cat CRLF.txt
- cat LF.txt

並用指令 diff 發現兩個檔案確實有差異。

- diff CRLF.txt LF.txt

如果在 Unix 系統下，可再用指令 file 觀看兩個檔案的類型。

- file CRLF.txt，CRLF.txt: ASCII text, with CRLF line terminators
- file LF.txt，LF.txt: ASCII text

### 情境02

- 情境描述: 英文大小寫轉換的轉換
- 目標: 利用bash指令，將文字檔中的[A-Z]轉換為[a-z]，並另存新檔
- 方法: tr '[A-Z]' '[a-z]' < LETTERS.txt > LETTERS_letters.txt

注意，因為檔案名稱不分大小寫，  
所以如果用指令 < LETTERS.txt > letters.txt，  
最後結果會是空白。

另外這方法中文字也行，  
tr '你' '我' < duplicate.txt > duplicate2.txt

### 情境03

- 情境描述: 壓縮重複出現的字元，squeeze-repeats。
- 目標: 利用bash指令，將文字檔中的[A-Z]轉換為[a-z]，並另存新檔
- 方法: tr -s '[A-Z]' '[a-z]' < duplicate.txt > duplicate2.txt

注意，功用是壓縮重複出現的字元，  
所謂重複出現是連續重複出現，和一般所講的 duplicate 有點差異，使用上要小心。

## cls、clear

- windows: 使用指令 cls，清空畫面
- unix: 使用指令 clear，清空畫面。或是利用快捷鍵 Ctrl+L

## copy

### 情境01

- 情境描述: 複製檔案
- 目標: 利用bash指令，將檔案作複製，除了移動到想要的資料夾下之外，也可以另外取名。
- 方法: copy filename1 filename2

> copy copy1.txt copy2.txt

## shift+echo+arg

- 情境描述: 輸入指令變成參數。
- 目標: 利用bash指令，傳送輸入值到參數，如何依序拉出、跳過、指定、全部印出。
- 方法: 下面三種方法存成一個test.bat，各自輸入 test.bat A B C D E，試試看

> set arg1=%1
> set arg2=%2
> echo /first_arg %arg1% /second_arg %arg2% all_args %*
>
> echo %1
> echo %1
> echo %1
> echo %*
>
> echo %1
> shift
> echo %1
> shift
> echo %1
> echo %*
>

## xcopy

[Xcopy - Copy files and folders - Windows CMD - SS64.com](https://ss64.com/nt/xcopy.html)

- 情境描述: 複製檔案。複製資料夾。複製資料夾結構。
- 目標: 利用bash指令，複製整個資料夾，或是複製資料夾結構
- 方法: 將下面的指令跑過一遍，即可初步理解。

> ::預設複製資料夾下的第一層檔案  
> xcopy dir dir_copy  
>
> ::複製資料夾結構  
> xcopy dir dir_copy /T /E

## netstat

尋找使用中的port。  
也可以去 C:\Windows\System32\drivers\etc\services 觀看有使用的port

```{batch}
netstat -ano | findstr 0.0:80
// 得到 TCP    0.0.0.0:80             0.0.0.0:0              LISTENING       3364

tasklist | findstr 3364
// httpd.exe                     3364 Services                   0     17,324 K
```

**reference:**

- [Windows下用cmd命令netstat查看系統端口(PORT)使用情況和刪除對應程式 @ Johnson峰的部落格 :: 痞客邦 ::](https://johnson560.pixnet.net/blog/post/344056561-windows%E4%B8%8B%E7%94%A8cmd%E5%91%BD%E4%BB%A4netstat%E6%9F%A5%E7%9C%8B%E7%B3%BB%E7%B5%B1%E7%AB%AF%E5%8F%A3%28port%29%E4%BD%BF%E7%94%A8)
- [[問題排除] 80PORT被佔用? 如何查出佔用PORT的方法 @ 小笨蛋的工作日誌 :: 隨意窩 Xuite日誌](https://blog.xuite.net/yamalin/blog/15087550-%5B%E5%95%8F%E9%A1%8C%E6%8E%92%E9%99%A4%5D+80PORT%E8%A2%AB%E4%BD%94%E7%94%A8%3F+%E5%A6%82%E4%BD%95%E6%9F%A5%E5%87%BA%E4%BD%94%E7%94%A8PORT%E7%9A%84%E6%96%B9%E6%B3%95)
- [【系】如何查詢哪個程式佔用了指定Port @ 卡達+噗寶=阿噗噗 :: 隨意窩 Xuite日誌](https://blog.xuite.net/cadmus.lin/yo/43386562-%E3%80%90%E7%B3%BB%E3%80%91%E5%A6%82%E4%BD%95%E6%9F%A5%E8%A9%A2%E5%93%AA%E5%80%8B%E7%A8%8B%E5%BC%8F%E4%BD%94%E7%94%A8%E4%BA%86%E6%8C%87%E5%AE%9APort)

## split

```{batch}
// -l: 一些參數設定，包含行數、容量等等。
// -d:是指說檔案名稱的後面要用數字做編號。
// --additional-suffix=.csv: 指定檔案類型。
split -l 9000000 ../bus/bus.csv ../bus/split9000k/split_res -d --additional-suffix=.csv
```

## lscpu

顯示電腦硬體資訊。

## lsb_release -a

顯示作業系統版本與相關的硬體資訊。

## df -h

可以看到硬體資訊以及資源使用量。
