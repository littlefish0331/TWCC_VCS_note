# Linux Tools

除了主題式的 mount, permission(chmod, chown) 之外，  
大多數的 linux 指令，其實都是一個一個的小工具，  
利用這些指令，就可以組成強大的功能。

所以本節就是將使用過的 linux 小指令，做一個收集與整理。

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

## ll, ls -al

List information about the FILEs (the current directory by default).  
列出目前目錄下的檔案以及資料夾之訊息。

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

總結來說，cp 可以做到複製目錄、檔案，以及複製後更新名稱。

Copy SOURCE to DEST, or multiple SOURCE(s) to DIRECTORY.

==**cp --help 有簡述的指令說明。man cp 則可以看到更多的解釋**==

--

**reference:**

[Day 14 Linux 複製目錄、檔案或更名cp相關指令 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10159879)

--

### cp常見參數

- -b, like --backup but does not accept an argument
- -f, --force, if an existing destination file cannot be opened, remove it and try again (this optionis ignored when the -n option is also used)
- -i, --interactive, prompt before overwrite (overrides a previous -n option)
- -R, -r, --recursive, copy directories recursively
- -n, --no-clobber, do not overwrite an existing file (overrides a previous -i option)
- -u, --update, copy only when the SOURCE file is newer than the destination file or when the destination file is missing

**來點中文翻譯:**

- -f 複製時檔案或目錄名稱一樣時，直接覆蓋不會警告。為強制(force)的意思，若目標檔案已經存在且無法開啟，則移除後再嘗試一次。
- -i 複製時檔案或目錄名稱一致時，會提出警告。若目標檔(destination)已經存在時，在覆蓋時會先詢問動作的進行(常用)，
- -r, -R 遞迴持續複製，用於目錄的複製行為。複製時包含目錄及目錄下的檔案。
- -p 連同檔案的屬性(權限、用戶、時間)一起複製過去，而非使用預設屬性(備份常用)。
- -s 複製成為符號連結檔 (symbolic link)，亦即『捷徑』檔案。
- -a 相當於 -dr --preserve=all 的意思，至於 dr 請參考下列說明；(常用)
- -l 進行硬式連結(hard link)的連結檔建立，而非複製檔案本身。
- -d 若來源檔為連結檔的屬性(link file)，則複製連結檔屬性而非檔案本身；
- -u destination 比 source 舊才更新 destination，或 destination 不存在的情況下才複製。
- --preserve=all 除了 -p 的權限相關參數外，還加入 SELinux 的屬性, links, xattr 等也複製了。

最後需要注意的，如果來源檔有兩個以上，則最後一個目的檔一定要是『目錄』才行!!  
另外，不同身份者執行這個指令會有不同的結果產生，尤其是那個-a, -p的選項， 對於不同身份來說，差異則非常的大!!  
在預設的條件中， cp 的來源檔與目的檔的權限是不同的，目的檔的擁有者通常會是指令操作者本身。

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

母目錄皆一起複製。  
**如果是複製多個來源，則要先建立好資料夾。**

```{bash}
cp -vr folderA/ folderC/
// 'folderA/' -> 'folderC/'
// 'folderA/fileA2.txt' -> 'folderC/fileA2.txt'
// 'folderA/fileA1.txt' -> 'folderC/fileA1.txt'

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
rm -r folderC/
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

該指令專職進行多個檔名的同時更名，並非針對單一檔名變更，與mv不同。請man rename。

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

-v, -verbose, Verbose: print names of files successfully renamed.
-n, -nono, No action: print names of files to be renamed, but don't rename.
-f, -force, Over write: allow existing files to be over-written.
-h, -help, Help: print SYNOPSIS and OPTIONS.
-m, -man, Manual: print manual page.
-V, -version, Version: show version number.
-e, Expression: code to act on files name. May be repeated to build up code (like "perl -e"). If no -e, the first argument is used as code.
-E, Statement: code to act on files name, as -e but terminated by ';'.

--

### rename 使用情境

(1) 









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

## df

- [Day 15 ubuntu 查詢硬碟使用量df指令 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10160087)

## rsync

- [Linux - How To Copy a Folder [ Command Line Option ] - nixCraft](https://www.cyberciti.biz/faq/copy-folder-linux-command-line/)
- [How to Copy Directory/Folder in Linux via Command Line!](https://www.webservertalk.com/copy-directory-folder-linux-cmd)

---

## END
