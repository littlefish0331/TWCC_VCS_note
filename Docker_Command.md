# Docker Command

簡單記錄使用過的 docker 指令，有點類似 cheet sheet。  <br>

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

## Docker 安裝 + 初始化步驟

```{bash}
curl -sSL https://get.docker.com/ | sh
sudo usermod -aG docker ubuntu  //記得要重新登入
```

If you would like to use Docker as a non-root user, you should now consider adding your user to the "docker" group with something like: `sudo usermod -aG docker ubuntu`

Remember that you will have to log out and back in for this to take effect!

WARNING: Adding a user to the "docker" group will grant the ability to run containers which can be used to obtain root privileges on the docker host. Refer to https://docs.docker.com/engine/security/security/#docker-daemon-attack-surface for more information.

---

## 連結 docker hub + 下載 image

連結登入 Docker Hub

```{bash}
docker login
```

下載 DockerCon範例

```{bash}
docker pull littlefish0331/hello-world
docker images
docker run -p 8080:80 --name DockerCon -d littlefish0331/hello-world
```

---

## MySQL

- [mysql - Docker Hub](https://hub.docker.com/_/mysql?tab=description)

如果連動的資料夾沒有建立，docker會自動建立幫忙建立該路徑的資料夾。  <br>
如果不想要有密碼問題就 pull 5.7.31版的 MySQL。

密碼無法登入的問題，詳見 Self_Command_History.md。

```{bash}
docker run --detach \
--name some-mysql \
-v /datamount/mysql/data:/var/lib/mysql \
-v /datamount/mysql/conf:/etc/mysql/conf.d \
-p 3306:3306 \
--env MYSQL_ROOT_PASSWORD=DAS@mysql2020 \
mysql:latest

//也可以只打一行
docker run --detach --name some-mysql -v /datamount/mysql/data:/var/lib/mysql -v /datamount/mysql/conf:/etc/mysql/conf.d -p 3306:3306 --env MYSQL_ROOT_PASSWORD=DAS@mysql2020 mysql:latest
```

---

## Rstudio

- [datascienceschool/rpython - Docker Hub](https://hub.docker.com/r/datascienceschool/rpython/): Ubuntu 18.04 + Python 3.7 (Anaconda3-2019.03) + R-3.6.1 + rstudio-server 1.2.1335 + Databases(PostgreSQL, Redis) + Tools(git, emacs, tex-live, pandoc, graphviz, imagemagick)、Running Services(jupyter notebook (port 8888), R-studio server (port 8787), ssh (port 22))。其實只需要 r, rstudio, python

```{bash}
docker run --name=rpython -dit -v e:\container_folder\rpython:/home/dockeruser/rpython -p 8787:8787 datascienceschool/rpython
```

---

## END
