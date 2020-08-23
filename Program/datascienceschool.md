# datascienceschool

> datascienceschool/rpython

記錄我在 VCS 上操作 container:datascienceschool/rpython 的過程，以及一些問題要如何克服。  
部分是從 DockerHub > Program > datascienceschool/rpython 複製過來。

這個 image 裡面有 Ubuntu, R, Python, Rstudio, postgres, jupyter notebook, ssh等等，  <br>
是很大一包的 image，共18G。  
自己沒有在 VCS 上面啟動這個 container，但是有幫同事在團隊的 server 打開。

--

**Content:**

<!-- TOC -->

- [datascienceschool](#datascienceschool)
  - [啟動 comtainer](#啟動-comtainer)
  - [改密碼](#改密碼)
  - [END](#end)

<!-- /TOC -->

--

**reference:**

- [datascienceschool/rpython - Docker Hub](https://hub.docker.com/r/datascienceschool/rpython/)
- [datascienceschool/docker_rpython: dockerfile for datascienceschool/rpython2 and datascienceschool/rpython3](https://github.com/datascienceschool/docker_rpython)

---

## 啟動 comtainer

```{bash}
docker run \
--name=rpython \
-p 8787:8787 \
-v e:\container_folder\rpython:/home/dockeruser/rpython \
-dit datascienceschool/rpython

docker run --name=rpython \
-p 8787:8787 \
-dit datascienceschool/rpython
```

因為其 docker hub 沒有寫 Docker file 的資訊，  <br>
所以後來我是去 github 找，  <br>
發現在 docker_rpython/02_rpython 目錄下，supervisord.conf 檔案中有做設定。

```{Dockerfile}
[program:rserver]
command=/usr/lib/rstudio-server/bin/rserver --auth-none 1 --server-user USER_ID --server-app-armor-enabled 0
stdout_logfile=/var/log/supervisor/%(program_name)s.log
stderr_logfile=/var/log/supervisor/%(program_name)s.log
startsecs=0
autorestart=false
user=USER_ID
```

所以要去修改 supervisord.conf 檔案的設定。  <br>
發現 docker_rpython/02_rpythona/Dockerfile 有標示這個檔案在哪，`/etc/supervisor/supervisord.conf`。

因此，如果不想要自動登入，就去改成 `--auth-none 0`，  <br>
重啟 container 即可。

---

## 改密碼

就登入之後，到 Rstudio > Tools > Shell

```{bash}
passwd
```

---

## END
