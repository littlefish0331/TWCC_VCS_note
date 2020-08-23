# Build Gitbook

記錄我在 VCS 上建立 Gitbook 的過程，以及一些問題要如何克服。  
部分是從 DockerHub > Website > Gitbook 複製過來。

--

**Content:**

<!-- TOC -->

- [Build Gitbook](#build-gitbook)
  - [啟動 container](#啟動-container)
  - [建置相關手冊建議](#建置相關手冊建議)
  - [結尾設定(end-of-line marker)](#結尾設定end-of-line-marker)
  - [上傳設定](#上傳設定)
  - [Github換行(CR/LF)設定](#github換行crlf設定)
  - [END](#end)

<!-- /TOC -->

---

## 啟動 container

- google key word: build a gitbook on docker。
- [fellah/gitbook - Docker Hub](https://hub.docker.com/r/fellah/gitbook/): 有在更新。可以參考Dockerfile。**我最後使用這個。**
- [yanqd0/gitbook - Docker Hub](https://hub.docker.com/r/yanqd0/gitbook/): 很久沒更新，inspire by fellah/gitbook。
- [10,000小時的修煉之路: 【Docker】Ubuntu / gitbook](http://webcache.googleusercontent.com/search?q=cache:3yNCZ36iXKQJ:maxdev.huder.link/2016/02/dockerubuntu-gitbook.html+&cd=10&hl=zh-TW&ct=clnk&gl=tw): 教學在 ubuntu 上建立環境，架設 Gitbook。環境需要 `apt-get install nodejs, npm, nodejs-legacy`，接著可開始安裝 gitbook。

存放 .md 的資料夾要先練立好，並把權限開啟。  
關於權限設計與管理其實我並不是很懂，實際案例經驗有點少，  
所以目前我都是直接權限開最大XD~

> chmod 777 <folder> -R

```{bash}
docker run \
--name FAE_no72_gitbook \
-v /datamount/Gitbook/FAE_no72:/srv/gitbook \
-p 4001:4000 \
-d fellah/gitbook

// 一行指令
docker run --name FAE_no72_gitbook -v /datamount/Gitbook/FAE_no72:/srv/gitbook -p 4001:4000 -d fellah/gitbook
```

---

## 建置相關手冊建議

基本上除非專案很大，不然不必為了 gitbook 而特別設立一個分支，然後將其他不必要文件刪除。  <br>
因誤這樣後續再管理上其實有點麻煩。(但也可能是我 Git 版本不夠管理不夠熟。)  
總之，我想如果是要做成 Gitbook，那專案應該就是以文件為主的專案。

如果真的專案很大，說明 Gitbook 文件只是其中一小項，  <br>
那在針對文件資料夾的部分做一個 git repository 也是一種解決辦法。  <br>
類似外層一個 repository，內部還有自己的 repositroy。

然後注意到在 VCS 上面建立  Gitbook 後，Gitbook 會自動生成一些 html 檔，  
同時因為資料夾的權限設定，所以建議在 VCS 做 git pull 之後，要馬上做 git status 確認一下更新的狀況，  
然後視情況再 git add/commit/push。

---

## 結尾設定(end-of-line marker)

- [How do I create a new line without creating a new paragraph? - GitBook Help Center · GitBook (Legacy)](https://legacy.gitbook.com/book/gitbookio/help/discussions/40)
- [regex - sed beginner: changing all occurrences in a folder - Stack Overflow](https://stackoverflow.com/questions/905144/sed-beginner-changing-all-occurrences-in-a-folder)

問題的起源於，Gitbook 和一般的 markdown 在換行採用的規則不相同，  
有興趣的話，可以去找關鍵字 Soft/Hard line breaks。  
在這邊就不深入探討，只說明解決的辦法。

簡單來說就是必須在每個檔案的後面加上 `<br>`。  <br>

> p.s.這和 git config --local core.autocrlf 的設定無關。  
> 如果不小心變更到這個，就到 .gitconfig 檔案修改就行了。(--local or --golobal or --system)

```{bash}
// 在該資料夾目錄下，輸入下列指令完成。
// 建議是在 wsl(Unix) 環境下執行，但 cmd or powershell 好像也可以。
// 下面兩個指令等價。
sed --in-place 's/  \r$/  <br>\r/g' *.md
sed -i 's/  \r$/  <br>\r/g' *.md

// 保留原始數據，並多加上放檔案名稱.orimd
sed -i.orimd 's/  \r$/  <br>\r/g' *.md
```

```{bash}
// 如果 .md 有在資料夾中，則需要迭代執行。
find ./ -type f -name '*.md' | xargs sed -i 's/  \r$/  <br>\r/g'
```

---

## 上傳設定

- [2019-01-06 Github 上傳錯誤， master (push declined due to email privacy restrictions - 簡書](https://www.jianshu.com/p/ae80af8f65e5)

要先去自己 Github 帳號的 E-mail settings，  <br>
把「Block command line pushes that expose my email」這個選項取消掉!!

![github_email_setting_command_line](./image/github_email_setting_command_line.jpg)

或是依照參考資料提供的方法，去 E-mail settings > Keep my email addresses private，  <br>
使用 Github 所提供的 Email。

---

## Github換行(CR/LF)設定

關鍵字: git config --local core.autocrlf

---

## END
