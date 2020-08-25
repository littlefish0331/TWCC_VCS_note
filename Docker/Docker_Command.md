# Docker Command

簡單記錄使用過的 docker 指令功用，有點類似 cheet sheet。  <br>

---

## docker

- docker version: docker 的 version。  <br>

- docker run: Run a command in a new container
  - docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
  - -i, --interactive                    Keep STDIN open even if not attached
  - -d, --detach                         Run container in background and print container ID
  - -t, --tty                            Allocate a pseudo-TTY

- docker pull: Pull an image or a repository from a registry
  - Usage:  docker pull [OPTIONS] NAME[:TAG|@DIGEST]
  - -q, --quiet                   Suppress verbose output

- docker load: Load an image from a tar archive or STDIN
  - Usage:  docker load [OPTIONS]
  - -i, --input string   Read from tar archive file, instead of STDIN
  - -q, --quiet          Suppress the load output

- docker import: Import the contents from a tarball to create a filesystem image
  - Usage:  docker import [OPTIONS] file|URL|- [REPOSITORY[:TAG]

- docker build: Build an image from a Dockerfile
  - Usage:  docker build [OPTIONS] PATH | URL | -

- docker images: List images
  - Usage:  docker images [OPTIONS] [REPOSITORY[:TAG]
  - -a, --all             Show all images (default hides intermediate images)
  - -q, --quiet           Only show numeric IDs

- docker ps: List containers
  - Usage:  docker ps [OPTIONS]
  - -a, --all             Show all containers (default shows just running)
  - -q, --quiet           Only display numeric IDs

- docker start: Start one or more stopped containers
  - Usage:  docker start [OPTIONS] CONTAINER [CONTAINER...]
  - example: docker start <container name or container ID>

- docker stop: Stop one or more running containers
  - Usage:  docker stop [OPTIONS] CONTAINER [CONTAINER...]
  - example: docker stop <container name or container ID>

- docker rm: Remove one or more containers
  - Usage:  docker rm [OPTIONS] CONTAINER [CONTAINER...]
  - example: docker rm <container name or container ID>

- docker exec: Run a command in a running container
  - Usage:  docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
  - example: docker exec <container name or container ID>
  - -i, --interactive          Keep STDIN open even if not attached
  - -t, --tty                  Allocate a pseudo-TTY
  - -w, --workdir string       Working directory inside the container
  - bash|sh: 用甚麼 command mode 去進入操作 container。

- docker logs: Fetch the logs of a container
  - Usage:  docker logs [OPTIONS] CONTAINER
  - example: docker logs <container name or container ID>
  - -f, --follow         Follow log output

- docker rmi: Remove one or more images
  - Usage:  docker rmi [OPTIONS] IMAGE [IMAGE...]

- docker export: Export a container's filesystem as a tar archive
  - Usage:  docker export [OPTIONS] CONTAINER

- docker stats

- docker commit

---

## docker-compose

Define and run multi-container applications with Docker.  <br>
除了可以用 .yml 腳本啟動之外，其他我覺得和一般的 docker 指令差不多。

- Usage: docker-compose [-f <arg>...] [options] [COMMAND] [ARGS...]
- Options:
  - -f, --file FILE             Specify an alternate compose file (default: docker-compose.yml)
  - -p, --project-name NAME     Specify an alternate project name (default: directory name)
  - --verbose                   Show more output  <br>
- Commands:
  - build              Build or rebuild services
  - create             Create services
  - down               Stop and remove containers, networks, images, and volumes
  - exec               Execute a command in a running container
  - images             List images
  - kill               Kill containers
  - port               Print the public port for a port binding
  - ps                 List containers
  - pull               Pull service images
  - push               Push service images
  - restart            Restart services
  - rm                 Remove stopped containers
  - run                Run a one-off command
  - scale              Set number of containers for a service
  - start              Start services
  - stop               Stop services
  - top                Display the running processes
  - unpause            Unpause services
  - up                 Create and start containers
  - version            Show the Docker-Compose version information

---

## 參考資料 - cheatsheet

更多請去 cheatsheet/ 資料夾觀看。

- [wsargent/docker-cheat-sheet: Docker Cheat Sheet](https://github.com/wsargent/docker-cheat-sheet)
- [docker_cheatsheet_r3v2.pdf](https://design.jboss.org/redhatdeveloper/marketing/docker_cheatsheet/cheatsheet/images/docker_cheatsheet_r3v2.pdf): 裡面還有 Dockerfile 的指令。
- [The Ultimate Docker Cheat Sheet | dockerlabs](http://dockerlabs.collabnix.com/docker/cheatsheet/)

---

## windows Docker 群組權限

使用 windows 系統下的 Docker 時，  <br>
會預設建立一個 docker group，所以要把使用者加到那個群組。

- [針對 Windows 上的 Docker 用戶端錯誤進行疑難排解 - Visual Studio | Microsoft Docs](https://docs.microsoft.com/zh-tw/visualstudio/containers/troubleshooting-docker-errors?view=vs-2019)

windows + x > 電腦管理 > 本機使用者和群組 > 群組 > docker-users。  <br>
到這裡面做設定。

---

## 本檔案紀錄的部分

- 下載與設定
  - docker
  - 加入 docker 帳號到群組
  - DockerHub login
  - docker-compose
- container 操作
  - 啟動
  - 重啟
  - 停止
  - 進入 container
  - docker start $(docker ps -a -q): [Command for restarting all running docker containers? - Stack Overflow](https://stackoverflow.com/questions/38221463/command-for-restarting-all-running-docker-containers)

---

## 下載與設定

### docker download

- [Linux 修改使用者帳號設定 - usermod](https://www.opencli.com/linux/usermod-modify-linux-account)

兩種方法其實是一樣的。

```{bash}
// 這是我從 社群好友 - 紙鈔(money)，那邊學的。
// 從官方網站下載，然後以 shell 執行。
curl -sSL https://get.docker.com/ | sh

// 官方 docker Github 作法
// 先從官方網站下載，儲存檔名為 get-docker.sh
// 再用 sh 執行。
// -o, --output <file> Write to file instead of stdout
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

### 加入 docker 帳號到群組

因為 Docker 安裝後，會建立一個 docker 帳號和群組。  <br>
如果沒有把 docker 帳號加入群組，就會每次使用 docker 指令都需要 sudo，  <br>
為了直接使用 docker 指令，所以要把 docker 加入 ubuntu 群組中。  <br>

- 當使用 "-G" 參數時, usermod 會將帳號從原來加入了的群組退出, 所以在 "-G" 參數前加入 "-a" 參數, 會保留原來的群組設定。
- 記得要重新登入。

```{bash}
sudo usermod -aG docker ubuntu
```

--

### DockerHub login

```{bash}
docker login

  > littlefish0331
  > 去看 myPC的密碼.txt
```

--

### docker-compose download

- [Install Docker Compose | Docker Documentation](https://docs.docker.com/compose/install/)
- [linux下chmod +x的意思？为什么要进行chmod +x_yunlive的博客-CSDN博客](https://blog.csdn.net/u012106306/article/details/80436911)

```{bash}
// 下載 docker-compose
sudo curl -L "https://github.com/docker/compose/releases/download/1.26.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

// 讓 docker-compose 變更為執行檔。
sudo chmod +x /usr/local/bin/docker-compose

// 查看 docker-compose 版本。
docker-compose version
```

---

## END

不過我覺得 如果你看一下 MSSQL 那個 container
你下這個
docker inspect <CONTAINER ID>

我們從 R 這裏
ip 改你看到的這個應該也可以

可能就不用開自己




`sudo usermod -a -G docker <username>`: 設定 user 可以執行 Docker。好像下一次登入才會生效。(亦可寫成`sudo usermod -aG docker ubuntu`)。將目前使用者加到docker群組。  <br>
`sudo service docker start`: 啟動docker服務。有其他方法，比如用systemctl啟動docker，詳見 `tutroial - others > phoenixnap website > 在 ubuntu 上安裝 Docker`。

docker pull
docker run
docker ps
docker exec
docker stop
docker start
docker stats

- docker start $(docker ps -a -q): [Command for restarting all running docker containers? - Stack Overflow](https://stackoverflow.com/questions/38221463/command-for-restarting-all-running-docker-containers)


---------------------
docker network ls

systemctl -a | grep docker

named volume
	docker volume create
	docker volume ls
Host Volume

資訊架構(IA, Information Architecture)
人工AI


