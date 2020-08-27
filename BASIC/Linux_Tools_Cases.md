# Linux Tools Cases

針對一些使用情境，其實就是透過每個簡單的 linux tools 兜起來完成。  
因此這邊就是將這些經驗收集起來。  

可能未來有更好的做法，比如一個指令完成，但是學習靈活運用各個小元件完成工作是很好的訓練。

p.s. 特定主題(如 mount disk, permission)是另外開一個 .md 檔案，也不會收錄到 Linux_tools.md。  
p.s. 也就是這邊只會收錄 Linux_tools.md 指令兜起來的功能。

---

## 情境目錄

- 僅複製資料夾結構

---

## 僅複製資料夾結構




- linux count specific character in each line
- [linux - Count occurrences of character per line/field on Unix - Stack Overflow](https://stackoverflow.com/questions/8629410/count-occurrences-of-character-per-line-field-on-unix)
- [directory - Copy only folders not files? - Ask Ubuntu](https://askubuntu.com/questions/365877/copy-only-folders-not-files)
- https://stackoverflow.com/questions/4073969/copy-folder-structure-without-files-from-one-location-to-another




(4) 複製目錄(只包含資料夾)

- [Copy only folder structure ?](https://www.unix.com/shell-programming-and-scripting/119654-copy-only-folder-structure.html)

根據參考資料，似乎無法使用 cp 作法完成，  
建議使用 `find` + `mkdir -p`(可惜的是mkdir沒辦法複製權限)，  
或使用 `rsync `指令。

先安裝可以看檔案結構的樹狀圖套件 tree。

```{bash}
sudo su
apt-get update
sudo apt install tree
```

到要複製的資料夾中，記得 mkdir 後面那個要用相對路徑。  

- find
  - -type d: 抓出資料夾的部分。  
  - ./*: 避免當前目錄的結果。就是沒有 folderA/. 的結果。可以避免出錯。  
- \; 逃脫字元以及結尾。
- mkdir
  - -p, --parents, no error if existing, make parent directories as needed

建立的路徑其實是

- ../folderC/./ (這個路徑被我用 ./* 篩選掉了。)
- ../folderC/./t1
- ../folderC/./t1/t2

```{bash}
mkdir folderC/
cd folderA/
find -type d // 也可以用 find ./ -type d
find ./* -type d -exec mkdir "../folderC/{}" \;

// 這種做法則更簡潔有力。
find -type d -exec mkdir -p "../folderC/{}" \;
```



find -type d -links 2 -exec mkdir -p "../folderC/{}" \;
find -type d -links 2 | xargs mkdir -p
find -type d | xargs cp -vt ../folderC


find /source/dir -type d -printf "%P\n" | xargs mkdir -p

rsync -avz -e ssh --include '*/' --exclude '*' local_source_dir user@host:/dest_dir

cd source_dir
find . -type d -depth | cpio -dumpl destination_dir




cd /path/to/directories &&
find . -type d -exec mkdir -p -- /path/to/backup/{} \;


find ~/dim_import/* -type f ! -name xdir | xargs -I {} cp {} new/{}





