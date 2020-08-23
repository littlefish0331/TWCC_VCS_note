# R_Rstudio

記錄我在 VCS 上操作 container:R+Rstudio 的過程，以及一些問題要如何克服。  <br>
部分是從 DockerHub > Program > R+Rstudio 複製過來。

> 其實完整是包含 R+Rstudio(+python2+python3)

--

**Content:**

<!-- TOC -->

- [R_Rstudio](#r_rstudio)
  - [啟動 container](#啟動-container)
  - [安裝R套件](#安裝r套件)
  - [密碼與登入](#密碼與登入)
    - [修改密碼](#修改密碼)
    - [預設登入或需重新登入](#預設登入或需重新登入)
  - [設定使用者與群組登入:**](#設定使用者與群組登入)
  - [END](#end)

<!-- /TOC -->

--

**reference:**

- [rocker/rstudio Tags - Docker Hub](https://hub.docker.com/r/rocker/rstudio/tags)

---

## 啟動 container

建議下載版本時，找 tag 帶有 ubuntu 的版本。
因為有嘗試過 tag: 3.6.3，結果啟動 container 失敗。  <br>

<!-- PASSWORD=rstudio@2020 -->

```{bash}
// 下載 tag: 3.6.3-ubuntu18.04
// 內部有 python2, python3
docker pull rocker/rstudio:3.6.3-ubuntu18.04

docker run --name rstudio_363ubuntu \
-e ROOT=TRUE \
-e PASSWORD=<password> \
-e ADD=shiny \
-v /datamount/rstudio:/home/rstudio \
-p 3838:3838 -p 8787:8787 \
-d rocker/rstudio:3.6.3-ubuntu18.04

// 下載 tag: latest
docker pull rocker/rstudio

docker run --name rstudio_latest \
-e ROOT=TRUE \
-e PASSWORD=<password> \
-e ADD=shiny \
-v /datamount/rstudio:/home/rstudio \
-p 3838:3838 -p 8787:8787 \
-d rocker/rstudio
```

---

## 安裝R套件

目前套件可以直接安裝。  <br>
如果以後遇到失敗的狀況，可以嘗試下列方法:

- install.packages(pkgs = "package_name", lib = "your_path)
- install.packages(pkgs = "package_name", lib = "your_path, dependencies = T)
- use terminal > R
- use terminal > sudo su > R

---

## 密碼與登入

### 修改密碼

修改密碼的方式很簡單，進到 Rstudio Server 之後，  <br>
上方功能列 > Tools > shell(terminal)。  <br>
輸入 `passwd`。  <br>
然後先輸入舊密碼，接著就可以改密碼了。  <br>

--

### 預設登入或需重新登入

**reference:**

- [How to set an environment variable in a running docker container - Stack Overflow](https://stackoverflow.com/questions/27812548/how-to-set-an-environment-variable-in-a-running-docker-container)
- [How to set an enviroment variable on an existing container? · Issue #8838 · moby/moby](https://github.com/moby/moby/issues/8838)
- [Allow `docker start` to take environment variables · Issue #7561 · moby/moby](https://github.com/moby/moby/issues/7561)

有幾種可能的做法可以嘗試!!  <br>
終究還是要去看一下這個 image 的 dockerfile 是如何撰寫的，  <br>
才知道它是怎麼啟動 Rstudio。

預設是每次需要重新登入，下面的做法可以改成預設登入。

**solution01:** 修改 rserver.conf

在 container 中，路徑 /etc/rstudio/ 之下，有一個檔案叫做 rserver.conf。  <br>
只要在裡面加入下方指令，就可以自動登入。  <br>
但目前在此 image 中嘗試失敗。

```{rserver.conf}
// rserver.conf
# Server Configuration File

rsession-which-r=/usr/local/bin/R
auth-none=1
server-user=rstudio
```

**solution02:** docker run -e DISABLE_AUTH=TRUE

從這個 image 的 Dockerfile，可以知道在路徑 /etc/cont-init.d 下觀看 userconf.conf 這個檔案，  <br>
就可以知道可以更改環境變數 DISABLE_AUTH=TRUE，  <br>
所以就在一開始 docker run 指令時增加參數如下。

```{bash}
docker run \
--name rstudio_363ubuntu \
-e ROOT=TRUE \
-e PASSWORD=<your_password> \
-e ADD=shiny \
-e DISABLE_AUTH=TRUE \
-v /datamount/rstudio:/home/rstudio \
-p 3838:3838 -p 8787:8787 \
-d rocker/rstudio:3.6.3-ubuntu18.04
```

依照 userconf.conf 可以知道會做的事情有

> - 修改 rserver.conf，新增 auth-none=1。
> - 將 USER=rstudio，放到環境變數中。

而這些修改，也可以從 docker logs 指令的方式看到該 container 的 log 紀錄。

--

**conclusion:**

我作了以下三個 container 並記錄其差異。

```{bash}
docker run --name r_env_no -e ROOT=TRUE -e PASSWORD=rstudio@2020 -p 8787:8787 -d rocker/rstudio:3.6.3-ubuntu18.04

docker run --name r_env_TRUE -e ROOT=TRUE -e PASSWORD=rstudio@2020 -e DISABLE_AUTH=TRUE -p 8788:8787 -d rocker/rstudio:3.6.3-ubuntu18.04

  > 依照 userconf.conf 的指令，會多這一行。  <br>
  > Skipping authentication as requested  <br>
  >  <br>
  > 在 /etc/rstudio 目錄之下，缺少 disable_auth_rserver.conf 檔案，  <br>
  > 因為這個檔案的 `auth-none=1` 被寫入 rserver.conf 中。
  >  <br>
  > cat /etc/environment
  > 環境變數增加 `USER=rstudio`。

docker run --name r_env_FALSE -e ROOT=TRUE -e PASSWORD=rstudio@2020 -e DISABLE_AUTH=FALSE -p 8789:8787 -d rocker/rstudio:3.6.3-ubuntu18.04

// 其餘的 logs 如下

  > [s6-init] making user provided files available at /var/run/s6/etc...exited 0.  <br>
  > [s6-init] ensuring user provided files have correct perms...exited 0.  <br>
  > [fix-attrs.d] applying ownership & permissions fixes...  <br>
  > [fix-attrs.d] done.  <br>
  > [cont-init.d] executing container initialization scripts...  <br>
  > [cont-init.d] userconf: executing...  <br>
  > Adding user `rstudio' to group `sudo' ...  <br>
  > Adding user rstudio to group sudo  <br>
  > Done.  <br>
  > rstudio added to sudoers  <br>
  > [cont-init.d] userconf: exited 0.  <br>
  > [cont-init.d] done.  <br>
  > [services.d] starting services  <br>
  > [services.d] done.

// 路徑 /etc/rstudio 目錄底下的資料有

  > drwxr-xr-x 3 root root 4096 Jun 30 06:09 ./
  > drwxr-xr-x 1 root root 4096 Aug 10 08:41 ../
  > -rw-r--r-- 1 root root   75 Jun 30 06:09 disable_auth_rserver.conf
  > -rw-r--r-- 1 root root   19 Jun 30 06:09 file-locks
  > -rw-r--r-- 1 root root   63 Jun 30 06:09 rserver.conf
  > -rw-r--r-- 1 root root   33 Jun 30 06:09 rsession.conf
  > drwxr-xr-x 2 root root 4096 Jun 30 06:09 themes/

// 環境變數 cat /etc/environment

  > PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games"
```

--

如果有下列兩種需求，解決方式如下:

1. 先設定每次都需要登入，後改為預設登入。container:r_env_no, r_env_FLASE  <br>
2. 先設定預設登入，後改為每次都需要登入。container:r_env_TRUE

均先安裝 vim。

```{bash}
apt-get update
apt-get install vim
```

1. 先設定每次都需要登入，後改為預設登入

```{bash}
vim /etc/rstudio/rserver.conf
// 新增 auth-none=1

vim /etc/environment
// 新增 USER=rstudio
```

再 docker restart container 即可。

2. 先設定預設登入，後改為每次都需要登入

```{bash}
vim /etc/rstudio/rserver.conf
// 將 auth-none=1 註釋掉。
```

再 docker restart container 即可。

p.s. 重啟的時候，/etc/environment 環境變數，可能會重複，但是沒差。

---

## 設定使用者與群組登入:**

**reference:**

- [RStudio Server Professional Edition 1.4.693-1](https://docs.rstudio.com/ide/server-pro/latest/index.html)
- [3 Authenticating Users | RStudio Server Professional Edition 1.3.1056-1](https://docs.rstudio.com/ide/server-pro/authenticating-users.html#Restricting-Access-to-Specific-Users)

我還沒有設定過，但是上面的官方手冊教學應該可以成功。

---

## END
