# Install Docker and DockerCompose

本節教學如何在 VCS 上面安裝 docker、docker-compose，以及將 docker 使用者加到 ubuntu 群組中。

--

**Content:**

<!-- TOC -->

- [Install Docker and DockerCompose](#install-docker-and-dockercompose)
  - [下載](#下載)
    - [docker](#docker)
    - [docker-compose](#docker-compose)
  - [加入 docker 帳號到群組](#加入-docker-帳號到群組)
  - [END](#end)

<!-- /TOC -->

---

## 下載

### docker

兩種方法其實是一樣的。

這是我從社群朋友 - 紙鈔(money)，那邊學的。  
從官方網站下載，然後以 shell 執行。

```{bash}
curl -sSL https://get.docker.com/ | sh
```

官方 docker Github 作法，  
先從官方網站下載，儲存檔名為 get-docker.sh，  
再用 sh 執行。

- -o, --output <file> Write to file instead of stdout

```{bash}
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
```

> 會有一個指令提醒使用者權限與群組問題
>
> If you would like to use Docker as a non-root user, you should now consider adding your user to the "docker" group with something like: `sudo usermod -aG docker ubuntu`
>
> Remember that you will have to log out and back in for this to take effect!
>
> WARNING: Adding a user to the "docker" group will grant the ability to run containers which can be used to obtain root privileges on the docker host. Refer to https://docs.docker.com/engine/security/security/#docker-daemon-attack-surface for more information.

--

### docker-compose

```{bash}
// 下載 docker-compose
sudo curl -L "https://github.com/docker/compose/releases/download/1.26.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

// 讓 docker-compose 變更為執行檔。
sudo chmod +x /usr/local/bin/docker-compose

// 查看 docker-compose 版本。
docker-compose version
```

---

## 加入 docker 帳號到群組

因為 Docker 安裝後，會建立一個 docker 帳號和群組。  <br>
如果沒有把 docker 帳號加入群組，就會每次使用 docker 指令都需要 sudo，  <br>
為了直接使用 docker 指令，所以要把 docker 加入 ubuntu 群組中。

- 當使用 "-G" 參數時, usermod 會將帳號從原來加入了的群組退出, 所以在 "-G" 參數前加入 "-a" 參數, 會保留原來的群組設定。
- 記得要重新登入。

```{bash}
sudo usermod -aG docker ubuntu
```

---

## END
