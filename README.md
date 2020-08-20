# README

<!-- X副對員工這麼壞= =，那公司的產品我還不用爆!  <br>
把資源用到大爆炸的那一種XDD~   -->

記得 VM 剛啟動的時候，建議最好 `sudo reboot` 一下，之前的經驗是有一些檔案好像還處於 lock 狀態。  <br>
跟著目錄(content)慢慢學習，大概可以一步步學習使用 TWCC 的資源。  <br>
docker 的部分就十分建議看官方的 tutorial!!

**密碼不怕看，因為 port 我有鎖國網的網域!!**

> 用VPN連回中心之後，有鎖網域的port依舊不能使用，  <br>
> 後來問過 THomas，是因為新竹的 VPN 似乎不允許這樣操作。

<!-- TOC -->

- [README](#readme)
    - [gitbook設定](#gitbook設定)
    - [專案目標](#專案目標)
    - [END](#end)

<!-- /TOC -->

---

## gitbook設定

抓取專案請用 `https://github.com/littlefish0331/TWCC_VCS_note.git`

> 舊的 repository。  <br>
> 抓取專案請用 `git clone https://github.com/littlefish0331/TWCC_tutorial.git --branch share_note`。--branch 可以用 -b。

基本上除非專案很大，不然不必為了 gitbook 而特別一個分支，將其他不必要文件刪除。  <br>
這樣太耗費工夫，因為基本上專案應該就是以文件為主的專案。

如果真的專案很大，文件說明只是其中一小項，  <br>
那在針對文件資料夾的部分做一個 git repository 的版本管理就好。  <br>
類似外層一個 repository，內部還有自己的 repositroy。

**結尾設定(end-of-line marker):**

- [How do I create a new line without creating a new paragraph? - GitBook Help Center · GitBook (Legacy)](https://legacy.gitbook.com/book/gitbookio/help/discussions/40)
- [regex - sed beginner: changing all occurrences in a folder - Stack Overflow](https://stackoverflow.com/questions/905144/sed-beginner-changing-all-occurrences-in-a-folder)

恩..這問題比較複雜，但是簡單來說就是必須在每個檔案的後面加上 <br>。  <br>
p.s.這和 git config --local core.autocrlf 的設定無關。如果不小心變更到，就到 .git/config 檔案修改就行了。(--local or --golobal or --system)

```{bash}
// 在該資料夾目錄下，輸入下列指令完成。
// 建議是在 wsl(Unix) 環境下執行，但 cmd or powershell 也可以。
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

**上傳設定:**

- [2019-01-06 Github 上傳錯誤， master (push declined due to email privacy restrictions - 簡書](https://www.jianshu.com/p/ae80af8f65e5)

要先去自己 Github 帳號的 E-mail settings，  <br>
把「Block command line pushes that expose my email」這個選項取消掉!!

![github_email_setting_command_line](./image/github_email_setting_command_line.jpg)

或是如參考資料提供的方法，去 E-mail settings > Keep my email addresses private，  <br>
使用 Github 所提供的 Email。

---

## 專案目標

學習如何在VM上架設各種服務、應用程式、網路設定，  <br>
以及如何讓不同服務之間溝通。最後打包成果與個體快照:

- Docker
  - run images
  - Dockerfile - docker compose - .yml
- VM
- program
  - python
  - R, Rstudio
  - JS、CSS、HTML
  - jupyter notebook
  - Julia
  - Grafana
- DB
  - Mysql
  - BigObject
  - SQL Server
  - MariaDB
  - Postgres
- API Server
  - Plumber
  - Flask
- Git
  - Gitbook
  - Gitlab
- FTP
  - FTP Client
  - FTP Server
- Web Service
  - Shiny
- VPN
- Jenkins

---

## END
